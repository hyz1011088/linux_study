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



```


## [练习2]	kernel mode linux：

该步骤和过程主要参考：
* http://web.yl.is.s.u-tokyo.ac.jp/~tosh/kml/index.html

####1.打补丁

    下载[Kernel Mode Linux Patch for Linux Kernel 4.0 (for IA-32, AMD64, MicroBlaze, and ARM)](http://web.yl.is.s.u-tokyo.ac.jp/~tosh/kml/kml/for4.x/kml_4.0_001.diff.gz) 

    将kml_4.0_001.diff文件放在/obj/linux_deficonfig/source目录下

    使用下列命令进行打补丁：
    patch -p1 < kml_4.0_001.diff

    接着到/linux_deficonfig重新make内核



####2.静态编译

    然后在/ramdisk下创建一个1.c文件，内容如下：

```
#include<stdio.h>
int main(int argc, char* argv[])
{
    __asm__ __volatile__("cli");
    int i=0,j=1,k=2,l=3,m=4,n=5;
    printf("ping i: %d\n", i);
    printf("ping j: %d\n", j);
    printf("ping k: %d\n", k);
    printf("ping l: %d\n", l);
    printf("ping m: %d\n", m);
    printf("ping n: %d\n", n);
    return 0;
}

```
    接着在/ramdisk目录下静态编译1.c文件
    gcc -static 1.c -o 1
   
    生成文件 1
   
