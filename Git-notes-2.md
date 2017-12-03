#Git-notes-2
[TOC]
_ _ _
### 1.git commit -a
**作用**：跳过使用暂存区域。在提交的时候自动把所有已经跟踪过的文件暂存起来一并提交，从而跳过git add操作。
**例如**：
```
$ git commit -a -m 'added new benchmarks'
```
### 2.git rm
**作用**：将某文件从已跟踪的文件清单中移除（从暂存区域移除），然后提交。
**注意**：
Situation1：该命令连带从工作目录中删除指定的文件，这样不会出现在未跟踪文件清单中。
如果简单的从工作目录手工删除文件，那么运行git status时显示该文件在*未暂存清单*中，然后运行git rm记录此次的移除文件操作。
另外，如果删除之前修改过并且已经放到暂存区了，那么就要用强制删除选项-f。
Situation2：如果想将文件从Git仓库中移除，但是仍然希望保留在当前目录，即仅仅移除清单中的文件，文件还在，现在要移除跟踪但不删除，那么就用--cached选项。
```
$ git rm --cached readme.txt
```
后面可接文件及目录名，也可以使用[glob模式](https://baike.baidu.com/item/glob模式/8290305?fr=aladdin).
例如：
```
$ git rm log/\*.log[^log]
```

[^log]: 此处用到了反斜杠 **\** 这个是git的文件扩展匹配方式，该处指定了目录，和shell中不用斜杠（\）的效果相同，都是只删除该目录下的文件。但是如下例：
```
$ git rm \*~
```
这个会递归删除该目录及子目录下的以~结尾的文件。
### 3.git mv [old_file_name] [new_file_name]
**功能**：将文件改名。
### 4.git log
**功能**：查看提交的历史。
**说明**：
Situation1：默认不用任何参数的话，git log会按提交时间列出所有的更新。
Situation2：使用 **-p** 选项展示每次提交的内容差异，用-2则显示近两次的更新。例如如下代码：
```
$ git log -p -2
```
Situation3：使用 **--stat** 命令，仅显示简要的增改行数统计。例如如下代码：
```
$ git log --stat
```
每次修改都列出了修改过的文件，增加和移除的行数统计，以及最后的总行数统计。
Situation4：使用 **--pretty** 选项，相当于能够使用自定义模式，例如使用oneline将每个提交显示为一行。如下例子：
```
$ git log --pretty=oneline
```
**format**选项，可以定制自己想要显示的模式，，这样便于之后进行编程提取分析。例如如下例子：
```
$ git log --pretty=format:"%h - %an,%ar : %s"
```