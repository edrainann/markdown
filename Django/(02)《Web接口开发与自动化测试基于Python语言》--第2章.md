# (02)《Web接口开发与自动化测试基于Python语言》--第2章

## 第2章 Django入门

之所以会选择 Django Web 框架来做 Web（接口）开发，除了它功能强大之外，最主要的原因是资料丰富，在我看来这一点对于初学者尤为重要，如果要学习的一项技术的资料很少，那么在学习过程中遇到问题将会很难找到答案，从而容易受挫；相反，如果很容易找到答案，那么学习就会变得简单很多。
Django 是在 BSD 许可证下的开源项目。官方建议在 Python3 的最新版本下使用 Django，但你也可以在Python2.7 版本中使用它。

### 2.1 Django开发环境

Django 的版本大体分为三种：一种是长时期支持版本（Long Term Support，简称 LTS），第二种是最新版本，正式发布的稳定版本；第三种是预览版（一般版本号中带 a1、a2，b1，b2 的标识），主要为愿意尝试新功能的用户使用。

安装Django：

`pip install django==1.10.3`

或者

`pip3 install django==1.10.3`

或者

`python3 -m pip install django=1.10.3`

或者

`pip install -i https://pypi.douban.com/simple/ django=1.10.3`

如果你只安装一个版本的 Python，那么第一个命令即可成功安装 Django，后两个命令是在你同时安装了Python2.x 和 Python3.x 两版本的情况下，用于区别 Python2.x 时使用。

当然，对于访问国外网站比较慢的读者也可以选择豆瓣源，如第四行命令。

### 2.2 开始第一个Demo

按照惯例，我们应该先谈论一下什么是 Web 框架，以及 Django 的特性和架构。但对于新手来说，灌输这些概念并不能使我们真正的了解 Django，所以，我们先通过一个简单的 demo 来体会 Django 是如何工作的。
你可以在操作系统中创建一个 pydj/目录，用于存放我的练习项目。笔者在本书中将会在 D:\\pydj\目录来做练习。

#### 2.2.1 创建项目与应用

如果你已经成功的安装 Django，在.../python35/Scripts/目录中将会多出一个 django-admin.exe 文件。

![image.png](https://upload-images.jianshu.io/upload_images/1683050-bcd15d93800d95bb.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

在Windows 命令提示符下输入`django-admin`命令回车。

这里罗列了 Django 所提供给我们的命令，其中使用`startproject`命令来创建项目。

##### 2.2.1.1使用`startproject`命令创建guest项目：

`django-admin startproject guest	#创建 guest 项目`

该项目命名为“guest”。项目结构如下：

```
guest/
|----guest/
|        |---- __init__.py
|        |---- settings.py
|        |---- urls.py
|        |---- wsgi.py
|-------- manage.py    
```

其中：

- **__guest/init__.py**:一个空文件，标识这是一个目录为Python的标准包；
- **guest/settings.py**: Django项目的配置文件，包括Django模块应用配置、数据库配置、模板配置等；
- **guest/urls.py** :Django项目的URL声明；
- **guest/wsgi.py**:与WSGI兼容的Web服务器为你的项目提供服务的切入点；
- **manage.py**:一个命令行工具，可以让你在使用Django项目时以不同的方式进行交互。

```
D:\pydj>cd guest # 进入 guest 项目目录
D:\pydj\guest>python3 manage.py # 查看 manage 所提供的命令
```

##### 2.2.1.2使用`startapp`命令创建项目内的应用：

使用`startapp`命令创建应用。一个项目可以包含多个应用，而我们要开发的签到系统应该在具体应用下面完成。

`python manage.py startapp sign	#创建sign项目`

创建成功后目录结构如下：

```
.
├── guest
│   ├── __init__.py
│   ├── __init__.pyc
│   ├── settings.py
│   ├── settings.pyc
│   ├── urls.py
│   └── wsgi.py
├── manage.py
└── sign
    ├── migrations
    │   ├── __init__.py
    ├── __init__.py
    ├── admin.py
    ├── apps.py
    ├── models.py
    ├── tests.py
    └── views.py
```

其中：

- **migrations/：**用于记录models中数据的变更；
- **admin.py：**映射models中的数据到Django自带的admin后台；
- **apps.py：**用于应用程序的配置（在新的Django版本中新增文件）；
- **models.py：** Django的模型文件，创建应用程序数据表模型（对应数据库的相关操作）；
- **tests.py：**创建Django测试用例；
- **views.py：** Django的视图文件，控制向前段页面显示的内容。

#### 2.2.2运行项目

现在我们要把项目运行起来，Django 提供了 Web 容器，只需要通过“runserver”命令就可以把项目运行起来。

运行命令：

`python manage.py runserver`

Django 默认会通过本机的 8000 端口来启动项目，如果你的当前环境该端口号被占用了，也可以在启动时指定 IP 地址和端口号。

`python manage.py runserver 127.0.0.0.1:8001`

其中“127.0.0.1”为指向本机的 IP 地址，“8001”为设置的端口号。

打开浏览器，访问：http://127.0.0.1:8001/



#### 2.2.3 Hello Django！

大多编程语言的教程，第一个例子总是会教你如何打印“Hello xxx！”，我们也不免俗套，接下来和我一起开发一个“Hello Django!”的页面。

运行“Hello Django”，需要分别做如下修改：

在此之前，我们首先需要配置一下 guest/settings.py 文件，将 sign 应用添加到项目中。

1. 增加sign应用到项目中：在guest/settings.py中增加’sign’到INSTALLED_APPS；

   将 sign 应用添加到项目中

```python
……
# Application definition
INSTALLED_APPS = [
'django.contrib.admin',
'django.contrib.auth',
'django.contrib.contenttypes',
'django.contrib.sessions',
'django.contrib.messages',
'django.contrib.staticfiles',
'sign', #添加 sign 应用
]
……
```

在浏览器地址栏输入：http://127.0.0.1:8001/index/
2. 增加/index/的路由配置：在guest/urls.py中增加index路径；

   打开 guest/urls.py 文件添加该目录

```python
……
from django.conf.urls import url
from django.contrib import admin
from sign import views #导入 sign 应用 views 文件
urlpatterns = [
url(r'^admin/', admin.site.urls),
url(r'^index/$', views.index), #添加 index/路径配置
]
```
3. 增加index属性：在sign/views.py中增加index属性；

   打开../sign/views.py 文件创建 index 函数

```python
from django.http import HttpResponse
# Create your views here.
def index(request):
	return HttpResponse("Hello Django!")
```



4. 运行程序

`\guest>python3 manage.py runserver`

**注意：**

Index函数通过HttpResponse类向客户端返回指定内容“Hello Django”，HttpResponse类在django.http.HttpResponse中，以字符串的形式传递给客户端。

#### 2.2.4使用模板

使用HTML页面来代替”Hello Django!”字符串：

- 在sign/目录下创建templates/index.html，包含内容如下：

```html
<!DOCTYPE html>
<html>
    <head>
        <meta charset="utf-8">
        <title>Django Page</title>
    </head>
    <body>
        <h1>Hello Django!</h1>
    </body>
</html>
```

- 修改sign/views.py中的index函数为：

```python
from django.shortcuts import render

# Create your views here.
def index(request):
    return render(request, "index.html")
```

这里抛弃 HttpResponse 类 ，转而使用 Django 的 render 函数。

**Render**函数的第一个参数是请求对象的，第二个参数返回一个 index.html 页面。

### 2.3 Django工作流

Django的简单处理流程：

![image.png](https://upload-images.jianshu.io/upload_images/1683050-63b6dddf20e6bb00.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

需要说明的是，这个处理流程并非 Django 的完整处理过程，其中最主要的就是缺失了数据层（model）的操作，但目前我们并没有涉及这数据层的操作，所以先暂时忽略。

#### 2.3.1URL组成

- 协议类型HTTP/HTTPS：

  HTTP协议：HyperText Transfer Protocol，超文本传输协议；

  HTTPS协议：HyperText Transfer Protocol over Secure Socket Layer，套接层加密超文本传输协议。是以安全为目标的HTTP 通道，简单讲是 HTTP 的安全版。

- 主机地址baidu.com：网址或IP地址，通过域名解析服务器找到对应的IP主机。

- 端口号8000：不同的应用使用不同的端口号。

  一台主机上有很多应用，不同的应用占用不同的端口号，除了要指定主机（网址或 IP 地址）之外，还要进一步指定相应的端口号才能访问到具体的应用。
  前面在运行 Django 服务器，默认使用 8000 的端口号，所以，在浏览器除了输入 IP 地址之后，还要指向端口号，才能访问到 Django 应用。

- 路径/index/ 、/admin/：表示主机上的一个目录或文件地址。

#### 2.3.2URLs的配置

URLconf：URL configuration，为了给一个应用设计URL而创建的Python模块，包含内容是URL模式（简单的正则表达式）到视图函数（默认views.py文件中的函数）的映射关系。

Django处理一个请求的过程：

1. Django使用的是根URLconf模块，这个值通常是通过ROOT_URLCONF设置（settings.py文件）： 
   `ROOT_URLCONF = 'guest.urls'`
2. Django加载URLconf模块（urls.py文件）并寻找可用的urlpatterns。
3. Django依次匹配每个URL模式，在与请求的URL匹配的第一个模式处停下来。
4. 通过匹配的正则，Django将请求指向对应的视图函数处理。
5. 如果没有匹配到正则，或过程中出现异常，Django将调用适当的错误处理视图。

![image.png](https://upload-images.jianshu.io/upload_images/1683050-f8c4c77358247225.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

#### 2.3.2views视图

视图函数：简称视图，是一个简单的Python函数，它接受Web请求并且返回Web响应。

Web响应可以是：HTML网页、一个重定向、一个404错误、一个XML文档、一张图片……

视图在我看来，它在Django 中非常重要，是连接页面与数据的中间纽带。

拿登录的例子来讲，用户在页面上输入了用户名和密码点击登录。那么request 请求会由视图来接收，如何提取出用户名和密码的数据，如何用这些数据去查询数据库，再如何将登录成功的页面返回给用户，这些全部由视图层来完成。

#### 2.3.4templates模板

打开.../sign/templates/index.html 文件

![image.png](https://upload-images.jianshu.io/upload_images/1683050-d6d54ee0727e9229.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

模板的载体就是我们所熟悉的Web 页面了，Django 自带的有模板语言。它的主要作用是如何展示数据，比如视图层返回的是一个字符串，要如何显示在页面上；返回的对象数组要如何显示等。当然，为了使页面更漂亮需要借助前端技术，比如CSS、JavaScript 等。

### 2.4 MTV开发模式

这段话在Django官方文档也曾出现过：鼓励松耦合以及对应用程序中不同部分的严格分割。

MVC：Model-View-Controller

- model：数据存取层。
- view：表现逻辑，代表的是系统中选择显示什么和怎么显示的部分。
- controller：业务逻辑，代表系统中根据用户输入及需要访问模型，使用哪个视图的哪部分。

对应到Django后：

- Model：数据存取部分，由Django数据库层处理；
- View：表现逻辑，选择显示哪些数据要显示以及怎样显示的部分，由Django的视图和模板处理；
- Controller：业务逻辑，由Django根据URLconf设置，对给定URL调用适当的Python函数。

**由于Controller由框架自行处理，而Django里更关注的是模型（Model）、模板（Template）、视图（View），因此Django也被称为MTV框架。**

MTV：Model-Template-Views

- Model：模型，数据存取层，该层处理与数据相关的所有事务，即如何存取、如果验证有效等；
- Template：模板，表现层，该层处理与表现相关的所有事务，即如何在页面或者其他类型文档中进行显示；
- View：视图，业务逻辑层，该层包含存取模型及调取恰当模板的相关逻辑，可以看作是模型和模板之间的桥梁。

### 参考

http://blog.csdn.net/zhaoxz1985/article/details/72862466