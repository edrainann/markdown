# (01)VScode终端运行MySQL语句

1.安装好MySQL运行环境

![image.png](https://upload-images.jianshu.io/upload_images/1683050-8dde916708989889.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

如何在WIN环境下，安装MySQL可以在网上找到很多教程，这里我就不做介绍啦。

放出个链接，以供参考https://www.cnblogs.com/lmh2072005/p/5656392.html



在Linxu环境下，安装MySQL最简单的方式是在线安装。只需要几行简单的命令（ # 号后面是注释）：

\#安装 MySQL 服务端、核心程序
`sudo apt-get install mysql-server`

\#安装 MySQL 客户端
`sudo apt-get install mysql-client`

在安装过程中会提示确认输入YES，设置 root用户密码（之后也可以修改）等，稍等片刻便可安装成功。

安装结束后，用命令验证是否安装并启动成功：

`sudo netstat -tap | grep mysql`



2.找到MySQL安装的路径

![image.png](https://upload-images.jianshu.io/upload_images/1683050-c54a3e47304ef63a.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

记住MySQL的起始位置；

3.打开VSCode

进入MySQL的起始路径

```
PS G:\Git> cd c:
PS C:\> cd Program" "Files
PS C:\Program Files> cd .\MySQL\
PS C:\Program Files\MySQL> cd '.\MySQL Server 5.6\'
PS C:\Program Files\MySQL\MySQL Server 5.6> cd bin
PS C:\Program Files\MySQL\MySQL Server 5.6\bin> mysql -u root -p
Enter password: *******

完整的路径就是这样子输入的啦：
G:\Git> cd C: 
C:\> cd '.\Program Files\MySQL\MySQL Server 5.6\bin'
```

通过 `mysql -u root -p` 成功运行MySQL

Tips：

如果路径中有空格的地方，可以用`" "`框起来 或者 `'.\ \'`框起来

这样便可以在VSCode中运行MySQL语句了。

![image.png](https://upload-images.jianshu.io/upload_images/1683050-e7988ec67e23ae2b.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)



Tips:

1）VSCode的**列模式**快捷键同时按住**Shift+Alt** 就可以啦，

Notepad++的列模式快捷键是直接按住Alt感觉更方便一点。

2）VSCode添加多行注释**Shift+Alt+A**

神奇吧，我也觉得，偶然间发现的，平时都是手敲，程序员们真伟大。