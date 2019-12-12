# selenium保留chrome配置及登陆

​	selenium默认开启浏览器是不带任何配置的,即你访问任何网页都是一次全新的访问,浏览器不会因为你前一次打开登陆过而现在打开不用登陆了,也就是不加在以前的cookie数据,这样会造成需要频繁登陆的困恼,容易出现一些问题,例如登陆失败



#### 1.找到Chrome浏览器已保存的密码

一般保存在一个sqlite3数据库文件中，和Cookies数据库在同一个文件夹。

如C:\Users\a\AppData\Local\Google\Chrome\User Data\Default\Login Data

![image.png](http://upload-images.jianshu.io/upload_images/1683050-0d1fb86e0d5b888d.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

#### 2.在第二步为webdriver设置这个Options

注意把前面俩个默认参数加上,"chromedriver"和0

driver = webdriver.Chrome("chromedriver",0,driverOptions)

参考https://jingyan.baidu.com/article/335530dae2dd3d19ca41c342.html

最终我把代码修改成了这样，也还是没有成功。

但是请不要慌张

 ```python
from selenium import webdriver
import time

driverOptions = webdriver.ChromeOptions()
# r代表后面的字符串斜杠不转义，''表示python识别空格
driverOptions.add_argument(r"user-data-dir=C:\Users\a\AppData\Local\Google\Chrome\User''Data\Default\Login''Data")
driver = webdriver.Chrome(executable_path=r"F:\broswerdrvier\chromedriver.exe",port=0,chrome_options=driverOptions)

driver.get("http://www.baidu.com")

 ```




> 关于python识别空格

​	文件是每一个程序员经常要用到的，原来主要用C编程，python是操作方便，于是最近的实验试着用python编程，首先说一下文件的空格和空行和结束符的区别。空格是指一行中有除了空格还有其余的字符，空行是指这行除了换行符没有其他的操作符，而文件的结束符都是空。这些区别也成为了文件处理他们的关键，判断空行是用‘\n’,判断结束符是用‘’（python代表空），判断空格是用‘ ’。

http://blog.csdn.net/u013498580/article/details/50594749



#### 3.在打开的百度页面中找到“登录”，并输入你的用户名、密码。

之后请关闭该网页

![image.png](http://upload-images.jianshu.io/upload_images/1683050-e1577b08437cd501.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)



#### 4.再次运行程序

终于实现成功登陆啦~

![image.png](http://upload-images.jianshu.io/upload_images/1683050-37409b521da43d9e.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)



#### 参考

[Python模拟登陆万能法-微博|知乎](https://zhuanlan.zhihu.com/p/28587931)

https://jingyan.baidu.com/article/335530dae2dd3d19ca41c342.html

http://blog.csdn.net/u013498580/article/details/50594749

http://blog.csdn.net/jueblog/article/details/50440202

https://stackoverflow.com/questions/22130109/cant-use-chrome-driver-for-selenium

[Python模拟登陆万能法-微博|知乎]: https://zhuanlan.zhihu.com/p/28587931

