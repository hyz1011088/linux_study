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




## [练习1]	内核裁剪：

####1. 修改configure配置

    configure配置项目众多，最开始到时候对于每个配置到具体功能不太清楚，不知道应该裁剪哪个配置之后kernel还能跑起来，只能一个一个试，不过耗时太长，而且容易出错。在过程中参考了：

* http://blog.chinaunix.net/uid-20531959-id-1931955.html

    但是这些裁剪之后还是不能够将内核裁剪到足够小，之后课上讨论之后，发现大家的裁剪思路，也是一个一个试。

    还有一个思路就是使用make allnoconfig就行一个一个添加，但是这个工作量也不小，耗时也很大。

####2. 裁剪之后大小

    编译的是64位内核。
    config  时大小为：      6M
    config2 时大小为：      984.6k


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
   

####3.内核态运行

    打包之后运行内核，然后运行1：
    ./1
    生成结果如下：
#####
    ![image](https://raw.github.com/hyz1011088/AOS_LAB/master/tiny_linux/pic/1.png)
     
    可以发现，1文件不能运行，提示权限不够！

    接着创建在目录下创建/trusted文件夹，并将1文件拷贝到该文件夹下运行，结果如下：
#####
    ![image](https://raw.github.com/hyz1011088/AOS_LAB/master/tiny_linux/pic/2.png)

    可以发现，程序输出1.c程序中用该输出到结果，即表示程序已在内核态运行。

####4.检查网络连接

    使用下面命令创建一个HTTP的server
    python2 -m SimpleHTTPServer

    使用wget命令来检测网络连接,结果如下：
#####
    ![image](https://raw.github.com/hyz1011088/AOS_LAB/master/tiny_linux/pic/3.png)

    可以发现网络已连接。



## [练习3]	问题集锦：

####1.busybox编译出错
     错误如下：
```
     inetd.c:(.text.unregister_rpc+0x17): undefined reference to `pmap_unset'
	networking/lib.a(inetd.o): In function `register_rpc':
	inetd.c:(.text.register_rpc+0x4e): undefined reference to `pmap_unset'
	inetd.c:(.text.register_rpc+0x75): undefined reference to `pmap_set'
	networking/lib.a(inetd.o): In function `prepare_socket_fd':
	inetd.c:(.text.prepare_socket_fd+0x8a): undefined reference to `bindresvport'

```
     解决办法：
	重新配置 $ make menuconfig
 	Linux System Utilities  --->  [ ]   Support mounting NFS file systems 网络文件系统
 	Networking Utilities  ---> [ ] inetd (Internet 超级服务器 )
	版本较高，默认支持功能较多，去掉编译不过的功能，如上。

     参考：
* http://blog.csdn.net/speedoniho/article/details/9089449


####2.安装完环境之后，使用make menuconfig，发现报错：

   错误如下：

```
*** Unable to find the ncurses libraries or the
*** required header files.
*** ‘make menuconfig’ requires the ncurses libraries.
***
*** Install ncurses (ncurses-devel) and try again.

```
   
    解决办法：
    
    安装ncurses-devel包即可。


####3.eth0启动不了
     裸机8139cp启动不起来，  找不到驱动。。然后insmod /8139cp.ko 手动将8139cp加载进来，但是注意在linux kernel的menuconfig中Enable loadable module support由于要加载8139网卡驱动。

     打开
```
    Symbol: 8139CP [=m]                                                     │  
  │ Type  : tristate                                                        │  
  │ Prompt: RealTek RTL-8139 C+ PCI Fast Ethernet Adapter support           │  
  │   Location:                                                             │  
  │     -> Device Drivers                                                   │  
  │       -> Network device support (NETDEVICES [=y])                       │  
  │         -> Ethernet driver support (ETHERNET [=y])                      │  
  │ (1)       -> Realtek devices (NET_VENDOR_REALTEK [=y])  

```
     并在rcS文件中加载8139cp.ko，rcS文件内容如下：

```
    #!/bin/sh

    mount proc
    mount -o remount,rw /
    mount -a
    
    insmod /8139cp.ko

    clear                               
    echo "Booting Tiny Linux"

    /sbin/ifconfig lo 127.0.0.1 up
    /sbin/route add 127.0.0.1 lo &

    ifconfig eth0 up
    ip addr add 10.0.2.15/24 dev eth0
    ip route add default via 10.0.2.2

```

####4.网络连接之后发现不能ping

    在测试网络是否正常连接过程中发现，ping 命令不能运行。后来查资料才知道qemu不支持ping功能。可以使用wget命令进行测试。


####5.打补丁细节问题

    打补丁的时候kml_4.0_001.diff放的位置，一开是将kml_4.0_001.diff放在/linux_defconfig下，执行patch命令，发现无法进行打补丁操作。

    打完补丁之后一定注意重新make一下，最开始容易忘记这个操作，导致无法进行内核态操作。


####6.每次修改ramdisk忘记打包

    在刚开始做实验的时候，不要忘记每次修改ramdisk之后要执行
    find . -print0 | cpio --null -ov --format=newc | gzip -9 > $TOP/obj/initramfs.cpio.gz
    不然修改的部分没有体现出来，这是对实验不熟悉的情况下容易犯到错误。


####7.打补丁make之后，发现仍然不能运行在内核态

    在打完补丁之后，发现仍然不能运行在内核态。当遇到这个问题之后。可能是原来make的内容/linux_defconfig会对结果存在影响，清除原来编译内容，重新编译。即如下操作之后即可解决：

    make O=../obj/linux_defconfig x86_64_defconfig
    cd $TOP/obj/linux_defconfig

    将kml_4.0_001.diff文件放在/obj/linux_deficonfig/source目录下
    cd ..
    make

    执行完上述操作之后，发现问题解决。


####8.内核裁剪问题

    在对内核进行裁剪时，修改config内容，然后再重新make，这个过程耗时较多，同时有些配置不太明白具体功能，可能N之后系统就跑不起来了。

    在修改配置文件的过程中，为的节省时间，我把[ir193](https://github.com/ir193/tiny_linux)的配置文件修改成64位之后，load进去发现报出错误：
    不能insmod 8139cp.ko
    即想到我到8139cp.ko文件需要加载，然后添加内核选项的模块加载配置，但是之后还是一直报错，才知道捷径是不可取的！！！由于编译器到版本等原因，配置文件直接加载会出现一系列问题。

    所以还是需要自己逐个修改配置尝试减小内核。
