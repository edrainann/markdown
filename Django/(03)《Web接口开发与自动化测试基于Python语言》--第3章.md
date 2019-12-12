# (03)《Web接口开发与自动化测试基于Python语言》–第3章

## 第3章 Django视图

以需求来驱动学习是笔者一直使用的学习方式，就是在学习一项技术之初，就确定好需求，要以该项技术实现一个具体需求。

我们一开始就已经定好了目标，就是要使用Django 开发一个发布会签到系统。
做为一个系统，那么**用户登录功能**必不可少。

这一章，我们将要开发一个用户登录功能。不要小看这个登录噢！它包含了不少需要学习知识点。

### 3.1 来写个登录功能

修改/guest/sign/templates/index.html：

```
<!DOCTYPE html>
<html>
    <head>
        <meta charset="utf-8">
        <title>Django Page</title>
    </head>
    <body>
        <h1>发布会管理</h1>
        <form>
            <input name="username" type="text" placeholder="username"><br>
            <input name="password" type="password" placeholder="password"><br>
            <button id="btn" type="submit">登录</button>
        </form>
    </body>
</html>
```

思考如下问题：

- 当用户输入用户名密码并点击登录按钮后，登录表单(form)中的数据以何种方式(GET/POST)提交到服务器端？
- Django系统如何验证用户名密码的正确性？
- 如果验证**成功**应该如何处理，跳转到什么页面？
- 如果验证**失败**应该如何处理，如何将错误提示返加给用户并跳转到什么页面？

下面一一解答上述的疑问。

#### 3.1.1GET与POST请求

当客户机通过HTTP 协议向服务器提交请求时，最常用到的方法是GET 和POST。

**GET**：从指定的资源请求数据；

**POST**：向指定的资源提交要被处理的数据。

##### GET请求

get方法传递参数，修改index.html，给form表单增加属性**method=”get”**：

```html
<form method="get">
    <input name="username" type="text" placeholder="input username in here"><br>
    <input name="password" type="password" placeholder="input password in here"><br>
    <button id="btn" type="submit">登录</button>
</form>
```

然后保存在index.html 文件，重新刷新页面。输入用户名、密码，点击登录。

输入用户名密码admin/admin123，点击登录按钮，浏览器的URL地址栏变为：

`http://127.0.0.1:8000/index/?username=admin&password=admin123`

get方法会将用户提交的数据添加到URL地址中，路径后面跟问号？。

其中username为HTML代码中<\input>标签的**name属性值**（name=”username”），username=admin 表示用户名输入框得到的输入数据为“admin”，多个参数之间用&符号分隔。password=admin123 密码输入框得到的输入数据为“admin123”。多个参数之间用“&”符号隔开。

【Tip】`placeholder`属性能够让你在文本框里显示提示信息，一旦你在文本框里输入了什么信息，提示信息就会隐藏。



##### POST请求

同样是上面的代码，再将form 表单的中的属性改为method="post" 。重新刷新页面后，再次输入用户名密码，点击“登录”。

```html
<form method="post">
    <input name="username" type="text" placeholder="input username in here"><br>
    <input name="password" type="password" placeholder="input password in here"><br>
    <button id="btn" type="submit">登录</button>
</form>
```

页面提示403错误：

![这里写图片描述](http://img.blog.csdn.net/20170605111601823?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvemhhb3h6MTk4NQ==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

**“CSRF verification failed. Request aborted.”**

**建议：**仔细阅读错误帮助信息有助于解决问题。

CSRF漏洞：Cross-Site Request Forgery，跨站请求伪造漏洞。

Django针对CSRF的保护措施是在生成的每个表单中放置一个自动生成的令牌，通过这个令牌判断POST请求是否来自同一个网站。

之前的模板都是纯粹的HTML，在这里要首次使用到Django 的模板，使用“模板标签”（ template tag）添加CSRF 令牌。在index.html的form 表单中添加`{% csrf_token %}`。

```html
<form method="post">
    <input name="username" type="text" placeholder="input username in here"><br>
    <input name="password" type="password" placeholder="input password in here"><br>
    <button id="btn" type="submit">登录</button>
    {% csrf_token %}
</form>
```

借助Firebug 前端调试工具进行查看POST 请求。你会看到除了usrname 和password 参数外，还多了一个csrfmiddlewaretoken 的参数。

当页面向Django 服务器发送一个POST 请求时，服务器端要求客户端加上csrfmiddlewaretoken 字段，该字段的值为当前会话ID 加上一个密钥的散列值。

```
Form Data
username:admin
password:admin123
csrfmiddlewaretoken:CEueSfB9S4bJQoW9x……n0OvGDAhWC9rIH2S7PegwZew
```

如果想忽略掉该检查，可以在/guest/settings.py文件中注释掉csrf：

```html
MIDDLEWARE = [
    'django.middleware.security.SecurityMiddleware',
    'django.contrib.sessions.middleware.SessionMiddleware',
    'django.middleware.common.CommonMiddleware',
    #'django.middleware.csrf.CsrfViewMiddleware',
    'django.contrib.auth.middleware.AuthenticationMiddleware',
    'django.contrib.messages.middleware.MessageMiddleware',
    'django.middleware.clickjacking.XFrameOptionsMiddleware',
]
```

#### 3.1.2处理登录请求

将表单中的数据提交给服务器的方式（GET/POST）。

Q:那么将登录数据提交给Django 服务器的谁来处理？

A:可以通过修改index.html，在form 表单的action 属性来指定提交的路径。

`<form method="post" action="/login_action/">`

这样，当我们再次点击登录按钮，将由<http://127.0.0.1:8000/login_action/>路径来提交登录请求。 
我们再来处理login_action，先修改/guest/urls.py文件增加login_action的路由：

```python
from sign import views

urlpatterns = [
    url(r'^login_action/$', views.login_action),
]
```

然后修改视图/guest/sign/views.py文件，创建login_action视图函数来处理验证信息：

```python
from django.http import HttpResponse
from django.shortcuts import render

# Create your views here.
def index(request):
    return render(request, "index.html")

# 登录动作 新创建得login_action
def login_action(request):
    if request.method == 'POST':
        username = request.POST.get('username', '')
        password = request.POST.get('password', '')
        if username == 'admin' and password == 'admin123':
            return HttpResponse("Login Success!")
        else:
            return render(request, 'index.html', {'error':'username or password error!'})
```

通过**login_aciton** 函数来处理登录请求。

客户端发送的请求信息全部包含在request 中。

关于如何获取request 中包含的信息，参考[Django 文档](https://docs.djangoproject.com/en/1.10/ref/request-response/)。

对上述代码进行分析：

1. 通过**request.method** 方法得到客户发送的请求方式，判断其是否为POST 请求类型；

2. 通过**request.POST** 来获取POST 请求。

   通过`.get()`方法来寻找name属性（还记得index.html的form里input name吗）为“username”和“password”的POST 参数，如果参数没有提交，返回一个空的字符串。此处的“username”和“password”对应form 表单中<input> 标签的name 属性，可见这个属性的重要性;

3. 通过if语句判断POST请求得到的username、password是否为“admin/admin123”。

   如果是，则通过**HttpResponse**类返回字符串“login success!”。

   如果不是，则通过**render**返回index.html登录页面，并且同时返回错误提示的字典“{'error': 'username or password error!'}”。

**注意：**

由于views.py里的login_action函数定义了登录失败时的错误提示，但是显然index.html 页面上并没有显示错误提示的地方，所以要同步修改index.html 页面中添加Django模板，增加错误信息展示的代码，修改如下：

```html
        <form method="post" action="/login_action/">
            <input name="username" type="text" placeholder="username" ><br>
            <input name="password" type="password" placeholder="password"><br>
            {{ error }}<br>
            <button id="btn" type="submit">登录</button>
            {% csrf_token %}
        </form>
```

这里依然是使用了Django模板的标签功能，添加**{{ error }}**，它对应render 返回字典中的key，并且在登录失败的页面中显示value，即**“username or password error!”**信息。

刷新/index/查看登录成功和登录失败的效果。

#### 3.1.3登录成功页

显然，登录成功返回的“login success!”字符串只是一种临时方案，只是为了方便验证登录的处理逻辑，现在没有问题之后，需要通过HTML 页面来替换。

我们要开发的是发布会签到系统， 那么我希望登录之后默认显示发布会列表。

所以， 首先创建.../templates/event_manage.html 页面。

```html
<!DOCTYPE html>
<html>
    <head>
        <meta charset="utf-8">
        <title>Event Manage Page</title>
    </head>
    <body>
        <h1>Login Success!！！</h1>
    </body>
</html>
```

同时修改视图/guest/sign/views.py中的login_action文件，

增加event_manage函数，用于返回发布会管理event_manage.html 面页：

```python

# 登录动作
def login_action(request):
    if request.method == 'POST':
        username = request.POST.get('username', '')
        password = request.POST.get('password', '')
        if username == 'admin' and password == 'admin123':
            #return HttpResponse("Login Success!")
            return HttpResponseRedirect('/event_manage/')
        else:
            return render(request, 'index.html', {'error':'username or password error!'})

# 发布会管理
def event_manage(request):
    return render(request, "event_manage.html")
```

**注意：** HttpResponseRedirect类，它可以对路径进行重定向，从而将登录成功之后的请求指向/event_mange/目录，即<http://127.0.0.1:8000/event_manage/>。

记得修改/guest/urls.py文件增加event_mange路由：

```
urlpatterns = [
    url(r'^admin/', admin.site.urls),
    url(r'^index/$', views.index),
    url(r'^login_action/$', views.login_action),
    url(r'^event_manage/$', views.event_manage),
]
```

### 3.2 Cookie和Session

通过在登录成功后，增加登录成功页显示“嘿，admin 你好！”，来研究Cookie和Session。

**Cookie机制：** **正统的Cookie 分发是通过扩展HTTP 协议来实现的，服务器通过在HTTP 的响应头中加上一行特殊的指示以提示浏览器按照指示生成相应的Cookie。**然而纯粹的客户端脚本如JavaScript 或者VBScript也可以生成Cookie。而Cookie 的使用是由**浏览器按照一定的原则在后台自动发送给服务器的。**浏览器检查所有存储的Cookie，如果某个Cookie 所声明的作用范围大于等于将要请求的资源所在的位置，则把该cookie 附在请求资源的HTTP 请求头上发送给服务器。

**Session机制：** Session机制是一种服务器端的机制，服务器使用一种类似于散列表的结构来保存信息。

#### 3.2.1Cookie的使用

这里我调整了下顺序，感觉按作者之前提到的Django的工作顺序来讲解代码的修改更合理：

- 首先，修改/guest/urls.py文件，由于还是使用上面提到的登录成功后的展示页面，所以依然使用event_mange路由，无需修改；
- 其次，修改/guest/sign/views.py文件，添加Cookie处理相关代码：

```
# 登录动作
def login_action(request):
    if request.method == 'POST':
        username = request.POST.get('username', '')
        password = request.POST.get('password', '')
        if username == 'admin' and password == 'admin123':
            #return HttpResponse("Login Success!")
            #return HttpResponseRedirect('/event_manage/')
            response = HttpResponseRedirect('/event_manage/')
            response.set_cookie('user', username, 3600) # 添加浏览器Cookie
            return response
        else:
            return render(request, 'index.html', {'error':'username or password error!'})

# 发布会管理
def event_manage(request):
    username = request.COOKIES.get('user', '') # 读取浏览器Cookie
    return render(request, "event_manage.html", {"user":username})
```

讲解上述代码，当用户登录成功后，在跳转到event_mange视图函数的过程中，通过**set_cookie()**方法向浏览器中添加Cookie信息。

set_cookie()方法传递了三个参数：**user**，用于表示写入浏览器的Cookie名；**username**，是由用户在登录页面上输入的用户名即admin（前面index.html的<input>的name属性）；**3600**，是用于设置Cookie信息在浏览器中的保持时间，单位为秒。

在event_mange视图函数中，通过**request.COOKIES**来读取Cookie名为“**user**”的值，并且通过**render**将它和event_mange.html页面一起返回。

- 最后，修改/guest/sign/templates/event_manage.html文件，添加<\div>标签来显示用户名：

```html
<!DOCTYPE html>
<html>
    <head>
        <meta charset="utf-8">
        <title>Event Manage Page</title>
    </head>
    <div style="float:right;">
        <a>嘿！{{ user }} 欢迎</a><hr/>
    </div>
    <body>
        <h1>Login Success!</h1>
    </body>
</html>
```

**注意：** 这里仍使用模板标签，user将会由views.py中的event_mange函数获取的username所代替。

最后重新登录查看效果，并且通过浏览器的F12功能查看POST请求的头部中Cookie信息为：

`Cookie:csrftoken=djsNd8BDErmFNZVXqg……9nZrDez5PgQFIaVLcVHhHcA1; user=admin`

#### 3.2.2 Session的使用

Cookie 固然好，但存在一定的安全隐患。Cookie 像我们以前用的存折，用户的存钱、取钱都会记录在这张存折上（即浏览器中会保存所有用户信息），那么对于有非分想法的人可能会去修改存折上的数据（这个比喻忽略掉银行同样会记录用户存取款的金额）。
相对于存折，银行卡要安全的得多，客户拿到的只是一个银行卡号（即浏览器只保留一个Sessionid），那么用户的存钱、取钱都会记录在银行的系统里（即服务器端），只得到一个sessionid 是没有任何意义的，所以相对于Cookie 来说就会安全很多。
在Django 中使用Session 和Cookie 类似。我们只用将Cookie 的几步操作替换成session 即可。

只需要修改/guest/sign/views.py文件，

在login_action函数中，将：

`response.set_cookie('user', username, 3600)`
替换为：
`request.session['user'] = username # 将session 信息记录到浏览器`

在event_manage 函数中，将：
`username = request.COOKIES.get('user', '')`
替换为：
`username = request.session.get('user', '') # 读取浏览器session`

```
# 登录动作
def login_action(request):
    if request.method == 'POST':
        username = request.POST.get('username', '')
        password = request.POST.get('password', '')
        if username == 'admin' and password == 'admin123':
            #return HttpResponse("Login Success!")
            #return HttpResponseRedirect('/event_manage/')
            response = HttpResponseRedirect('/event_manage/')
            #response.set_cookie('user', username, 3600) # 添加浏览器Cookie
            request.session['user'] = username           # 将session信息添加到浏览器
            return response
        else:
            return render(request, 'index.html', {'error':'username or password error!'})

# 发布会管理
def event_manage(request):
    #username = request.COOKIES.get('user', '') # 读取浏览器Cookie
    username = reuqest.session.get('user', '')  # 读取浏览器session
    return render(request, "event_manage.html", {"user":username})
```

再次刷新页面登录，得到了如下错误：

```
OperationalError at /login_action/
no such table: django_session
```

Q:为什么会有此错误？

A:这个错误跟Session 的机制有关，既然要服务器端记录用户的数据，那么一定要有地方来存放用户Sessionid 对应的信息才对。所以，我们需要创建django_session 表。别着急！

Django已经准备好这些常用表，只需要使用如下命令来生成即可：

```
\guest>python manage.py migrate
Operations to perform:
  Apply all migrations: admin, auth, contenttypes, sessions
Running migrations:
  Applying contenttypes.0001_initial... OK
  Applying auth.0001_initial... OK
  Applying admin.0001_initial... OK
  Applying admin.0002_logentry_remove_auto_add... OK
  Applying contenttypes.0002_remove_content_type_name... OK
  Applying auth.0002_alter_permission_name_max_length... OK
  Applying auth.0003_alter_user_email_max_length... OK
  Applying auth.0004_alter_user_username_opts... OK
  Applying auth.0005_alter_user_last_login_null... OK
  Applying auth.0006_require_contenttypes_0002... OK
  Applying auth.0007_alter_validators_add_error_messages... OK
  Applying auth.0008_alter_user_username_max_length... OK
  Applying sessions.0001_initial... OK
```

通过“**migrate**”命令进行数据迁移。

那么问题来了，我们并没有设置数据库，为什么已经生成了数据库表呢？ 
这是因为Django默认设置了SQLite3数据库，数据库配置存储在/guest/settings.py文件中：

```
# Database
# https://docs.djangoproject.com/en/1.10/ref/settings/#databases

DATABASES = {
    'default': {
        'ENGINE': 'django.db.backends.sqlite3',
        'NAME': os.path.join(BASE_DIR, 'db.sqlite3'),
    }
}
```

### 3.3 Django认证系统

到目前为止，虽然实现了登录，但显然用户登录信息的验证并未真正实现，目前的做法只是简单的用if语句判断用户名和密码是否为“admin/admin123”，所以，我们并没有完整的用户数据。

#### 3.3.1登录Admin后台

执行manage.py 的“**migrate**”命令时，Django 同时也帮我们生成了auth_user 表。同时，我们可以通过URL 地址：http://127.0.0.1:8000/admin/ 来访问Django 自带的Admin 管理后台。
在此之前，先通过**createsuperuser**命令来创建登录Admin 后台的管理员账号。

```
\guest>python3 manage.py createsuperuser
Username (leave blank to use 'edrain'): admin #输入用户名
Email address: admin@mail.com #输入邮箱
Password:admin123456	#输入密码
Password (again):admin123456	#重复输入密码
Superuser created successfully.
```

本例创建的超级管理员Admin帐号：admin/admin123456

Admin管理后台的地址是：<http://127.0.0.1:8000/admin/>

都是图形操作界面，不再做过多解释。可自己动手尝试增加用户。

#### 3.3.2引用Django认证登录

Django已经封装好了用户认证和登录的相关方法，只需要拿来用即可。并且，同样使用auth_user表中的数据进行验证，前面已经通过Admin后台向该表中添加了用户信息。

要达到此目的，只需要修改/guest/sign/views.py文件中的login_action函数：

```
from django.contrib import auth

# 登录动作
def login_action(request):
    if request.method == 'POST':
        username = request.POST.get('username', '')
        password = request.POST.get('password', '')
        user = auth.authenticate(username=username, password=password)
        if user is not None:
            auth.login(request, user)                   # 登录
        #if username == 'admin' and password == 'admin123':
            #return HttpResponse("Login Success!")
            #return HttpResponseRedirect('/event_manage/')
            #response.set_cookie('user', username, 3600) # 添加浏览器Cookie
            request.session['user'] = username           # 将session信息添加到浏览器
            response = HttpResponseRedirect('/event_manage/')
            return response
        else:
            return render(request, 'index.html', {'error':'username or password error!'})
```

**代码讲解：**

authenticate()函数认证给出的用户名和密码，它接受两个参数：username、password，并且会在用户名密码正确的情况下返回一个user对象，否则authenticate()返回None。

if语句对authenticate()返回对象进行判断，如果不为None，说明用户认证通过，则调用auth的login()函数进行登录，它接受两个参数：HttpRequest对象、一个user对象。

返回到index页面，尝试使用admin/admin123456和自定义用户进行登录。

#### 3.3.3关上窗户

在浏览器中直接访问：<http://127.0.0.1:8000/event_manage/>

不需要认证也可以访问这个页面，这是个漏洞，我们不允许用户不经认证就直接访问这些页面，所有需要将这个窗户关闭。

要做到这点，只需要再指定的**@login_required**视图函数前增加装饰器即可，如下修改/guest/sign/views.py文件：

```
from django.contrib.auth.decorators import login_required

# 发布会管理
@login_required
def event_manage(request):
    #username = request.COOKIES.get('user', '') # 读取浏览器Cookie
    username = request.session.get('user', '')  # 读取浏览器Session
    return render(request, "event_manage.html", {"user":username})
```

千万记得清空缓存，再次访问/event_manage/，Django会提示页面不存在：Page not found 404

我们可以发现一个问题，当访问@login_required装饰器的视图时，默认跳转的URL中会包含“/accounts/login/”，这里为什么不让它直接跳转到登录页面？不仅要告诉用户窗户是关着的，还指引用户到门的位置来进行登录操作呢？

想要达到这个目的，需要做如下修改，在/guest/urls.py文件中，增加新的路径配置：

```python
from sign import views

urlpatterns = [
    url(r'^$', views.index),
    url(r'^admin/', admin.site.urls),
    #url(r'^index/$', views.index),
    url(r'^login_action/$', views.login_action),
    url(r'^event_manage/$', views.event_manage),
    url(r'^accounts/login/$', views.index),	#关上窗户
]
```

当用户访问： 
[http://127.0.0.1:8000](http://127.0.0.1:8000/) 
<http://127.0.0.1:8000/index/> 
<http://127.0.0.1:8000/event_manage/> 
都会跳转到登录页面。但是如果尝试访问一个不存在的页面，Django依然会给出404错误。



### 参考

https://blog.csdn.net/zhaoxz1985/article/details/72866612