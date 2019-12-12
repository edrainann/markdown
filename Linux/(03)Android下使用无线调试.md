# （03）Android下使用无线调试

Android开发过程中，大部分时候我们需要连接usb线通过adb做调试，adb实际上可以设置通过网络来连接，这个设置是在手机端，而不是在pc端。

### 一、初始设置

要想设置adb走无线网络，有几种方法：

1）对所有手机都适用的方法，就是通过pc侧的adb命令去设置。

1. 先通过usb连接手机，
2. 然后执行`adb tcpip 5555` ，此时手机侧的adb就处于无线模式，
3. 最后就可以拔出数据线了。

2）root后的设备，可以在手机端直接通过App设置，这样当然比较方便，并且完全不需要usb线，可惜需要root，很多手机不具备这个条件。

3）有些手机的ROM，在开发人员选项中，可以打开adb wifi，这样也算方便，但是大部分手机的ROM也没有这个选项。

### 二、平常使用

只要手机没有重启过，任何时候想调试手机，需要先执行adb connect命令就可以了，这个命令很简单

```bash
adb connect phone-ip:5555
```

上面的phone-ip就是手机的ip，显然我们要让手机和pc处于同一个wifi局域网中，才能访问，通过手机端wifi设置的页面，可以看到这个ip。要确保是连接得通的，需要先用电脑ping一下手机ip地址：

```bash
 ping 10.244.87.88
```

### 三、adb远程控制

通过adb，有一个非常有用的工具，叫做scrcpy，可以对安卓设备远程控制，同样的，这个工具也一样可以在无线模式下工作。

Github：<https://github.com/Genymobile/scrcpy>
下载地址：<https://github.com/Genymobile/scrcpy/releases>

命令行执行:

启动scrcpy`scrcpy`

如果有多个设备，需要指定序列号，序列号可以从`adb devices`获得

`scrcpy -s a1171b8`

查看帮助`scrcpy --help`

设置码率（默认8M）`scrcpy -b 8M`

限制投屏尺寸`scrcpy -m 1024`

裁剪投屏屏幕(长:宽:偏移x:偏移y)```scrcpy -c 800:800:0:0```

手指触摸的时候显示轨迹球`scrcpy -t`

显示版本信息`scrcpy -v`

但在无线模式下，网络速度可能不如usb直线快，所以需要在scrcpy启动的时候，增加几个参数，控制scrcpy的屏幕分辨率和压缩率，需要两个参数：

```
scrcpy -m 600 -b 1m
```

这两个参数分别控制屏幕分辨率和压缩码率，根据你自己的无线网速来调整就好，这样也可以愉快的通过无线使用scrcpy的远程控制功能了。