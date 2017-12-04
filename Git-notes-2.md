#Git-notes-2
[TOC]
December 4, 2017 9:27 AM
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
如下表格显示常用格式占用符以及其代表的意义：

|      选项      |        说明         |
|:-------------:|:-------------------:|
| %H |      提交对象的完整的哈希字串(Hush)      |
| %h |      提交对象的简短的哈希字串      |
| %T |      树对象的完整哈希字串(Tree)        |
| %t |      树对象的简短哈希字串          |
| %P |      父对象的完整哈希字串(Parent)          |
| %p |      父对象的简短哈希字串          |
| %an|      作者的名字(author name)                  |
| %ae|      作者的电子邮件地址(author email)           |
| %ad|      作者的修订日期(author date)              |
| %ar| 作者的修订日期，按照多久以前的方式显示|
| %cn|       提交者的名字               |
| %ce|      提交者的电子邮件地址         |
| %cd|       提交日期                  |
| %cr|     提交日期，按照多久之间的格式显示|
| %s |           提交说明              |

Situation5：用oneline或format结合 **--graph** 选项，可以看到使用ASCII字符串表示的简单图形展示==每个提交所在的分支及其衍合情况==。（==此处不太懂==）
例如如下例子：
```
$ git log --pretty=format:"%h %s" --graph
```
**总结**:如下表格显示一些常用的选项及其释义：

|  选项  |  说明  |
|:----------:|:----------:|
| -p | 按补丁形式显示每个更新之间的差异 | 
| --stat |显示每次更新简短修改统计信息（增加或移除行数）|
| --shortstat | 只显示 --stat 中最后的行数添加和移除统计 |
| --name-only | 仅在提交信息后显示已修改的文件清单   |
| --name-status | 显示新增，修改，删除的文件清单 |
| --abbrev-commit | 仅显示 SHA-1 的前几个字符，而非所有的40个字符|
| --relative-date | 使用较短的相对时间显示 |
| --graph | 显示ASCII图形表示的分支合并历史 |
| --pretty | 使用其他格式显示历史的提交信息。 |

Situation6：限制输出长度。比如如下使用 **--since** 选项，显示最近两周的提交。
```
$ git log --since=2.weeks
```
|  选项  |  说明  |
|:----------:|:---------:|
| -(n) | 仅显示最近的**n**条提交|
| --since，--after |仅显示指定时间之后的提交 |
| --until，--before |仅显示指定时间之前的提交 |
| --author | 仅显示指定作者的相关提交 |
| --committer | 仅显示指定提交者的相关提交 |

比如如下例子：
```
$ git log --pretty="%h:%s" --author=gitster --since="2008-10-01" --before="2008-11-01" --no-merges -- t/
```
### 5.gitk工具
**用途**：gitk是一个可以查阅提交历史的可视化工具。上半个窗口显示历次提交的分支祖先图谱，下半个窗口显示当前选的对应的具体差异。
如下图：
![git界面](http://p08dasr0h.bkt.clouddn.com/17-12-4/7880567.jpg)

### 6.git commit --amend
**用途**：撤销刚才的提交操作，使用 **--amend** 选项进行重新提交。
**用法**：
如果提交说明写错了，刚才的提交没有做任何改动，那么可以直接运行`git commit --amend`命令，相当于重新写一遍提交说明。
如果刚才提交时忘了暂存某些修改或者忘了暂存某个文件，可以补上暂存操作，然后再运行该命令。
### 7.git reset HEAD < file >
**用途**：取消文件的暂存。例如以下例子取消对readme.txt文件的暂存。
```
$ git reset HEAD readme.txt
```
### 8.git checkout
**用途**：撤销对文件的更改。例如如下例子：
```
$ git checkout -- readme.txt
```
**注意**：该命令有危险，这条命令会用之前的版本来替换当前的文件，所以 ，当前的更改都被覆盖了，在运行该条命令前，**务必要确认真的不再需要之前的更改**。

### 9.git remote
**用途**：查看当前的远程仓库。origin为默认名。
- git remote
显示当前原始仓库的名称。
- git remote -v
显示当前的原始仓库以及对应的克隆地址。
- git remote add [shortname] [url]
该命令将相应的简短字串代替链接名称，比如如下例子中：
```
$ git remote add pb git://github/……
```
那么这个时候可以直接使用pb代替仓库地址了。

### 10.git fetch [remote-name]
**用途**：该命令会拉取远程仓库中本地仓库没有的数据到本地。比如如下例子：
```
$ git fetch origin
```
会拉取自上一次克隆或上一次fetch以来的更新到本地。
**注意**：git fetch只将数据拉取到本地，而不会自动将数据合并到本地仓库，需要手工合并。

### 11.git pull
**用途**：自动将远程仓库的数据抓取下来并合并到本地的仓库。

### 12.git push [remote-name] [branch-name]
**用途**：将本地仓库的数据推送到远程仓库。例如将本地的master分支推送到origin服务器上：
```
$ git push origin master
```
**注意**：如果在推送数据的同时，别人也在推送数据，那么请求会被驳回，必须要先将别人的更新拉取到本地，然后才能够推送到远程仓库。
### 13.git remote show [remote-name]
**用途**：显示远程仓库的详细信息。
**注释**：
*Local branch pushed with 'git push'*：表示直接使用`git push`命令时缺省的推送方式。
*New remote branch*：表示那些远程仓库还没有同步到本地。
*Stale tracking branch*：表示同步到本地的哪些分支在远端已经被删除。

### 14.git remote rename [old-name] [new-name]
**用途**：将远程仓库进行重命名。例如如下例子，将pb改名为paul：
```
$ git remote rename pb paul
```
### 15.git remote rm [name]
**用途**：将远端仓库移除。例如以下例子将远端仓库paul进行移除。
```
$ git remote rm paul
```
### 16.git tag
- git tag
直接显示现有的标签。
- git tag -1 [tag-module]
显示感兴趣的标签。例如如下例子：
```
$ git tag -1 'v1.4.2.*'
```
那么会列出所有以v1.4.2.开头的标签。
- git tag -a [tag-name] -m [appended-notes]
创建一个含附注的标签。例如如下例子：
```
$ git tag -a v1.4 -m 'my version 1.4'
```
其中-m指定了附注内容，如果没有-m，那么之后会启动编辑器供你输入。
- git show [tag-name]
显示相应标签的版本信息。
- git tag -s [tag-name] -m [appended]
使用GPG进行签署标签。
- git tag [tag-name]
直接添加轻量级的标签。
- git tag -v [tag-name]
验证标签。此命令会调用GPG来验证签名，所以需要签署者的公钥，才能验证。
- git push [branch-name] [tag-name]
由于默认情况下，标签不会随着git push被推送到远程仓库，所以需要通过显式命令。
例如如下代码：
```
$ git push origin v1.0
```
如果想一次性将所有标签推送上去，那么可以使用 **--tags** 选项。
```
$ git push origin --tags
```
### Tips
输入命令时，可以输入部分后连续按两次**Tab**键，那么会显示出可能给要输出的所有的命令。