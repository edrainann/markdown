# (04)selenium模拟键盘操作出现  'value': keys_to_typing(value)})

当我尝试用python + selenium 模拟键盘事件的时候抛出了 `'value': keys_to_typing(value)})`这个异常，但是检查了N次也没有发现语句问题，后来google + 思考出了两个解决方法，希望对抛出该问题的童鞋们有点帮助。

![image.png](https://upload-images.jianshu.io/upload_images/1683050-e2d4eca35487de1c.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

```python
#首先需要引入Keys类包
from selenium.webdriver.common.keys import Keys
#模拟键盘操作语句
#通过回车键盘来代替点击操作
driver.find_element_by_id("su").send_keys(Keys.ENTER)

#在Keys里面查看有哪些常用键盘事件
 NULL = '\ue000'
    CANCEL = '\ue001'  # ^break
    HELP = '\ue002'
    BACKSPACE = '\ue003'
    BACK_SPACE = BACKSPACE
    TAB = '\ue004'
    CLEAR = '\ue005'
    RETURN = '\ue006'
    ENTER = '\ue007'
    SHIFT = '\ue008'
    LEFT_SHIFT = SHIFT
    CONTROL = '\ue009'
    LEFT_CONTROL = CONTROL
    ALT = '\ue00a'
    LEFT_ALT = ALT
    PAUSE = '\ue00b'
    ESCAPE = '\ue00c'
    SPACE = '\ue00d'
    PAGE_UP = '\ue00e'
    PAGE_DOWN = '\ue00f'
    END = '\ue010'
    HOME = '\ue011'
    LEFT = '\ue012'
    ARROW_LEFT = LEFT
    UP = '\ue013'
    ARROW_UP = UP
    RIGHT = '\ue014'
    ARROW_RIGHT = RIGHT
    DOWN = '\ue015'
    ARROW_DOWN = DOWN
    INSERT = '\ue016'
    DELETE = '\ue017'
    SEMICOLON = '\ue018'
    EQUALS = '\ue019'

    NUMPAD0 = '\ue01a'  # number pad keys
    NUMPAD1 = '\ue01b'
    NUMPAD2 = '\ue01c'
    NUMPAD3 = '\ue01d'
    NUMPAD4 = '\ue01e'
    NUMPAD5 = '\ue01f'
    NUMPAD6 = '\ue020'
    NUMPAD7 = '\ue021'
    NUMPAD8 = '\ue022'
    NUMPAD9 = '\ue023'
    MULTIPLY = '\ue024'
    ADD = '\ue025'
    SEPARATOR = '\ue026'
    SUBTRACT = '\ue027'
    DECIMAL = '\ue028'
    DIVIDE = '\ue029'

    F1 = '\ue031'  # function  keys
    F2 = '\ue032'
    F3 = '\ue033'
    F4 = '\ue034'
    F5 = '\ue035'
    F6 = '\ue036'
    F7 = '\ue037'
    F8 = '\ue038'
    F9 = '\ue039'
    F10 = '\ue03a'
    F11 = '\ue03b'
    F12 = '\ue03c'

    META = '\ue03d'
    COMMAND = '\ue03d'
```



------

**网络流传解决方案一**：

驱动的原因，由于驱动和浏览器版本不匹配导致的。

关于谷歌驱动和和浏览器的版本对照及下载可以在这里进行查找比对与下载：

chromedriver官方地址：http://chromedriver.chromium.org/，但是下载速度嘛都懂的...

传送门开启：<http://chromedriver.storage.googleapis.com/index.html> 

20180727更新的chromedriver与chrome的对应关系表：

| chromedriver版本 | 支持的Chrome版本 |
| ---------------- | ---------------- |
| v2.40            | v66-68           |
| v2.39            | v66-68           |
| v2.38            | v65-67           |
| v2.37            | v64-66           |
| v2.36            | v63-65           |
| v2.35            | v62-64           |
| v2.34            | v61-63           |
| v2.33            | v60-62           |
| v2.32            | v59-61           |
| v2.31            | v58-60           |
| v2.30            | v58-60           |
| v2.29            | v56-58           |
| v2.28            | v55-57           |
| v2.27            | v54-56           |
| v2.26            | v53-55           |
| v2.25            | v53-55           |
| v2.24            | v52-54           |
| v2.23            | v51-53           |
| v2.22            | v49-52           |
| v2.21            | v46-50           |
| v2.20            | v43-48           |
| v2.19            | v43-47           |
| v2.18            | v43-46           |
| v2.17            | v42-43           |
| v2.13            | v42-45           |
| v2.15            | v40-43           |
| v2.14            | v39-42           |
| v2.13            | v38-41           |
| v2.12            | v36-40           |
| v2.11            | v36-40           |
| v2.10            | v33-36           |
| v2.9             | v31-34           |
| v2.8             | v30-33           |
| v2.7             | v30-33           |
| v2.6             | v29-32           |
| v2.5             | v29-32           |
| v2.4             | v29-32           |



我是最新的chromedriver和chrome浏览器也报了这个错，并且它们真的是匹配的...于是有了-->

**可能有帮助的解决方案二**:

定位的时候要选择**文字框** 哈哈哈 是不是so easy😂

```python
#定位请选文字框
driver.find_element_by_id('kw').send_keys(Keys.PAGE_DOWN)
driver.find_element_by_xpath('//*[@id="kw"]').send_keys(Keys.PAGE_DOWN)
```





------

参考链接：

https://germey.gitbooks.io/python3webspider/1.2.3-ChromeDriver%E7%9A%84%E5%AE%89%E8%A3%85.html

https://blog.csdn.net/qq_32897143/article/details/79803127

https://blog.csdn.net/huilan_same/article/details/51896672





