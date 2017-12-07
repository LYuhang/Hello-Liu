# Linux磁盘与文件系统管理
[TOC]
December 7, 2017 11:09 PM
_ _ _

#### 1.实体磁盘与虚拟磁盘的文件名
- /dev/sd\[a-p]\[1-128]：为实体磁盘的磁盘文件名；
- /dev/vd\[a-d]\[1-128]：为虚拟磁盘的磁盘文件名；

#### 2.文件系统特性
首先要将分区进行格式化，以成为操作系统能够利用的“文件系统格式”。
文件系统分为三块内容：inode，data block，superblock。
- **superblock**：记录filesystem的整体信息，包括inode/block的总量、使用量、剩余量等等，以及文件系统的格式与相关信息等。
- **inode**：记录文件的属性，一个文件占用一个inode，这个inode同时记录数据所在的block号码。
- **block**：记录文件的实际内容，若文件太大，会占用多个block。一个data block的大小有1K，2K及4K三种。
**说明**：
 * 读取文件时，直接读取文件的inode信息中的该文件数据部分所在的block编号，然后根据编号读取文件内容。
 * 文件系统在格式化的时候基本上区分为多个区块群组，每个区块群组都有独立的inode/block/superblock系统。并且已经规划好了各个部分的数量与内存编号。
 * 在block里面进行存取数据时，每个block至多可以存一个文件，如果文件大小小于一个block大小，那么该block会有空间浪费。

#### 3.inode
- inode table
inode记录的文件数据至少有：
 * 该文件的存取模式（read/write/excute）
 * 该文件的拥有者与群组（owner/group）
 * 该文件的容量
 * 该文件创建或状态改变的时间（ctime）
 * 最近一次的读取时间（atime）
 * 最近修改的时间（mtime）
 * 定义文件特性的旗标（flag），如 SetUID...
 * 该文件真正内容的指向 （pointer）
- 说明
 * inode 的数量与大小也是在格式化时就已经固定了
 * 每个 inode 大小均固定为 128 Bytes/256 Bytes
 * 每个文件都仅会占用一个 inode 而已，因此，文件系统能够创建的文件与inode数量有关系
- inode结构示意图如下所示：
![inode结构示意图](http://p08dasr0h.bkt.clouddn.com/17-12-7/35214352.jpg)

#### 4.superblock
- **superblock**记录的信息主要有：
 * block 与 inode 的总量
 * 未使用与已使用的 inode / block 数量
 * block 与 inode 的大小 （block 为 1, 2, 4K，inode 为 128Bytes 或 256Bytes）
 * filesystem 的挂载时间、最近一次写入数据的时间、最近一次检验磁盘（fsck） 的时间等文件系统的相关信息
 * 一个 valid bit 数值，若此文件系统已被挂载，则 valid bit 为 0 ，若未被挂载，则 valid bit为 1

#### 5.Filesystem Description（文件系统说明）
这个区段可以描述每个 block group 的开始与结束的 block 号码，以及说明每个区段（superblock, bitmap, inodemap, data block）分别介于哪一个block 号码之间。

#### 6.block bitmap（区块对照表）
block bitmap中记录的是哪些block是空的，哪些block是已经被文件占用了的，当写入数据需要寻找空的block的时候，就可以通过block bitmap进行寻找空的block。
如果删除某一个区块的数据，那么bitmap就会将该区块的标志设定为未使用中。

#### 7.inode bitmap
和block bitmap有相似的功能，主要是这个记录的是inode bitmap记录的是使用了的和未使用的inode号码。

#### 8.dumpe2fs
**作用**：查询Ext2/3/4文件系统中superblock信息的指令。
**使用方式**：
```
$ dumpe2fs [-bh] 设备文件名
```
- **-b**：列出保留为坏轨的部分（一般用不到）。
- **-h**：仅列出superblock的数据，不会列出其他的区段的内容。

**查询结果**：查询结果可以分为两部分：上半部分是superblock的内容，下半部分是每个block group的信息。

#### 9.文件系统与目录树的关系
目录是记录文件的文件名，inode记录对应的文件的属性，一般文件才是记录数据内容的地方。
- 目录
当创建一个目录时，文件系统会分配一个inode与至少一块block给该目录。
 * inode ：记录该目录的属性与权限，并可记录分配的那块block号码。
 * block ：记录在该目录下的文件名与该文件名占用的inode号码数据。
 * `ls [-i] 文件夹`：观察文件文件夹下的文件所占用的inode号码。
- 文件
在创建一个文件时，文件系统会分配一个inode与对应大小的block数量给该文件。
- 目录树读取
 * `ll [-di] 文件夹`：列出该文件夹的inode号码，权限等信息（具体不清楚，有待深入研究）
 * 读取的过程：比如读取/etc/passwd文件
 Step1：通过挂载点信息找到根目录的inode号码，检查权限（有r和x）可以读取该目录下的文件名信息。
 Step2：根据上面步骤得到block号码，找到etc/目录的号码。
 Step3：一层一层地检查权限，读取目录的文件名信息。
 Step4：最后得到文件的inode号码，检查文件的权限情况，如果可以读取，那么访问区块号码，读取block内容。

#### 10.EXT文件系统文件的存取与日志式文件系统的功能
- 新建一个文件的步骤：
 * 确认新增文件的目录是否有w和x的权限
 * 根据inode bitmap寻找未使用的inode号码分配给文件，将属性写入
 * 根据block bitmap寻找未使用的block号码，将实际的数据写入block，更新inode。
 * 将数据同步到inode bitmap和block bitmap，更新superblock内容。

但当文件写入过程中，系统突然死机或突然断电等突发情况发生导致文件写入异常中断。那么就会出现会导致inode bitmap和block bitmap和superblock的数据未更新，与实际数据区的内容不匹配的问题。
- 日志式文件系统
**在filesystem中规划处一个区块专门用于记录写入或修订文件时的步骤，那么系统检查时可以直接检查日志就很快判断**


#### 11.挂载点（mount point）的意义
**挂载**：将文件系统与目录树结合的动作称为挂载。
- XFS文件系统
 * 文件系统配置：数据分布上分为三个部分，一个数据区（data section），一个文件系统活动登录区（log section）以及一个实时运行区（realtime section）。
 * 数据区
 数据区和之前的ext文件系统很相似，包括inode/data block/superblock等部分，其中inode与block都是系统需要用时进行动态分配的。
 * 文件系统活动登录区
 有点像日志区。
 * 实时运行区
 当创建一个文件时，现在该部分找到数个extent区块将文件放置在这个区块内，等到分配完毕后，再写入inode与block去。extent区域在格式化是就已经指定了。

#### 12.文件系统的简单操作
- df：列出文件系统的整体磁盘使用量
```
$ df [-ahikHTm] [文件或目录名]
```
 * **-a**：列出所有的文件系统
 * **-k**：以 KBytes 的容量显示各文件系统
 * **-m**：以 MBytes 的容量显示各文件系统
 * **-h**：以人们较易阅读的 GBytes, MBytes, KBytes 等格式自行显示
 * **-H**：以 M=1000K 取代 M=1024K 的进位方式
 * **-T**：连同该 partition 的 filesystem 名称 （例如 xfs） 也列出
 * **-i**：不用磁盘容量，而以 inode 的数量来显示
- du：直接到文件系统内去搜寻所有的文件数据
```
$ du [-ahskm] 文件或目录名称
```
 * **-a**:列出所有的文件与目录容量
 * **-h**:以人们较易读的容量格式 （G/M） 显示
 * **-s**:列出总量而已，而不列出每个各别的目录占用容量
 * **-S**:不包括子目录下的总计，与 -s 有点差别
 * **-k**:以 KBytes 列出容量显示
 * **-m**:以 MBytes 列出容量显示