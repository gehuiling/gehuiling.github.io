---
layout: post
title: "🎐 常见命令"
date: 2021-03-30 00:00:00
categories: [Linux]
tags: [Linux]
comments: true
---


<!--more-->

### Git

```shell

git 

```
### Linux常用命令

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

#### 修改权限 

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