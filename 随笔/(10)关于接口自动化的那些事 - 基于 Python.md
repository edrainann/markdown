转自：https://testerhome.com/topics/8091

## 开始之前说一些废话

> 前段时间老大让我搞个接口自动化，好气啊我只想当个纯粹的点点点！（真没志气真没用，真给咱测试丢脸~！
后来我挑灯夜读，经过两个星期的挣扎终于把用例搞出来了！噢哈哈真是大快人心啊我真个聪明的家伙给自己一个[
>  我把做出来的成果贴出来，也顺便把学习的经过贴出来，大家来看看有什么地方可以做得更好的，一起交流学习呀，我会是一个悉心听教的好学生的~嗯！Begin~
## 准备工作

> 这部分其实在谷歌或者百度上搜索下就可以完成的，可是我就是想再啰嗦一遍，说不定有比我更懒的同学呢哈哈~

#### **第一步 Python的安装配置**

*   **打开官网**: [https://www.python.org/downloads/](https://www.python.org/downloads/) 目前官网上已经更新到3.6.1啦，有两个版本，大家可以按自己喜欢的去下载，我自己选择的是Python3.5，语法对比2.7版本的会有些改进，用2.7版本的小朋友贴我的代码是会报错的哦！ [![image](http://upload-images.jianshu.io/upload_images/1683050-e87903bdbfb51c5e.png!large?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)](https://testerhome.com/uploads/photo/2017/289f9d47d0dedd28ff2741cf5a9b0825.png!large) 

*   **安装并配置环境** 安装过程非常的简单，选择你想安装的目录，然后拼命的点击下一步，然后在最后一步的时候，记得记得记得要勾选底部的**“Add Python 3.5 to PATH”**，这样的话，你就不用手动去系统环境中配置啦，是不是很easy呢！妈妈再也不用担心你的学习啦~（最好再检查一下下啦，快捷键Windows+R ，打开CMD命令，输入“python”，如果没有报错，那就恭喜你，安装成功~） [![image](http://upload-images.jianshu.io/upload_images/1683050-e38b735a9a07ff10.png!large?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)](https://testerhome.com/uploads/photo/2017/241dc8373467f273024e102ea0b1a5d7.png!large) 

*   **Python小提示** 学好Python语言很重要哦~跟我一样的菜鸟们可以找一本Python基础教程，照着例子一个个去练习，不要偷懒哦，只有自己动手写了才会记得更牢呢，亲测有效！在这里推荐一个非常好的学习教程，传送门 -> [廖雪峰的Python教程](http://www.liaoxuefeng.com/wiki/0014316089557264a6b348958f449949df42a6d3a2e542c000/001431608990315a01b575e2ab041168ff0df194698afac000) 来来来，菜鸟们跟我念个顺口溜：学好Python真重要，走遍天下都不怕~（好像根本就不顺口啊！

    ）

#### 第二步 Python编辑器的选择及使用

*   终于又到了选择编辑器的矫情时刻了，据我短浅的头脑所知，目前比较流行的编辑器有Sublime Text3，Pycharm，Atom等，我自己有使用过Sublime Text3和Pycharm，个人感觉Pycharm比较符合我的编程习惯，它是Python重量级IDE，能自动检测语法，帮助使用者写出更规范的代码，大家如果有时间的话可以多去使用其它款编辑器，选一个自己最顺眼的，那码起代码来也会爽歪歪~

  

*   **Pycharm传送门** -> [http://www.jetbrains.com/pycharm/download/#section=windows](http://www.jetbrains.com/pycharm/download/#section=windows) 蹬蹬！！看到两个版本就开始头疼啦哈哈~别慌，毫不犹豫选择右边的社区版吧~ 人家专业版是留给大神级别的啦哈哈~ [图片上传失败...(image-e6051d-1542258838095)]

     （但据专业人士透露，社区版和专业版的区别并不大，一般的编程开发使用社区版就足够啦~所以宝宝们不需要太纠结版本~） [![image](http://upload-images.jianshu.io/upload_images/1683050-edea4bd3cfee0e83.png!large?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)](https://testerhome.com/uploads/photo/2017/7cfa9150e8b7ccc47b2478b8b74ddba9.png!large) 

#### 第三步 打开Python编辑器，开始练习吧

*   恭喜你完成了前面的步骤，到了这一步，咱们就可以开始练习python啦~（如果是选择了Pycharm的同学，有关于Pycharm的配置略复杂，在这里我就不详细解说啦，万能的谷歌可以帮助你~[图片上传失败...(image-2956c4-1542258838095)]

     ）

## 网络请求模拟小技巧

> 在学习了一段时间的Python语言后，咱也大概对Python的语法和逻辑有了一定的积累，接下来，可以为接口自动化测试开始尝试做一些比较简单的准备工作啦~跟着我一起来来来~

#### 扩展库requests

> 一般来说接口测试都是基于HTTP和HTTPS的网络请求，Python中有很多自带原生库和扩展库均可以实现。Python模拟HTTP请求有两种方式，一种是使用httplib模块，一种是使用requests模块，我个人比较倾向于使用requests库，该库把请求的框架都搭建好了，使用简洁明了呢~[图片上传失败...(image-e93596-1542258838095)]

*   **使用requests**

首先要安装及导入扩展库requests，咱可以使用pip来扩展第三方库，打开CMD，输入**pip install requests**，安装完成后，打开编辑器，练习使用requests库，废话不多说，直接上代码~

**GET请求方式模拟：**

```
import requests      #调用requests库test_url = 'http://xxx:8080.com'      #访问接口的url地址response = requests.get(test_url)       #发起一个请求，使用get方法result = response.text          #读取请求返回的结果print(result)     #打印返回的结果
```

**POST请求方式模拟：**

```
import requests      #调用requests库username = 'ningxw'    #定义参数usernamepassword = '123456'   #定义参数passwordtest_url = 'http://xxx:8080.com'      #访问登录接口的url地址datalist = {'uname': username, 'pwd': password}    #将参数添加到需求post的data中head = {"Content-Type": "application/Json"}   #定义头部response = requests.post(test_url, datas=datalist， headers=head)   #发起一个请求，使用post方法result = response.text          #读取请求返回的结果print(result)   #打印返回的结果
```

> 其实我的废话是可以不用说这么多的，给你们分享一个快速学习requests的干货，传送门 -> [快速上手 - Requests](http://stackoverflow.com/questions/9733638/post-json-using-python-requests) ，但是我就是想要展示下我的才气啊，我要表露出我博学多才的气质！哼！

## 简单的接口自动化测试用例

> 恭喜你啊恭喜我啊，能把上面的网络请求掌握了，咱就可以继续下一步了~因为懂得了如何向接口请求数据，那我们就可以根据返回的数据来进行断言啦，断言是接口测试的重要部分哦~
> 


#### 做接口测试前的准备

*   **理解业务需求**

**所有测试的基础，都离不开对业务的理解，接口测试也是一样的**~只有搞清楚业务的需求和逻辑，咱们才可以设计出一个比较好的用例哦~（我感觉我在讲废话！

 ）

**了解业务需求**，比较快速简单的做法是可以从产品经理那边要求获取一份项目的原型，一般原型里面会把项目的内容说得很清楚，里面包含了比较重要的业务逻辑~

**了解代码逻辑**，可以通过开发已完成的代码去理解代码逻辑和业务逻辑，显然这部分对于大部分测试包括身为菜鸟的我是一样的难，但不得不说这是一个通往更高层面的测试的必经之路，开发的代码里面会包括多个开发的沟通协调及测试人员本身的编程基础和测试思想，为了不让自己成为轻易取代的人，咱必须得努力起来呀~干呀~

*   **查看规范的接口文档**

咱做接口测试，就得对接口有深入的了解，所以**一份规范的接口文档会让接口自动化测试事半功倍**~但是呢，要沉淀出一份规范的接口文档，对开发来说无疑是增加重复劳动力的事情，而很多开发也不大愿意想帮助测试做些什么事~（我就常常被之前公司的开发鄙视，说我不务正业，瞎搞东西，让我做好点点点工作就好！真是好气哦！！

 ）所以说呀，凡事还是靠自己呀，要是咱掌握了代码的能力，我们就可以自己去看代码了呀，就可以自己整理出接口文档，又不用有求于人，说不定还可以被上司赏识，从此升职加薪走上人生巅峰迎娶白富美…

 好了，想想就好了，踏实的工作最重要呢~~

最后最后，我有一份不太规范的接口文档，但是对我来说也已经足够了，给你们这些没瞧过接口文档的小小菜鸟看看，长长见识哈哈哈~~（拥有规范的接口文档的大神请无视我就可以了~感激不尽~
 ）

[](https://testerhome.com/uploads/photo/2017/f74877f524dbf3701a7f799132959255.png!large)

[![image](http://upload-images.jianshu.io/upload_images/1683050-eacc2c67c671d15a.png!large?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)](https://testerhome.com/uploads/photo/2017/f74877f524dbf3701a7f799132959255.png!large) 

*   **编写代码完成接口测试**

噔噔噔噔~一把鼻涕一把泪的终于要来到写代码测试接口的时刻了

 ~在理清了业务需求和逻辑后，我们就可以开始试着去写一些请求了，看看接口返回的内容是不是我们预期的结果，然后通过断言来判断测试的结果~在写一个较为完整的用例之前，我们还是需要先**巩固Python编程**，了解一下有关**数组**的内容，**json数据格式及转换方式**，**unittest框架**等，这样你才可以在写代码的时候游刃有余噢！！
贴一下我常用的接口自动化测试代码的基本格式和内容：

```
import unittest       #导入unittestimport requests     #导入requests库import json            #导入jsonclass LianXi(unittest.TestCase):              #定义一个类，类的首字母要大写哦       def setUp(self):                                 #初始化             self.base_url = 'http://ip:端口号/url地址'             def test_get_success(self):             #定义一个方法，切记要以test开头哦             datalist = {'参数1': '值1', '参数2': '值2'}               #定义传参数据             head = {"Content-Type": "application/Json"}     #定义头部             r = requests.post(self.base_url, params=datalist, headers=head)          #传入参数             result = json.loads(r.text)            #使用json格式返回             self.assertEqual(result['status'], 0)      #检验返回值             print(result)if __name__ == '__main__':      unittest.main()
```

以上就是我编写自动化用例的基本模板了，才疏学浅啊各位大神多多指教~[图片上传失败...(image-efb08a-1542258838098)]

在编写测试代码的时候，会很纠结在post数据的时候，应该选用哪个关键字参数，也许聪明的你熟读上面我分享的干货《快速上手 - Requests》就可以搞懂，但是蠢萌的我还是没能完全掌握啊！

 于是在我无数次的谷歌之后，我找到一个可以暂时解答我疑惑的回答，传送门 -> [http://stackoverflow.com/questions/9733638/post-json-using-python-requests](http://stackoverflow.com/questions/9733638/post-json-using-python-requests) ，这部分还是需要多花点时间去学习和巩固哦~ 

## 接口自动化实践

> 前面啰啰嗦嗦的写了这么多基础的东西，目的就是为了让比我还菜鸟的宝宝们方便学习~到了这一章，估计也是本文的最后一章啦啦啦，咱们就来说说接口自动化的实践吧~宝宝们，带上你的大脑，跟着我一起high high high~（一言不合就想唱歌的老毛病居然犯了~
> 
>  ）

我现在做的项目是一个提供用户查询物流信息的系统，就给大家讲解一个最简单的**“增加单号”**接口的例子吧~

*   **业务需求**

我们把“增加单号”的业务需求逐条提炼出来~

| 编号 | 业务步骤 |
| 1 | 用户登录 |
| 2 | 输入单号 |
| 3 | 检验单号是否重复 |
| 4 | 检验单号是否超过今日追踪数量 |
| 5 | 增加单号成功 |

*   **根据业务，定义需要处理的函数**

| 编号 | 函数 |
| 1 | 初始化数据：setup()，用来初始化一些比较常用到的数据 |
| 2 | 用户登录：test_getcookie() ，用来获取用户登录的cookie |
| 3 | 增加单号成功：test_addtrackno_success()，用来测试增加单号接口 |

*   **初步的自动化框架**

根据定义的函数，可以初步搭建出一个简陋的自动化框架，就是下面那一坨~

```
class AddTrackNo():         def setup():         #初始化数据         def test_getcookie():         #获取用户登录的cookie         def test_addtrackno_success():         #测试增加单号接口
```

看到这里，我知道有人想对我的设计以深深的鄙视来吐槽我，来啊造作啊，反正你又打不着我！！哈哈哈~~[图片上传失败...(image-c06fd0-1542258838098)]

 [图片上传失败...(image-81a3f6-1542258838098)]

*   **还是简单粗暴点，上代码吧**

上代码前啊我就是想装逼说几句，如今这世道都是套路，不过只要你掌握了其中的思想和工具，那就不在怕的了！就一个字，上！！

```
# -*- coding: utf-8 -*-#导入各种库import requestsimport jsonimport unittestimport pymssqlimport xlrdimport reclass AddTrackNo(unittest.TestCase):    '''“（批量）增加单号”接口'''    def setUp(self):        '''初始化数据'''        self.user_url = 'https://xxxxxxuser/call'        self.buyer_url = 'https://xxxxxxorder/call'        '''查询数据库中登录用户的信息'''        self.conn = pymssql.connect(host='192.xx.xx.xx', user='sa', password='123456', database='Test_Buyer1')        cur = self.conn.cursor()        sql = 'SELECT TrackNO FROM TrackInfo02 WHERE (Email = '123@qq.com')'        cur.execute(sql)        self.result = cur.fetchall()    def tearDown(self):        '''关闭数据库'''        self.conn.close()    def test_getcookie(self):        '''用户登录，获取cookie<账号：123@qq.com>'''        data = {"Version": "xxx",                "Method": "Signin",                "SourceType": xx,                "Param":                    {"Email": "123@qq.com",                     "Password": "123456"}                }        req = requests.post(self.user_url, json=data)        res = json.loads(req.text)        self.assertEqual(res["Code"], 0)        self.assertEqual(res['Json']['FEmail'], '123@qq.com')        #print(res)        cookie = res["Cookies"]        return cookie       #返回用户cookie    def test_addtrackno_success(self):        '''成功_增加单号(状态码：0 保存成功)'''        '''查询数据库中该用户下的单号'''        result = self.result        '''获取EXCEL表内的单号'''        file = xlrd.open_workbook(r'C:\Users\xx\test_trackno.xlsx')        sheet = file.sheet_by_name('单号数据')        cols = sheet.col_values(0)        n = len(cols)        '''去除重复单号，并post增加单号'''        index = 0        arylist=[]        for row in result:            a = re.findall('[a-zA-Z0-9]', str(row))            sj = ''.join(a)            arylist.append(sj)            index += 1        j = 0        for i in range(n):            if(j == 5):                break            if(cols[i] not in arylist):                if len(result)>= 40:                    break                else:                    j += 1                    datalist = {"Version": "xxx",                             "Method": "AddTrackNo",                             "Cookies": self.test_getcookie(),                             "SourceType": xx,                             "Param": {                                "TrackNos": [cols[i]]}                             }                    req1  = requests.post(self.buyer_url, json=datalist, headers={"Content-Type": "application/Json"} )                    res = json.loads(req1.text)                    print(res)                    self.assertEqual(res['Code'], 0)                    self.assertEqual(res['Json']['Items'][0]['ResultCode'], 0)
```

*   **完善自动化测试用例**

以上展示的就是其中一个接口的设计和实现，是不是很简单呢！

 在实际的项目中肯定会遇到很多很多接口，那咱写完一个接口，肯定不想一个个的跑起来呀，那我们就可以再创建一个文件，使其做自动化跑接口用例~

Run_Tests.py代码如下：

```
import time, osimport sysimport unittestfrom HTMLTestRunner import HTMLTestRunner     #引入HTMLTestRunner模板sys.path.append('./Interface')    test_dir = './interface'       #指定当前文件夹下的Interface目录file = unittest.defaultTestLoader.discover(test_dir, pattern='*_test.py')    #匹配开头为test的py文件if __name__=="__main__":    now = time.strftime("%Y-%m-%d %H-%M-%S", time.localtime(time.time()))    # 取当前时间    public_path = os.path.dirname(os.path.abspath(sys.argv[0]))       # 获取当前运行的.py文件所在的绝对路径    filename = public_path + "\\Report\\" + now + "report.html"   #保存的报告路径和名称    fp = open(filename, 'wb')    runner = HTMLTestRunner(stream=fp,                            title="接口自动化报告",                            description="详细描述如下："                            )    runner.run(file)     #执行测试套件    fp.close()
```

运行该文件，就可以把很多很多接口用例跑起来啦~

 代码写得就跟我一样的粗糙！！不过都是新鲜出炉的哈，以后有时间，我会认认真真去规范代码，争取做个努力上进的菜鸟~（认认真真规范的代码增加啦已经 ~~~ 耶耶耶 ~~~ 
 ）

## 优化接口自动化用例

> 啦啦啦啦啦~ 努力上进的小菜鸟又花了一些时间优化了下代码结构
> 
>  上面的一大部分内容已经讲解了如何搭建一个丑陋的自动化接口测试框架，但是呢，爱美嘛是人的天性，自己撸出来的代码也要美化一下呀，不然怎么对得起自己的臭美天性呢哈哈~

>  来来来，一起来看看这个“整容”过程吧 ~ 嘻嘻 ~

*   **增加配置文件（conf.ini，请注意内容，有血的教训之提示！！）**

**conf.ini** （主要配置数据库、url、用户信息、状态码等常用数据信息）

```
[test_db]#数据库信息（注意不要带上引号哦，不然会报错哦！！！）host = 192.168.xx.xxuser = sapassword = sa123db_test_buyer = Test_Buyer1[user_info]#用户信息email = "123@qq.com"upassword = "123456"[url]#请求的接口地址（url不要带上引号哦，不然会报错哦！！！）user_url = https://xxxxxxuser/call[code]#成功状态success = 0#追踪号已存在，已添加该单号TrackNoIsExist = -11010101#追踪号无效TrackNoInvalid = -11010102
```

*   **初始化配置文件config.py（初始化并封装配置文件的数据）**

```
import configparser    #导入configparser库，用于读取配置文件import os class Config():    def __init__(self):        self.config = configparser.ConfigParser()        self.conf_path = os.path.join(os.path.dirname(os.path.abspath(__file__)), 'conf.ini')    #获取制定配置文件所在路径        self.config.read(self.conf_path, encoding='utf-8-sig')     #读取配置文件，编码格式为utf-8-sig        self.conf = {            'host': '', 'user': '', 'password': '', 'db_test_buyer': '', 'email': '', 'upassword': '', 'user_url': '', 'success': '', 'TrackNoIsExist ': '', 'TrackNoInvalid ': ''                        }        def get_conf(self):        """        配置文件读取，并赋值给全局参数        :return:        """        self.conf['host'] = self.config.get("test_db", 'host')        self.conf['user'] = self.config.get("test_db", "user")        self.conf['password'] = self.config.get("test_db", "password")        self.conf['db_test_buyer'] = self.config.get("test_db", "db_test_buyer")        . . . . . .         return self.confif __name__ == '__main__':    Config()
```

*   **增加公共文件Common_Func.py（主要配置数据库连接、获取用户cookie等常用函数）**

```
import Conf.config          #导入Conf文件夹下的config.py文件，用于初始化配置文件import pymssql                #导入pymssql，用于连接SqlServerimport requestsimport jsondef Get_TrackNo():    '''初始化数据'''    c = Conf.config.Config().get_conf()   #调用config.py文件的get_conf()函数    '''数据库配置'''    db_host = c["host"]    db_user = c["user"]    db_password = c["password"]    db_test_buyer = c["db_test_buyer"]    db_test_user = c["db_test_user"]    '''连接SqlServer数据库'''    conn_buyer = pymssql.connect(host=db_host, user=db_user, password=db_password, database=db_test_buyer)    cur_buyer = conn_buyer.cursor()    sql_buyer =sql = 'SELECT TrackNO FROM TrackInfo02 WHERE (Email = '123@qq.com')'    cur_buyer.execute(sql_buyer)    trackno_result = cur_buyer.fetchall()    '''关闭数据库连接'''    cur_buyer.close()    cur_user.close()    return trackno_result      #返回单号列表结果def Get_Cookie():    '''用户登录，获取cookie<账号：123@qq.com>'''    c = Conf.config.Config().get_conf()   #调用config.py文件的get_conf()函数    user_url = c["user_url"]    user_email =c["email"]    user_pwd = c["upassword"]    data = {"Version": "xxx",            "Method": "Signin",            "SourceType": xxx,            "Param":                {"Email": user_email,                "Password": user_pwd }                }    req = requests.post(user_url, json=data)    uid = req.cookies.get('uid')    return uid                              #返回用户cookie
```

*   **修改“增加单号”接口文件Addtrackno_test.py**

```
# -*- coding: utf-8 -*-import requestsimport jsonimport unittestimport xlrdimport reimport Conf.config     #导入配置文件import Common_Func      #导入公共文件Common_Func.pyclass AddTrackNo(unittest.TestCase):    '''“（批量）增加单号”接口'''    def setUp(self):        '''初始化数据'''        '''获取请求地址'''        c = Conf.config.Config().get_conf()   #调用config.py文件的get_conf()函数        self.buyer = c["buyer_url"]        '''获取状态码'''        self.success = c["success"]      #成功的状态码        self.TrackNoIsExist = c["TrackNoIsExist"]      #其它状态码        '''获取某账号下的单号列表'''        self.trackno_result = Common_Func.Get_TrackNo()        '''获取某账号的cookie值'''        self.user_cookie = Common_Func.Get_Cookie()    def tearDown(self):        pass    def test_addtrackno_success(self):        '''成功_（批量）增加单号(状态码：0 保存成功)'''        '''获取某账号下的单号列表'''        result = self.trackno_result        '''获取EXCEL表内的单号'''        file = xlrd.open_workbook(r'C:\Users\xx\test_trackno.xlsx')        sheet = file.sheet_by_name('Sheet4')        cols = sheet.col_values(0)        n = len(cols)        '''去除重复单号，并post增加单号'''        index = 0        arylist=[]        for row in result:            a = re.findall('[a-zA-Z0-9]', str(row))            sj = ''.join(a)            arylist.append(sj)            index += 1        j = 0        for i in range(n):            if(j == 5):                break            if(cols[i] not in arylist):                if len(result)>= 40:                    break                else:                    j += 1                    data1 = {"Version": "1",                             "Method": "AddTrackNo",                             "Cookies": self.user_cookie,                             "SourceType": 0,                             "Param": {                                "TrackNos": [cols[i]]}                             }                    req1 = requests.post(self.buyer_url, json=data1, headers={"Content-Type": "application/Json"} )                    res = json.loads(req1.text)                    print(res)                    self.assertEqual(res['Code'], self.success)        #使用配置文件数据进行断言                    self.assertEqual(res['Json']['Items'][0]['ResultCode'], self.success)     #使用配置文件数据进行断言
```

*   **修改Run_Tests.py**

```
import time, osimport syssys.path.append('./Interface')import Conf.config   #导入配置文件import unittestfrom HTMLTestRunner import HTMLTestRunnertest_dir = './Interface'file = unittest.defaultTestLoader.discover(test_dir, pattern='*_test.py')if __name__ == '__main__':    Conf.config.Config().get_conf()      #初始化配置文件    now = time.strftime("%Y-%m-%d %H-%M-%S", time.localtime(time.time()))    public_path = os.path.dirname(os.path.abspath(sys.argv[0]))    filename = public_path + "\\Report\\" + now + "report.html"    fp = open(filename, 'wb')    runner = HTMLTestRunner(stream=fp,                            title="接口自动化报告",                            description="详细描述如下："                            )    runner.run(file)    fp.close()
```

*   **接口自动化测试框架结构**

> 小伙伴们看到这么多乱糟糟的代码肯定是已经头晕眼花，想马上关闭页面放弃治疗了是吧哈哈~下面本宝宝给大家列出了本次测试框架的结构，很清晰明了辣~大家记得要在每个文件夹下加上init.py文件哦，不懂为什么要这么做的小伙伴，谷歌和百度搜搜搜就出来啦！！本宝宝脑力有限，就将就看看吧哈哈哈我不管啦！

```
Conf/  |-- init.py  |-- conf.ini   |-- config.pyInterface/   |-- init.py   |-- Common_Func.py.py   |-- Addtrackno_test.py Report/   |-- report.html init.pyRun_Tests.py 
```

## 总结

> 好啦，看到这个标题，你就知道我想说什么啦~花了整整一天的时间去编写这篇文章，还要担心产品和开发过来找我定位问题，我可是冒着生命危险在给大家伙分享我的学习经历和成果呢！！
> 
>  自己写完也检查好几遍了，以我有限的脑力和胡言乱语，我完成了人生中第一篇博客分享~这感觉就像是拱了一颗优质大白菜，哈哈哈哈哈~
> 
>  各路兄弟姐妹，很感谢你能看到这里，同时也特别希望你们可以给我提出意见，在测试的这条路上，我始终都是自己一个人瞎搞乱搞，搞出来的东西也没人帮我检验正误，搞不出来的东西我也还是搞不出来~
> 
>  但是好在我肯努力呀，肯学呀，不会就是不会，我从来不会掩饰自己的无知，但也不会卖弄自己的本事，其实就是没本事卖弄而已
> 
>  期待大家给与我的意见~嗯！End~！