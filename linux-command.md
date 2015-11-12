# linux学习

## [练习0]	基础命令:

linux的最基础命令：

```
ls -al ~
:列出主文件夹下的所有隐藏和相关文件属性

echo $LANG	
:显示目前所支持的语言，若修改，命令为LANG=en_US

date 
:显示日期和时间

date +%Y/%m/%d/%H:%M	
:按照这个模式显示年月日时分

cal
:显示日历

cal 2009
:显示2009年日历

cal 10 2009
:显示2009年10月日历

bc
:计算器，+-*/^%,默认是整数运算，需要小数位数设置scale=位数。quit离开

[tab]键
:命令补全和文件补齐

[ctrl]-c
:中断目前程序

[ctrl]-d
:键盘输入结束，有时候相当于exit

man 
:manual操作说明,man+命令，即可查看命令如何使用，例如：man date，可以查看命令如何使用.进入man命令之后，可以按下空格键进行翻页，q键退出man环境,/+字符串用于查询字符串,如/date可以查询date查询结果中的date字符串

info
:用法与man类似，但是info将文件数据拆分成不同段落，查看下一节点用N,上一阶段用P

nano
:调用nano编辑器，[ctrl]+字符进行操作，如[ctrl]-X离开软件

sync
:数据同步写入磁盘,直接sync命令即可

shutdown
:关机命令，要执行这个命令需要在root权限下，可以使用sudo su获取root权限，shutdown -h 20:30/now

reboot
:重启命令

init
:切换执行等级   run level 0: 关机；run level 3:纯命令行模式；run level 5:含有图形界面模式；run level 6:重启    例如： init 0 即为关机命令

```


## [练习1]	linux文件,目录，磁盘格式：

####1. linux文件权限和目录配置

```
ls -al
：列出所有文件的权限和属性，其中权限说明为10位，例如[drwxr-xr-x],划分为[d,rwx,r-x,r-x]，第1位代表文件名为目录或者文件，d代表目录；234位代表拥有者权限；567位代表用户组权限；890代表其他用户权限；r:读；w：写；x：可执行。
列出的信息分别为：文件权限；连接数；文件所有者；文件所属用户组；文件大小；文件最后被修改时间；文件名

chgrp
：改变文件所属用户组，例如命令，chgrp users 1.txt,即将1.txt所属用户组改为users所有，当用户组不存在时会报错

chown
：改变文件所有者，例如命令，chown bin 1.txt，即将1.txt文件所有者变为bin

chmod
：改变文件权限。linux文件的基本权限有9个，分别是owner,group,others三种身份的r,w,e。权限总共是10位字符，其中赋予r:4;w:2;x:1;
:所以770代表的权限为：[-rwxrwx---]4+2+1;4+2+1;0+0+0。改变文件苏醒到命令为：chmod 777 .bashrc

ls -l /
:显示操作系统根目录下的目录树
```

####2. linux文件和目录管理

```
特殊的目录：
.	代表此层目录
..	代表上一曾目录
-	代表前一个工作目录
~	代表“目前用户身份”所在的主文件夹
~acc	代表acc这个用户到主文件夹(acc是个帐号名称)

cd ～
：表示回到自己的主文件夹

pwd
:显示目前所在目录

mkdir
：创建新目录，mkdir test即为创建一个test新目录；如果要创建一个含有权限到目录，mkdir -m 711 test即创建一个权限为711的目录test

rmdir
：删除一个空到目录，如rmdir test即为删除空目录test，当test非空时无法删除。rmdir -p test1/test2/test3，-p代表将上层空目录一并删除

ls
:查看文件和目录

cp
:复制文件，命令：cp 源文件 目标文件；源文件和目标文件可以是文件或是文件夹

mv
:移动文件和目录，或更名；命令：cp source destinaion; 多个需要移动时:cp source1 source2 source3... directory；
重命名命令：mv mvtest mvtest2;即对目录mvtest重命名为mvtests2

cat
:直接查看文件内容。直接加文件名，例如：cat 1.c

tac
:反向显示文件内容。观察可知tac和cat顺序刚好相反，即可明白命令含义

nl
：添加行号打印文件内容

more
:翻页查看文件内容。当文件行数较多时，辅助按键：
	空格键	代表向下翻一页
	Enter	代表向下滚动一行
	/字符串	代表在这个显示的内容当中，向下查询‘字符串’这个关键字
	:f	立即显示文件名以及当前显示到行数
	q	代表立即离开more命令，不再显示文件内容
	b或[ctrl]-b	代表立即翻页，不过这个操作只对文件有用，对管道无用

less
：翻页查看文件内容。比more命令更灵活，辅助键：
	空格键		向下翻动一页
	[PageDown]	向下翻动一页
	[PageUp]	向上翻动一页
	/字符串		向下查询“字符串”的功能
	?字符串		向上查询“字符串”的功能
	n		重复前一个查询(与/或？有关)
	N		反向重复前一个查询(与/或？有关)
	q		离开less命令

head
:选取文件前几行

tail
:选取文件后几行

od
:查看非文本文件

umask
:当前用户在新建文件或目录时候的权限默认值。直接umask命令即可； umask -S可以看到数字形态的权限；

chattr
:设置文件的隐藏属性

lsattr
：显示文件到隐藏属性

file
:查看文件类型。如：file 1.c

whereis
:寻找特定文件

loacte
:查找与给出名称相关的文件名,例如：loacte 1.c

find
:发现差别；有详细命令参考

```

####3. linux磁盘与文件系统管理

```
df
:调出目前挂载的设备

dumpe2fs [-bh] 设备文件名
:查询每个区段与superblock的信息都可以使用dumpe2fs这个命令来查询

ls -li
:查看root目录内的文件所占用的inode号码

du
:评估文件系统的磁盘使用量

fdisk [-l] 设备名
:磁盘分区

mkfs
:磁盘格式化命令

fsck/badblocks
:磁盘校验

mount
:磁盘挂载
 
```

####4. linux文件与文件系统的压缩和打包

```
常见的压缩文案扩展名:
*.Z:	compress程序压缩的文件
*.gz	gzip程序压缩的文件
*.bz2	bzip程序压缩的文件
*.tar	tar程序打包的数据，并没有压缩过
*.tar.gz  tar程序打包的文件，其中经过gzip的压缩
*.tar.bz2  tar程序打包的文件，其中经过bzip2的压缩

compress [-rcv] 文件或目录
:compress压缩，其中-r可以连同目录下的文件也同时给予压缩，-c将压缩数据输出成standard output(输出到屏幕),-v可以显示压缩后的文件信息以及压缩过程中的一些文件名变化
uncompress 文件.Z
:解压缩

gzip [-cdtv#] 文件名
:用的比较广泛的压缩文件命令，其中-c将压缩的数据输出到屏幕上，可通过数据流重定向来处理;-d解压缩的参数；-t可以用来检验一个压缩文件的一致性；-v可以显示出原文件/压缩文件的压缩信息；-#压缩等级，-1最快，但是压缩比最差，-9最慢，但是压缩比默认是-6
zcat 文件名.gz
:解压缩

bzip2 [-cdkzv#] 文件名
:压缩比比bzip2更好的压缩命令，用法和gzip类似
bzcat 文件名.bz2
:解压缩

tar
:压缩常见命令，主要有
压缩： tar -jcv -f filename.tar.bz2 被压缩的文件或目录名称
查询： tar -jtv -f filename.tar.bz2
解压缩： tar -jxv -f filename.tar.bz2 -C 欲解压缩的目录

dump
:完整备份工具

mkisofs
:新建镜像文件

cdrecord
:光盘刻录工具

dd
:可以备份完整的分区或磁盘

cpio
:备份命令
	
```

## [练习2]	学习shell与shell script：


####1.vim程序编辑器

```
vi filename
:使用vi打开文件进入一般模式
   按下i进入编辑模式，开始编辑文字
   按下[ESC]键回到一般模式
   在一般模式中输入“:wq”保存后离开vi，输入“:q”不保存离开，“:wq!”强制保存离开。!是强制离开的意思

块选择
:v 字符选择，会将光标经过的地方反白选择
 V 行选择，会将光标经过的行反白选择
 [Ctrl]+v 块选择，可以用长方形的方式选择数据
 y 将反白的地方复制起来
 d 将反白的地方删除

多文件编辑
:vim 1.c 2.c 3.c ....即vim打开多个文件，编辑完一个之后
  :n 编辑下一个文件
  :N 编辑上一个文件
  :files 列出目前这个vim的打开的所有文件

多窗口功能
:vim打开一个文件窗口之后
   :sp [filename]  打开一个新窗口，如果加filename,表示在新窗口打开一个新文件，否则表示两个窗口为统一个文件内容（同步显示）
   [ctrl]+w+j/[ctrl]+w+向上箭头  切换到上面窗口
   [ctrl]+w+k/[ctrl]+w+向下箭头  切换到下面窗口
   [ctrl]+w+q   结束离开

vim环境变量设置与记录：～/.vimrc,~/.viminfo

```

## [练习1]	学习shell和shell script：

####1. 学习bash

```
type [-tpa] name
:不带参数时，显示命令是否是内置命令；-t显示命令的意义，-p表示当name是外部命令时，才会显示完整文件名；-a表示由PATH变量定义的路径中，将所有含有name的命令都列出来。命令例如：type ls

echo $PATH
:PATH变量的显示，要显示其他变量，替换PATH即可

unset name
:取消设置的name变量的内容

env
:env查看环境变量与常见环境变量说明

set
:查看所有环境变量（含环境变量和自定义变量）

locale
:查看linux支持多少种语系

环境变量=全局变量，自定义变量=局部变量

read
:读取来自键盘输入的变量

declare/typeset
:声明变量类型

array
:数组变量类型

ulimit
:设置限制用户的某些系统资源

alias
:命令别名设置。例如：alias lm='ls -l | more',即设置lm执行ls -l | more

unalias
:取消别名设置，如：unalias lm，即取消刚才的别名lm

history
:查看历史命令,history 10即为查看最近10条command记录

!number
:执行第几条命令的意思

!command
:由最近的命令向前搜索命令串开头为command的那个命令，并执行

!!
:执行上一条命令

cut
:可以将一段信息的某一段“切”出来，处理的信息是以“行”为单位的

grep
:grep分析一行信息，若当中有需要的信息，就将该行拿出来。而cut是在一行信息中取出我们想要的

sort
:根据不同的数据类型进行排序。如命令：cat /etc/passwd | sort，即为对/etc/passwd目录下的帐号进行排序

uniq
:将重复的数据一行显示。命令：last | cut -d ' ' -f1 | sort | uniq -c，使用last将帐号列出，仅取出帐号列，进行排序后仅取出一位，同时进行登录次数计数

wc
:统计的行数，字数，字符数。命令：cat /etc/man.config | wc即为统计/etc/man.config中有多少相关的字，行，字符数

tr,col,join,paste,expand
:字符转换命令

xargs
:参数代换

```


####2. 静态编译


```


```



####内容参考《Linux私房菜》
