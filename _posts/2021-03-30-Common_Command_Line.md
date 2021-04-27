---
layout: post
title: "🥞 常用命令行"
date: 2021-03-30 00:00:00
categories: [Linux]
tags: [Linux]
comments: true
---


<!--more-->

### Git

[git branch超棒教程！](https://learngitbranching.js.org/?locale=zh_CN)

[Git Reset 三种模式](https://www.jianshu.com/p/c2ec5f06cf1a)

<img src="/image/posts/20210427.png" style="display:block;margin:0 auto;">

`master` 引用通常指向主分支的最新依次提交

`HEAD` 该分支上的最后一次提交的快照

[如果当前分支所做的修改没有提交就切换去其他分支也会看到相同的修改](https://blog.csdn.net/frank_jb/article/details/109126509)

`git stash apply`：暂存的恢复在哪个分支下运行，就会把暂存的内容在当前分支下恢复

`git diff`：比较的是工作目录中当前文件和暂存区域快照之间的差异,也就是修改之后还没有暂存起来的变化内容.

`git diff -staged`：比对已暂存文件与最后一次提交的文件差异

`git log --oneline`：显示的是当前所在分支内的提交记录

`^`是`~`都是父节点，区别是跟随数字时候，`^2` 是第二个父节点（水平上查找）， `~2 ` 是父节点的父节点（一直向上查找）[HEAD^和HEAD~](https://www.imooc.com/wenda/detail/574774)

`git pull` 是 fetch 和 merge 的简写， `git pull --rebase` 就是 fetch 和 rebase 的简写


```shell

git push origin ${本地分支名}:${远程分支名}  # 即使没有该远程分支，也会自动创建该远程分支

git branch -v  # 查看每一个分支的最后一次提交

git checkout -b ${新建本地分支名} ${origin/要跟踪的远程分支名} # 新建本地分支并指定跟踪远程仓库中的分支

git branch -u ${要跟踪的远程分支名,如origin/main} ${本地分支名} # 设置远程追踪分支，如果当前就在该本地分支上，还可以省略本地分支名 即 git branch -u ${要跟踪的远程分支名,如origin/main}

git push ${远程服务器名字,如origin} ${远程分支名字} # 将本地分支的提交推送到同名的远程分支

git push ${远程服务器名字,如origin} ${本地分支名字}:${远程分支名字} # 本地分支提交推送到指定远程分支，如果你要推送到的目的分支不存在会怎么样呢？Git 会在远程仓库中根据你提供的名称帮你创建这个分支

git fetch origin ${远程分支名如foo} # Git 会到远程仓库的 foo 分支上，然后获取所有本地不存在的提交，放到本地的 origin/foo 上【注意：git fetch不会更新你的本地的非远程分支, 只是下载提交记录（这样, 你就可以对远程分支进行检查或者合并了）】

git fetch # 如果没有参数，它会下载所有的提交记录到各个远程分支

git pull origin foo # 等价于 git fetch origin foo ; git merge origin/foo

git fetch ${远程主机名} # 将某个远程主机的更新，全部取回本地.通常用来查看其他人的进程，因为它取回的代码对你本地的开发代码没有影响;

git fetch ${远程主机名} ${远程分支名字}:${本地分支名字} # 拉取某个远程分支的更新到某个本地分支

git fetch ${远程主机名} ${分支名} # 取回特定分支的更新

git checkout ${分支名} # 切换到某分支

git checkout ${某次提交的哈希值或相对} # HEAD指针指向到某此，HEAD变成游离状态（HEAD指针默认是志向分支指针）

git branch -f ${分支名} ${某次提交的哈希值或相对} # 移动分支指针

git reset HEAD~1 # 【退回到上一个提交】通过把分支记录回退几个提交记录来实现撤销改动。你可以将这想象成“改写历史”，git reset 向上移动分支，原来指向的提交记录就跟从来没有提交过一样，git log中不会出现。但是提交的变更还在，只是修改的文件处于未跟踪状态

git revert HEAD # 【退回到上一个提交】创建一个新的提交来抵消之前的提交

git push origin :foo # 如果 push 空 到远程仓库会如何呢？它会删除远程仓库中的分支

git fetch origin :bar # 如果 fetch 空 到本地，会在本地创建一个新分支bar

git reset HEAD ${file} # 将暂存区的文件变成 unstage

git reset --hard HEAD^ # 完全回退到HEAD^，暂存区以及修改了但没有暂存的都会被清除掉（当然untrack文件是不影响的，因为其没有被git跟踪）【 HEAD 和当前 branch 切到上一条commit 的同时，除untrack之外，其他改动一起全都消失】

git reset --soft HEAD^ # 保留工作目录和暂存区中的内容，并把重置 HEAD 所带来的新的差异放进暂存区

git reset # 如果不加参数，就是默认使用--mixed,工作目录的修改、暂存区的内容以及由 reset 所导致的新的文件差异，都会被放进工作目录【把暂存区清空,并把原节点和reset节点的差异的文件放在工作目录】

```
### Linux常用命令

[参考](https://www.cnblogs.com/huchong/tag/%E6%AF%8F%E5%A4%A9%E5%AD%A6%E4%B9%A0%E4%B8%80%E4%B8%AAlinux%E5%91%BD%E4%BB%A4/)

#### echo

[详细说明](https://www.runoob.com/linux/linux-shell-echo.html)

```shell

vim test.sh
# 输入 
read name
echo "$name,cone here"
#

# 执行test.sh
sh test.sh
# 输入 gerger,会输出gerger,come here

```

#### 查看文件

- **`cat`**：显示文件内容

```shell

cat ${filename}

cat -n ${filename}  # 带行号，从1开始编号（包括空行）

cat -b ${filename}  # 带行号，从1开始编号（不包括空行）

```

- **`more`**：分页显示文件内容

```shell

more ${filename}         # 每次显示一页，space继续显示下一页，enter显示下一行，b往回一页

more -${num} ${filename} # 一次显示num行

more -${num} ${filename} # 从第num行开始显示

```

- **`less`**：可以向前/向后滚动查看文件内容，按 q 键退出

```shell

less ${filename}     # 按[PageDown]向下翻动一页，按[PageUp]向上翻动一页

less -m ${filename}  #显示文件内容，并在屏幕底部显示已显示的百分比

```

- **`tail`**：显示文件尾部的内容

```shell

tail ${filename}             # 显示文件尾部10行内容

tail -n ${num} ${filename}   # 显示文件尾部的num行内容

tail -c ${num} ${filename}   # 显示文件尾部的num个字节内容

tail -f ${filename}      # 显示文件最尾部的内容，并且不断刷新，只要文件更新就可以看到最新的文件内容

```

- **`head`**：查看文件开头的内容

```shell

head ${filename}              # 默认情况下，只显示文件的头10行内容

head -n ${num} ${filename}    # 显示文件前num行

head -c ${num} ${filename}    # 显示文件内容的前num个字节

```

#### 文件详情 `ls -l`

```shell
ls -l  # 详细列出当前目录下文件或文件夹的信息

ls -l ${filename} # 列出filename的详细信息
```
<img src="/image/posts/2021033101.png" style="display:block;margin:0 auto;">

用`ls -l`命令查看某一个目录会得到一个7个字段的列表，total后面的数字表示当前目录下所有文件所占的空间总和。

| 文件属性 | 文件数 | 拥有者 | 拥有者所属Group | 文件大小(字节) | 文件(目录)最近访问(修改)时间 | 文件名 |

**第1个字段：文件属性**
<br />
由10个字母组成，第一个字符表示**文件类型，** -：普通文件；d：目录(dirtectory)；l：链接文件(link)；b：块设备文件(block)；c：字符设备文件(character)；p：命令管道文件；s：sock文件.<br />
剩余的9个字符每3个为一个单位，其中前三个表示文件拥有者的权限，中间三个表示文件所属组拥有的权限，最后三个表示其他用户拥有的权限。Linux/Unix 的文件调用权限分为三级 : 文件所有者（Owner）、用户组（Group）、其它用户（Other Users）
<img src="/image/posts/2021033102.png" style="display:block;margin:0 auto;">
**第2个字段：文件数**
<br />
如果是文件，这个数目是1；如果是目录的话，是该目录中的文件个数.

#### 移动/重命名  `mv`

```shell

mv [options] ${source_file} ${dest_file}

mv [options] ${source_file...} ${directory}

mv ${source_file(文件)} ${dest_file(文件)} # 源文件名source_file改为目标文件名 dest_file

mv ${cource_file(文件)} ${dest_directory} # 将文件source_file移动到目标目录 dest_directory

mv ${source_directory(目录)} ${dest_directory(目录)} # 目录名 dest_directory 已存在，将 source_directory 移动到目录名 dest_directory 中；目录名 dest_directory 不存在， source_directory 改名为目录名 dest_directory

mv ${source_directory(目录)} ${dest_file(文件)} # 错误

# Example:
mv /usr/runoob/*  . # 将 /usr/runoob 下的所有文件和目录移到当前目录下

```
**-b**: 当目标文件或目录存在时，在执行覆盖前，会为其创建一个备份<br />
**-i**: 如果指定移动的源目录或文件与目标的目录或文件同名，则会先询问是否覆盖旧文件，输入 y 表示直接覆盖，输入 n 表示取消该操作<br />
**-f**: 如果指定移动的源目录或文件与目标的目录或文件同名，不会询问，直接覆盖旧文件<br />
**-n**: 不要覆盖任何已存在的文件或目录<br />
**-u**：当源文件比目标文件新或者目标文件不存在时，才执行移动操作<br />

#### 复制  `cp`
```shell

cp [options] ${source...} ${dest}

# Example
cp –r test/ newtest  # 将当前目录 test/下的所有文件复制到新目录newtest下【注意：复制目录下所有文件必须使用参数 -r 或者 -R 】

```

**-a**：此选项通常在复制目录时使用，它保留链接、文件属性，并复制目录下的所有内容。其作用等于dpR参数组合<br />
**-d**：复制时保留链接。这里所说的链接相当于Windows系统中的快捷方式<br />
**-f**：覆盖已经存在的目标文件而不给出提示<br />
**-i**：与-f选项相反，在覆盖目标文件之前给出提示，要求用户确认是否覆盖，回答"y"时目标文件将被覆盖<br />
**-p**：除复制文件的内容外，还把修改时间和访问权限也复制到新文件中<br />
**-r**：若给出的源文件是一个目录文件，此时将复制该目录下所有的子目录和文件<br />
**-l**：不复制文件，只是生成链接文件<br />

#### 新建 `mkdir`
```shell

mkdir [-p] ${dirName}  # -p 确保目录dirName存在，不存在的就建一个

# Example
mkdir -p test1/test2  # 在工作目录下的 test1 目录中新建名为 test2 的子目录，若 test1 目录原本不存在，则建立一个【注意：不加 -p 参数且 test1 目录不存在，报错】

```

#### 删除 `rm`
```shell

rm ${filename}      # 直接删除文件

rm -i ${filename}   # 删除文件前询问确认，y删除，n不删除

rm -f ${filename}   # 即使原文件权限设为只读，亦直接删除，无需逐一确认

rm -r ${dirname}    # 删除目录 dirname及里面的文件
 
rm -rf ${dirname}   # 【!!!慎重使用】删除目录以及目录里的东西
```

#### 修改权限 `chmod`

[Linux chmod命令](https://www.runoob.com/linux/linux-comm-chmod.html)

```shell

chmod 777 ${filename}     # 给filename目文件设为任何人可读可写可执行

chmod -R 777 ${dir_name}  # 给dir_name目录下的所有文件与子目录皆设为任何人可读可写可执行

chmod abc ${filename}     # a,b,c各为一个数字，分别表示User、Group、及Other的权限,r=4，w=2，x=1

```

#### vi/vim
```shell

vim test.txt   # 进入 vi 的一般模式。vim 后面一定要加文件名，不管该文件存在与否，存在则打开，不存在则新建

# 按下 i 进入输入模式(也称为编辑模式)，开始编辑文字，键盘上除了 Esc 这个按键之外，其他的按键都可以视作为一般的输入按钮进行任何的编辑。

# 按下 ESC 按钮回到一般模式

# 在一般模式中按下 `:wq` 储存后退出 【:q  退出  :w 保存】

```

#### touch

用于修改文件或者目录的时间属性，包括存取时间和更改时间。若文件不存在，系统会建立一个新的文件

```shell

touch ${filename}  #修改文件时间属性为当前系统时间,若filename文件不存在，则会建立一个新的空白文件filename

```


#### 查找文件 `find/locate/which/whereis`

> `find`: 实际搜寻硬盘查询文件名称<br />
> `locate`: 配合数据库查看文件位置<br />
> `which`: 查看可执行文件的位置<br />
> `whereis`: 查看文件的位置

+ **`find`**

[详细教程](https://www.cnblogs.com/jiftle/p/9707518.html)
```shell

# 查找文件
find . -name "*.sh"  # 查找当前目录（包括子目录）下所有所有.sh 文件

#查找目录
find . -name 'tes*' -type d # 查找当前目录（包括子目录）下所有包含tes名称前缀的文件夹（显示目标文件夹的路径，如：./testDir1、./testDir、./testDir/testchildren）

find /home -name "*.txt" # 在/home目录下查找以.txt结尾的文件名

find /home -iname "*.txt" # 同上，但忽略大小写

```

+ **`locate`**

locate 命令无需指定路径，直接搜索，locate 的搜索速度远快于 find 命令。locate不是直接去系统的各个角落搜索文件，而是在一个叫 mlocate.db 的数据库下搜索。locate 命令在找到文件之后，将直接显示该文件的绝对路径。**locate 命令有个弊端，它无法搜索当天所创建的文件**，因为它的数据库一天只在早上更新一次。
```shell

locate test1.sh # /home/gehuiling/Code/test2/test1.sh

```
+ **`which`**

[详细教程](https://www.cnblogs.com/huchong/p/9938426.html)
which指令会在PATH变量指定的路径中，搜索某个系统命令的位置，并且返回第一个搜索结果。
```shell

which pwd     # /bin/pwd

which java    # /usr/local/jdk1.8.0_271/bin/java

which python  # /usr/bin/python

```

+ **`whereis`**

该指令会在特定目录中查找符合条件的文件。这些文件应属于原始代码、二进制文件，或是帮助文件。该指令只能用于查找二进制文件、源代码文件和man手册页，一般文件的定位需使用locate命令。
搜索结果一般包括：二进制文件的路径、二进制文件的源码路径、对应 man 文件的路径
```shell

# 查看 bash 指令的位置
whereis ls # bash: /bin/bash /etc/bash.bashrc /usr/share/man/man1/bash.1.gz

# 显示bash 命令的二进制程序的地址
whereis -b bash # bash: /bin/bash /etc/bash.bashrc

# 显示bash命令的帮助文件地址
whereis -m bash # bash: /usr/share/man/man1/bash.1.gz

```

#### `grep`
[查找文件里符合条件的字符串](https://www.runoob.com/linux/linux-comm-grep.html)

```shell

grep test test* #查找前缀有“test”的文件包含“test”字符串的文件  

# testfile1:This a Linux testfile!       列出testfile1 文件中包含test字符的行  
# testfile_2:This is a linux testfile!   列出testfile_2 文件中包含test字符的行  
# testfile_2:Linux test                  列出testfile_2 文件中包含test字符的行 

```