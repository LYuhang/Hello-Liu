# Linux-FileAccess&ContentSettings
[TOC]
_ _ _

#### 1.文件的使用者与群组概念
- 文件拥有者：文件的编辑与保存者
- 群组概念：适用于团队开发资源的时候，那么每个成员都可以访问资源数据。
- 其他人概念：外来的访问者。
**注意**：在Linux系统中，默认的情况下，系统上的账号与一般使用者，还有root相关信息，都记录在/etc/passwd文件内，个人密码则记录在/etc/shadow文件内。此外，Linux所有群组名称记录在/etc/group内。

#### 2.ls
**用途**：查看文件的属性，显示文件名和相关属性。一般先用su切换为root身份再查看。比如运行以下代码：
```
$ ls -al
```
那么会列出当前目录下的文件信息。
**显示结果各部分含义**：
\[d] \[r-x] \[r-x] \[---]
\[ 权限 ] \[ 链接 ] \[ 拥有者 ] \[ 群组 ] \[ 文件大小 ] \[ 修改日期 ] \[ 文件名 ]
- 第一栏：代表文件类型与权限，该处共有10个字符。可以分为四部分：\[\-]\[\-\-\-]\[\-\-\-]\[\-\-\-]。
*第一个字符*：表示文件的类型，有以下的选项：
 * **[d]**：则是目录。
 * **[-]**:表示文件。
 * **[l]**：表示链接文件。
 * **[b]**：表示设备里可供储存的周边设备。
 * **[c]**：表示设备文件里面的序列埠设备，例如键盘，鼠标。

 *接下来的字符三个一组，且均为**rwx**组合*：**[r]**表示可读权限、**[w]**表示可写、**[x]**表示可执行权限。这三个位置不会改变，如果没有对应权限，那么该处就是**[-]**。
 * 第一组为“文件拥有者可具备的权限”。
 * 第二组为“加入此群组的账号的权限”。
 * 第三组为“非本人且没有加入本群组的其他人账号的权限”。

- 第二栏：表示有多少文件名链接到此节点（i-node）：
每个文件都会将该文件的属性和权限记录到文件系统的i-node中。

- 第三栏：表示这个文件（或目录）的“拥有者账号”
- 第四栏：表示这个文件所属群组
在Linux系统中，一个账号会加入一个或多个所属群组。

- 第五栏：这个文件的容量大小，默认单位是Bytes。
- 第六栏：这个文件的创建日期或者最近一次的修改日期。
 * ls -l --full-time：显示完整的时间格式。

- 第七栏：这个文件的文件名

#### 3.改变文件的属性与权限
|   指令   |   作用    |
|:--------:|:--------:|
|chgrp     |改变文件的所属群组|
|chown     |改变文件的拥有者 |
|chmod     |改变文件的权限，SUID，SGID，SBIT等等特性|

- chgrp：改变文件的群组
**例子**：
```
$ chgrp [-R] group1 test.txt
```
其中 **-R** 为可选参数，表示进行递归变更。这个指令将该文件的群组改成了group1,。
**注意**：群组的名称必须要在/etc/group中存在，否则会出现错误。

- chown：改变文件的拥有者
**注意**：使用者必须是已经存在系统中的账号，也就是/etc/passwd这个文件中的有记录的使用者的名称。
```
$ chown [-R] 账号名称 文件或目录
```
或者同时改变所属者与群组，用：隔开
```
$ chown [-R] 账号名称：群组名称 文件或目录
```
**user**与**group**之间可以用**.**隔开，但是容易和账号中的小数点混淆。当只改变群组时，可以使用：
```
$ chown [-R] .群组名称 文件或目录
```
**注意**：当把一个文件复制给别人时，一定要注意改变文件的拥有者，因为复制行为会将权限也复制过去。

- chmod
改变文件的权限有两种方法：
 * 数字类型改变文件权限、
 `r`对应4，`w`对应2，`x`对应1。那么权限值累加就是总的权限值了。例如：`777`表示每个身份（owner/group/others）的权限是`rwxrwxrwx`。
 例如如下代码：
 ```
 $ chmod [-R] xyz 文件或目录
 ```
 其中`xyz`就是上述的数字类型的权限属性。
 * 符号类型改变文件权限
 借用**u**，**g**，**o**（即user、group、others的首字母）来代表三种身份的权限，**a**表示全部的身份。格式为：
`| chmod |u g o a| +(加入) -(除去) =(设置) | r w x| 文件或目录 |`
比如设置test文件的权限为`-rwxr-xr-x`。如下代码：
```
$ chmod u=rwx,go=rx test
```
或者给所有的身份都加上**写**的权限：
```
$ chmod a+w test
```

#### 4.文件与目录的权限区别
|   权限  |    文件  |     目录    |
|:-------:|:-------:|:-----------:|
|r(read)   |读取文件实际内容|具有读取目录结构的清单(ls)|
|w(write)  |可以编辑、新增或者修改文件内容|表示可以改变文件清单，包括：新建，重命名，删除，移动文件和目录。|
|x(execute) |表示是否能够进入该目录成为工作目录 |