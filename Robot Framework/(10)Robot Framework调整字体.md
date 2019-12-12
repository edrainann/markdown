# (10)Robot Framework调整字体

Robot Framework中RIDE那点点小的字和一片黑，感觉眼睛都要看得瞎掉了，一不小心输错一个字母调试的时候就出现了问题。

万能的google告诉了我答案，原来它也是可以调的，嘻嘻~



在使用robot的时候，在Text Edit模式下，代码和其他编辑器一样可以区分代码颜色。

#### 1、安装控件Pygments

```
F:\Python27\Scripts>pip show pygments

F:\Python27\Scripts>pip install pygments
Collecting pygments
  Using cached Pygments-2.2.0-py2.py3-none-any.whl
Installing collected packages: pygments
Successfully installed pygments-2.2.0

F:\Python27\Scripts>pip show pygments
Name: Pygments
Version: 2.2.0
Summary: Pygments is a syntax highlighting package written in Python.
Home-page: http://pygments.org/
Author: Georg Brandl
Author-email: georg@python.org
License: BSD License
Location: f:\python27\lib\site-packages
Requires:
```



#### 2、打开RIDE

安装完成后可以在robot的IDE菜单栏 Tools > Preferences > Text Editor

然后设置你平时适应的颜色和字体啦

![image.png](http://upload-images.jianshu.io/upload_images/1683050-064c2afe89834029.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)



![image.png](http://upload-images.jianshu.io/upload_images/1683050-5ed88a2c321f44d2.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)







#### 参考

http://blog.csdn.net/cuipan1234/article/details/70649827