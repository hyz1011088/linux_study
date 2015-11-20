# linux学习

## [练习0]	学习shell script：

####1. shell script

shell script是利用shell的功能所写的一个程序（program），这个程序是使用纯文本文件，将一些shell的语法与命令（含外部命令）写在里面，搭配正则表达式,管道命令与数据流重定向等功能，以达到我们所想要的处理目的。

第一个script,sh0.sh:
```
#!/bin/bash   声明script使用的是shell名称
# program:
#	This program shows "hello world!" in your screen.
# History:
# 22时12分17秒
PATH=/bin:/sbin:/usr/bin:/usr/sbin:/usr/local/bin:/usr/local/bin:~/bin
export PATH
echo -e "hello world! \a \n"
exit 0
```

输入命令：sh sh0.sh

输出：	hello world!



####内容参考《Linux私房菜》
