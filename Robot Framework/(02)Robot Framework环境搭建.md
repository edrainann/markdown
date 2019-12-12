# (02)Robot Framework环境搭建

## 安装Robot Framework	

如果想使用 Robot Framework 必须要安装：

- Python 编程语言，[参考](http://www.testclass.net/selenium_python/install-selenium/) 。
- Robot Framework

### python2.X下

#### 安装环境

Python:

<https://www.python.org/>

RF框架是基于python 的，所以一定要有python环境。

 

Robot framework ：

<https://pypi.python.org/pypi/robotframework/2.8.5>

　　这个不是解释了，RF框架。虽然在做基于UI的自动化时，它展现出来的很像QTP，我之前也以为它和QTP差不多，仔细了解你会发展它能做的事情还是很多的。就像初学selenium 者，会误以为selenium 就是seleniumIDE。

 

wxPython :

<http://www.wxpython.org/download.php>

　　Wxpython 是python 非常有名的一个GUI库，因为RIDE 是基于这个库开发的，所以这个必须安装。

 

Robot framework-ride

<https://pypi.python.org/pypi/robotframework-ride>

　　RIDE就是一个图形界面的用于创建、组织、运行测试的软件。

 

Robot framework-selenium2library：

<https://pypi.python.org/pypi/robotframework-selenium2library/1.5.0>

　　RF-seleniumlibrary 可以看做RF版的selenium 库，selenium （webdriver）可以认为是一套基于web的规范（API），所以，RF 、appium 等测试工具都可以基于这套API进行页面的定位与操作。

\----------------------

可以通过python 的pip工具包进行安装：

\>pip install robotframework-selenium2library

 

如果初次接触上面的东西的话，觉得装的东西有点多。 如果之前有了解过python 或selenium的话就不会有这样的感觉。

Robot Framework 推荐 pip 方法安装 (在Windows命令提示符（cmd）/ Linux终端输入)：

```
λ pip install robotframework

Collecting robotframework
  Using cached robotframework-3.0.2.tar.gz
Installing collected packages: robotframework
  Running setup.py install for robotframework ... done
Successfully installed robotframework-3.0.2　　　　　　　　　　　　　　　　　　　　　　　

```

#### 安装 RIDE（可选）

------

如果你使用的是 Python2.x 版本，虽然 Python2.x 预计到2020年停止维护了，但仍然不少人在使用，Robot Framework 的所有相关库也没有完全迁移到Python3.x。

比如 [Robot Framework RIDE](https://github.com/robotframework/RIDE) ，它是编写 Robot Framework 的标准编辑器。对于新手来降低了 Robot Framework的使用门槛。

接下来安装 RIDE (只针对 Python2.x 环境)

- 安装 wxPython

下载地址: <http://sourceforge.net/projects/wxpython/files/wxPython/2.8.12.1/>

wxPython 是 Python 非常有名的一个 GUI 库，因为 RIDE 是基于这个库开发的，所以这个必须安装。必须是 **wxPython 2.8.12.1** 版本，RIDE 基于该版本开发。

- 安装 RIDE

推荐 pip 安装

```
λ pip install robotframework-ride

Collecting robotframework-ride
  Using cached robotframework-ride-1.5.2.1.tar.gz
Installing collected packages: robotframework-ride
  Running setup.py install for robotframework-ride ... done
Successfully installed robotframework-ride-1.5.2.1

```

RIDE 是 Robot Framework 的官方编辑器。它使测试用例的创建、运行、测试项目的组织可以在图形界面下完成。

- 启动 RIDE

切换到 Python2.7.x 的 Script 目录（例如：**C:\Python27\Scripts**）， 运行 ride.py 文件。

```
C:\Python27\Scripts> python ride.py
```



在你安装好RF-ride之后，桌面就会生成一个RIDE图标。双击启动，界面如下：
![img](http://otfah9orz.bkt.clouddn.com/rf_RIDE.png)

#### 关于MAC OS系统安装

通过`edrain$ brew install wxPython`安装wxPython后， 执行ride.py时会出现：

Ride executable was installed in `/usr/local/bin/ride.py`. If you try to start RIDE now, you’ll get following error:

```
wxPython not found.
You need to install wxPython 2.8.12.1 with unicode support to run RIDE.
wxPython 2.8.12.1 can be downloaded from http://sourceforge.net/projects/wxpython/
```

经过一番查找终于发现：

Patch robotframework-ride’s wx detection

Find your `robotide` python egg and open its `__init__.py`. On my machine this was located in`/usr/local/lib/python2.7/site-packages/robotide`.

You’ll have to update `wxversion` detection, so add following line:

```
try:
    import wxversion
    from wxversion import VersionError
    if sys.platform == 'darwin': # CAN NOT IMPORT IS_MAC AS THERE IS A wx IMPORT
        supported_versions.append("2.9")
        supported_versions.append("3.0")
    wxversion.select(supported_versions)
    import wx
```

需要添加`supported_versions.append("3.0")`这句话,便可以成功运行ride.py

That’s it, now let’s start `ride.py`.

## Starting RIDE

If you’ve added `/usr/local/bin` to you’re `PATH` environment variable you should be able to start ride by typing `ride.py` on your command line.:

```
$ ride.py
Creating librarykeywords database to "/Users/daniel/.robotframework/ride/librarykeywords.db"

```

![../../_images/robotframework-ride.png](http://widerin.net/_images/robotframework-ride.png)

http://widerin.net/blog/install-robot-framework-ride-through-homebrew/

会提示 warnings.warn("wxPython/wxWidgets release number mismatch")，目前本人还没有找到解决办法。

```
EdraindeMacBook-Pro:~ edrain$ python
Python 2.7.13 (default, Dec 18 2016, 07:03:39)
[GCC 4.2.1 Compatible Apple LLVM 8.0.0 (clang-800.0.42.1)] on darwin
Type "help", "copyright", "credits" or "license" for more information.
>>> import wx
/usr/local/lib/python2.7/site-packages/wx-3.0-osx_cocoa/wx/_core.py:16633: UserWarning: wxPython/wxWidgets release number mismatch

  warnings.warn("wxPython/wxWidgets release number mismatch")
```



mac下wxpython的安装问题

https://www.zhihu.com/question/40061801

### python3.X下

#### 安装环境

由于我直接是使用的python3，电脑的环境变量路径也是配置的python3的，于是直接win+R打开CMD，pip安装以下插件：



C:\Users\a>pip install wxPython

 

C:\Users\a>pip install robotframework

 

C:\Users\a>pip install robot framework

 

C:\Users\a>pip install robotframework-ride

 

C:\Users\a>pip install pygments

 

C:\Users\a>pip install robotframework-selenium2library

可以通过pip show 插件名进行查看安装的路径

```
C:\Users\a>pip show robot framework
Name: robot
Version: 20071211
Summary: Django application for Request Tracking
Home-page: http://www.irl.styx.org/hgweb.py/robot/
Author: William Waites
Author-email: ww@irl.styx.org
License: GPL
Location: c:\python36-32\lib\site-packages
Requires:
---
Name: framework
Version: 0.1.0
Summary: The one framework of all frameworks
Home-page: UNKNOWN
Author: Ivan Suroegin
Author-email: ivan.suroegin@gmail.com
License: UNKNOWN
Location: c:\python36-32\lib\site-packages
Requires:

C:\Users\a>pip show robotframework
Name: robotframework
Version: 3.0.2
Summary: A generic test automation framework
Home-page: http://robotframework.org
Author: Robot Framework Developers
Author-email: robotframework@gmail.com
License: Apache License 2.0
Location: c:\python36-32\lib\site-packages
Requires:
```
pip install robotframework 和 pip install robot framework中间有空格，安装的不是一个插件。

#### 安装RIDE

没有官方版本的，直接在github上找到一个大神写的RIDE适配Python3

```		
pip install -U https://github.com/HelioGuilherme66/RIDE/archive/python3.zip
```

https://github.com/HelioGuilherme66/RIDE

https://github.com/robotframework/RIDE/issues/1703



启动RIDE

1、切换到切换到 Python3.x 的 Script 目录（例如：**C:\Python36-32\Scripts**）， 运行 ride.py 文件。

```
C:\Python36-32\Scripts>python ride.py
```
![微信截图_20180222171558.png](http://upload-images.jianshu.io/upload_images/1683050-984870d90088b88c.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)



2、在桌面创建.bat执行文件

```
python -c "from robotide import main;main()"
```

![py3ride.bat](http://upload-images.jianshu.io/upload_images/1683050-bf9abf16308edc35.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

也可以是

```
cd C:\Python36-32\Scripts
python ride.py
```

个人觉得在定制化用python3写插件时用处比较大。

平时的话，也可以老老实实装个python2，直接运行。






#### 参考

http://www.cnblogs.com/fnng/p/3871712.html

http://www.testclass.net/rf/install/

http://robotframework.org/robotframework/#user-guide

https://www.zhihu.com/question/40061801

http://widerin.net/blog/install-robot-framework-ride-through-homebrew/