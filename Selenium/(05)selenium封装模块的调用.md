# (05)selenium封装模块的调用

python的内部模块调用

1.需要用到的文件们都在同一个目录下



![目录格式是这个样子的](https://upload-images.jianshu.io/upload_images/1683050-fe2033642af789ca.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

pub.py的代码：

```python
#!/usr/bin/env python
# -*- coding: utf-8 -*-
def add(a, b):
    return a+b
```

count.py的代码：

```python
#!/usr/bin/env python
# -*- coding: utf-8 -*-

from pub import add #从模块pub 导入 add这个方法

print(add(4,5))
```



2.需要用到的文件们不在同一个目录下：

![目录格式是这个样子的](https://upload-images.jianshu.io/upload_images/1683050-f1c99048e3d7e26a.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

model文件夹下pub.py的代码：（嗯，跟之前的是一模一样）

```python
#!/usr/bin/env python
# -*- coding: utf-8 -*-
def add(a, b):
    return a+b
```

count.py的代码：

```python
#!/usr/bin/env python
# -*- coding: utf-8 -*-

from  model.pub import add  #这里就有变化啦，model.pub
print(add(4,5))

```

3.稍微洋气一点点封装~~~

![目录格式是这个样子的](https://upload-images.jianshu.io/upload_images/1683050-809e560b289209e6.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

show u the code：

mymodule作为package包需要有`__init__.py`的文件，可以为空文件；module.py里面可以存放需要的模块方法。

module.py

```python
from time import sleep

class DiaoYong(object):
    """这是某些方法的调用"""

    def __init__(self, driver):
        self.driver = driver #这里不能再文件顶部导入webdriver，不然之后会有冲突
        # self.driver = None

    def swipeDown(self,site):
         """模拟H5端滑倒的代码"""
        # sleep(2)
        # button = self.driver.find_element_by_xpath(site)
        # sleep(3)
        # """从button元素像下滑动200元素"""
        # Action = TouchActions(self.driver)
        # Action.scroll_from_element(button, 0, -200).perform()
        # sleep(3)
        """通过找到百度来调试是否成功调用了这个方法"""
        sleep(2)
        self.driver.find_element_by_xpath(site).click()
        print("swipeDown调用成功的啦~~~")
        

```

02unittest.py

```python
from selenium import webdriver
import unittest
from time import sleep

from mymodule.module import DiaoYong

class unitTest01(unittest.TestCase):  #unitTest01继承unittest.TestCase

    def setUp(self):
        print ("--------start---------")
        self.driver = webdriver.Chrome()
        # sd = diaoyong(driver)
        self.driver.implicitly_wait(30)
        self.base_url = "http://www.baidu.com"
        # 脚本运行时，错误的信息将被打印到这个列表中
        self.verificationErrors = []
        # 是否继续接受下一个警告
        self.accept_next_alert = True
        # diaoyong=module.diaoyong(self)

    def tearDown(self):
        self.driver.quit()
        print ("-------end--------")
        # pass

    def test_Button(self):
        driver = self.driver
        driver.get(self.base_url + "/")
        driver.find_element_by_id("kw").send_keys("selenium webdriver")
        driver.find_element_by_id("su").click()
        sd = DiaoYong(driver)  #调用前将对象实例化；这个地方传入的是driver，跟前面modle里面diaoyong对应
        sd.swipeDown('//*[@id="1"]/h3/a/em[2]')


if __name__ == '__main__':
     unittest.main()
```

如果你觉得实例化比较麻烦的话，可以直接在代码里面def 方法

```python
from selenium import webdriver
import unittest
from time import sleep
from selenium.webdriver.common.touch_actions import TouchActions


class unitTest01(unittest.TestCase):

    def setUp(self):
        print ("--------start---------")
        self.driver = webdriver.Chrome()
        self.driver.implicitly_wait(30)
        self.base_url = "http://www.baidu.com"
        # 脚本运行时，错误的信息将被打印到这个列表中
        self.verificationErrors = []
        # 是否继续接受下一个警告
        self.accept_next_alert = True
        

    def tearDown(self):
        self.driver.quit()
        print ("-------end--------")
        # pass

   def swipeDown(self, site):
       driver = self.driver
       sleep(2)
       button = driver.find_element_by_xpath(site)
       sleep(3)
       """从button元素像下滑动200元素"""
       Action = TouchActions(driver)
       Action.scroll_from_element(button, 0, -200).perform()
       sleep(3)
       print("success")
 

    def test_Button(self):
        driver = self.driver
        driver.get(self.base_url + "/")
        driver.find_element_by_id("kw").send_keys("selenium webdriver")
        driver.find_element_by_id("su").click()
        self.swipeDown('//*[@id="kw"]')     #class内部调用
        

```





Tips：

想要Pycharm正确识别模块的话，需要**右键**点击该文件夹->选择"Sources Root"，这样就可以正确的导入模块啦~

![image.png](https://upload-images.jianshu.io/upload_images/1683050-cee9bb3505335038.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)