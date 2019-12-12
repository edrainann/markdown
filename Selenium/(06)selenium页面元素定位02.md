

比较好用的一些元素定位

## 一、Xpath

### 1、基本常用

页面上

```html
<input type="text" value="" name="IDCard" id="iptIDCard" maxlength="18" autocomplete="off" placeholder="请输入">
```

selenium + python 获取元素

```python
driver.find_element_by_xpath('//*[@id="iptIDCard"]').send_keys(idcard)	#输入身份证号码
```



### 2、确定页面上显示元素的文字

```python
element = driver.find_element_by_xpath('//*[@id="app"]/div/div/div/div[2]/div/div/div[2]/div[4]/a').text
        print("element的值是：",element )
```



### 3、直接通过页面上显示的文字来获取元素

页面上

```html
<div unselectable="unselectable" class="ant-select-selection__placeholder" style="display: none; user-select: none;">请选择</div>
```

selenium + python 获取元素

```python
driver.find_element_by_xpath('//div[contains(text(),"请选择")]').click()
# 当不确定是什么标签时
driver.find_element_by_xpath('//*[contains(text(),"请选择")]').click()
```



### 4、通过多个标签关联，查找出想要的元素

页面上

```html
<input type="text" value="哇哈哈" placeholder="身份证姓名">
```

selenium + python 获取元素

```python
driver.find_element_by_xpath("//input[@type='text'][@value='哇哈哈']").clear()  # 调用clear()方法去清除名字
# 或者是通过 placeholder=“文字”进行删除
# placeholder 是当输入框什么都没有输入时显示的内容，value 是显示输入框中输入的文字内容
driver.find_element_by_xpath("//input[@type='text'][@placeholder='身份证姓名']").clear()
```



### 5、找出当前标签下的兄弟标签

页面上

```html
<div style="display: flex; box-sizing: border-box; width: 100%; height: 100%; align-items: center; justify-content: center; flex-direction: column;">
    <img src="/supplychain/main//images/add.png" style="height: 38px; width: auto;">
    <div style="display: flex; box-sizing: border-box; align-items: center; justify-content: center; font-size: 10px; color: rgb(179, 179, 179); margin-top: 8px;">我的照片</div>
    <input type="file" style="display: none;">
    <canvas style="display: none;"></canvas>
</div>
```

selenium + python 获取元素

```python
# 通过页面上的文字找到元素，但由于上传照片需要input，所以利用 following-sibling::input，找到input元素
driver.find_element_by_xpath('//*[contains(text(),"我的照片")]/following-sibling::input').send_keys('H:\\哇哈哈\\photo.jpg')
```



## 二、class_name

### 1、基本常用

页面上

```html
<a class="code-btn ">获取验证码</a>
```

selenium + python 获取元素

```python
driver.find_element_by_class_name('code-btn ').click()      # 点击 获取验证码
```



## 三、元素获取超时

如果发现A元素时，执行test01；如果没有发现A元素时，执行test02

```python
from selenium.common.exceptions import NoSuchElementException, TimeoutException
class Tool(object):
    def __init__(self, driver):
        self.driver = driver  # 这里不能再文件顶部导入webdriver，不然之后会有冲突
	def is_element_exit(self, identifyBy, c):
	    '''
	    Determine whether elements exist
	    Usage:
	    isElement(By.XPATH,"//a")
	    '''
	    flag = None
	    try:
	        if identifyBy == "xpath":
	            # self.driver.find_element_by_xpath(c)
	            wait = WebDriverWait(self.driver, 3)
	            wait.until(EC.presence_of_element_located((By.XPATH, c)))  # 等待该xpath元素最多3s，找到则返回元素，否则抛异常
	            # WebDriverWait(self.driver, 3, 0.8).until(lambda x: x.find_element_by_xpath(c))  # 等待XPATH元素可见 
	        elif identifyBy == "id":
	            self.driver.implicitly_wait(60)
	        	self.driver.find_element_by_id(c)
	        	self.driver.find_element_by_id(c).click()
	        elif identifyBy == "name":
	             self.driver.find_element_by_name(c)
	             self.driver.find_element_by_name(c).click()
	        flag = True
	        print("can find: ", c)
	    # except NoSuchElementException as e:
	    except TimeoutException as e:
	        flag = False
	        print("获取元素超时因为{}，无法找到{}元素", format(e, c))
	        print("can't find: %s" % c)
	    finally:
	        return flag
```

调用这个方法

```python
from common.tool import Tool

class TestEdrain(unittest.TestCase):
	def test_Flow(self):
	        driver = self.driver
	        driver.get(self.base_url)
	        tool = Tool(driver)
	        if tool.is_element_exit("xpath", '//*[@id="iptIDCard"]'):
	            print("开始注册")
	            edrain.test01()
	        else:
	            print("开始登录")
	            edrain.test02()
 
```

TimeoutException：获取元素超时

NoSuchElementException：找不到该元素

等异常均会被捕捉。

小tip：

```
# 最长的等待时间取决于两者之间的大者。
# 如果隐性等待时间 > 显性等待时间，则该句代码的最长等待时间等于隐性等待时间。
 """
implicitly_wait():隐式等待
 当使用了隐式等待执行测试的时候，如果 WebDriver没有在 DOM中找到元素，将继续等待，超出设定时间后则抛出找不到元素的异常。换句话说，当查找元素或元素并没有立即出现的时候，隐式等待将等待一段时间再查找 DOM，默认的时间是0
  一旦设置了隐式等待，则它存在整个 WebDriver 对象实例的声明周期中，隐式等待会让一个正常响应的应用的测试变慢，它将会在寻找每个元素的时候都进行等待，这样会增加整个测试执行的时间。
  所以：：：隐式等待 慎用。
  """
---------
driver.implicitly_wait(30)  # 隐式等待
--------
wait = WebDriverWait(self.driver, 3)
wait.until(EC.presence_of_element_located((By.XPATH, c)))  # 显示等待
```



## 四、断言

1、包含关系assertIn(A, B)------B 中包含 A

```python
ele_string = driver.find_element_by_xpath('//*[@id="app"]/div/div/div[2]/div/div/div[1]/p[1]').text
assertIn(u"系统正在审批您提交的申请", ele_string)
```

2、相等关系assertEqual(A, B)------A和B相等

```python
assertEqual(a,b,msg=msg)   #判断a与.b是否一致，msg类似备注，可以为空
```



