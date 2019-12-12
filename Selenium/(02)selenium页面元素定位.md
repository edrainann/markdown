# (02)selenium页面元素定位

　用selenium操作浏览器进行自动化操作其实就是通过元素属性执行相关操作。所以，我们要知道怎样去查找元素，定位元素。

　　常见的定位属性有：

```python
#查找元素的id
find_elements_by_id(id)
find_elements_by_id(id)
#查找元素的name
find_element_by_name(name)
find_elements_by_name(name)
#查找元素的链接文本
find_element_by_link_text(link_text)
find_elements_by_link_text(link_text)
#查找元素的链接的部分文本
find_element_by_partial_link_text(link_text)
find_elements_by_partial_link_text(link_text)
#查找元素的标签名
find_element_by_tag_name(name)
find_elements_by_tag_name(name)
#查找元素的xpath
find_element_by_xpath(xpath)
#查找元素内的子元素的xpath
find_elements_by_xpath(xpath)
#查找一个元素的类名
find_element_by_class_name(name)
#查找元素的类名
find_elements_by_class_name(name)
#查找并返回一个元素的CSS 选择器
find_element_by_css_selector(css_selector)
#查找并返回多个元素的CSS 选择器列表
find_elements_by_css_selector(css_selector)

webdriver中常用的操作方法有：
.click()              	   进行点击操作，用于点击一个按钮。
.send_keys()          	   在对象上模拟按键输入，用于在一个输入框里输入内容。
.clear()                   清除对象的内容。
.text                      获取该元素的文本。
.submit()                  提交表单。
.get_attrbute              获得属性值。
```



　　以[**百度**](javascript:;)首页为例：

```python
#coding:utf-8
from selenium import webdriverimport 
import time

brower = webdriver.Firefox()
brower.get("http://www.baidu.com")

#根据元素的类名进行定位
brower.find_element_by_class_name('s_ipt').send_keys('12306')
#根据元素的id进行定位
brower.find_element_by_id('kw').send_keys('12306')
#根据元素的name进行定位
brower.find_element_by_name('wd').send_keys('selenium')
#根据元素的xpath进行定位
brower.find_element_by_xpath("//input[@id = 'kw']").send_keys('selenium')
#根据元素的链接文本进行定位
brower.find_element_by_link_text(u'糯米').click()
#根据元素的CSS选择器进行定位

#----------------第一种id进行定位------------------
brower.find_element_by_css_selector('#kw').send_keys(u'定位')
brower.find_element_by_css_selector('#su').click()
#----------------第二种属性元素定位------------------
brower.find_element_by_css_selector('input[id = "kw"]').send_keys(u'自动化')
brower.find_element_by_css_selector('input[id = "su"]').click()
#----------------第三种标签进行定位------------------
brower.find_element_by_css_selector('input.s_ipt').send_keys(u'百度')
brower.find_element_by_css_selector('input.bg').click()
#----------------第四种class属性组合定位------------------
brower.find_element_by_css_selector('span.bg>input.s_ipt').send_keys(u'测试')
brower.find_element_by_css_selector('span.bg>input.bg').click()
#----------------第五种id属性组合定位------------------
brower.find_element_by_css_selector('span>input#kw').send_keys(u'地铁')
brower.find_element_by_css_selector('span>input#su').click()
#----------------第六种class属性定位------------------
brower.find_element_by_css_selector('.s_ipt').send_keys(u'高铁')
#或者
brower.find_element_by_css_selector('.s_btn').click()
brower.find_element_by_css_selector('.bg.s_btn').click()
#----------------第七种class与id组合定位------------------
brower.find_element_by_css_selector('.bg.s_ipt_wr.quickdelete-wrap>#kw').send_keys('selenium')
brower.find_element_by_css_selector('#su').click()
time.sleep(3)
brower.quit()
```

　

　注：用css定位时：

​	元素"id"  用  `#`

​	元素"class" 用 `·`





------

selenium定位元素，如何点击链接跳转页面

https://segmentfault.com/q/1010000012083117

------

selenium + python 中文文档
个人翻译英文文档，仅供参考，原文档地址(http://selenium-python.readthedocs.org/)

https://python-selenium-zh.readthedocs.io/zh_CN/latest/4.%E5%85%83%E7%B4%A0%E5%AE%9A%E4%BD%8D/

------



#### 参考

http://www.51testing.com/html/82/n-3714782.html