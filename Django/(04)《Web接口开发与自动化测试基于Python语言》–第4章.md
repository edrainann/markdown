

# (04)《Web接口开发与自动化测试基于Python语言》–第4章

[TOC]



## 第4章 Django模型

​	在Web 应用中，主观逻辑经常牵涉到与数据库的交互。以数据库驱动网站在后台连接数据库服务器，从中取出一些数据，然后在Web 页面用漂亮的格式展示这些数据。这个网站也可能会向访问者提供修改数据库数据的方法。许多复杂的网站都提供了以上两个功能的某种结合。
​	对于我们的发布会签到系统来说，也是以**数据管理为主**的网站，主要管理发布会和嘉宾数据。有一个观点，对于**数据驱动**的Web 系统，数据库表的设计完成，就相当于Web 系统已经完成了一半，可见数据库表的设计难度，以及在Web 开发中的重要性。

### 4.1 设计系统表

Django 提供完善的模型（model）层主要用来创建和存取数据，不需要我们直接对数据库操作。

**Django模型的基础知识：**

- 每个模型是一个Python类，继承**django.db.models.Model类**
- 该模型的每个属性表示一个数据库表字段
- 所有这一切，已经给了你一个自动生成数据库访问的API

**本例所需的模型，修改/guest/sign/models.py文件：**

```
#! /usr/bin python
# -*- coding:utf-8 -*-

from __future__ import unicode_literals

from django.db import models

# Create your models here.

# 发布会表
class Event(models.Model):
    name = models.CharField(max_length=100)             # 发布会标题
    limit = models.IntegerField()                       # 参加人数
    status = models.BooleanField()                      # 状态
    address = models.CharField(max_length=200)          # 地址
    start_time = models.DateTimeField('events time')    # 发布会时间
    create_time = models.DateTimeField(auto_now=True)   # 创建时间（自动获取当前时间）

    def __str__(self):
        return self.name

# 嘉宾表
class Guest(models.Model):
    event = models.ForeignKey(Event)                    # 外键，关联发布会id
    realname = models.CharField(max_length=64)          # 姓名
    phone = models.CharField(max_length=16)             # 电话
    email = models.EmailField()                         # 邮箱
    sign = models.BooleanField()                        # 签到状态
    create_time = models.DateTimeField(auto_now=True)   # 创建时间（自动获取当前时间）

class Meta:
    unique_together = ("event", "phone")

def __str__(self):
    return self.name
```

发布会表（Event 类）和嘉宾表（Guest 类）

**注意：**

1. 发布会表和嘉宾表中默认都会生成自增id，而我们在创建模型时不需要声明该字段。
2. 发布会表中增加了status 字段用于表示发布会的状态是否开启，用于控制该发布会是否可用。
3. 嘉宾表中通过event_id 关联发布会表，一条嘉宾信息一定所属于某一场发布会。


4. 对于一场发布会来说，一般会选择手机号作为一位嘉宾的验证信息，所以，对于一场发布会来说，手机号必须是唯一。除了嘉宾id 外，这里通过发布会id +手机号来做为联合主键。
5. python3使用__str__()方法、python2使用__unicode__()方法，显示对应字段。

**Django模型字段常用类型**

| 类型                      | 说明                                                         |
| ------------------------- | ------------------------------------------------------------ |
| AutoField                 | 一个IntegerField类型的自动增量                               |
| BooleanField              | 用于存放布尔类型的数据（True or False）                      |
| CharField                 | 用于存放字符型的数据，需要指定长度max_length                 |
| DateField                 | 用于存放日期类型的数据，格式为：YYYY-MM-DD                   |
| DecimalField              | 用于存放小数型的数据                                         |
| EmailField                | 用于存放电子邮件类型的数据                                   |
| FilePathField             | 用于存放文件路径类型的数据                                   |
| FloatField                | 用于存放浮点型的数据                                         |
| IntegerField              | 用于存放整数类型的数据，范围是：-2147483648至2147483647      |
| BigIntegerField           | 用于存放大整数类型的数据，最大支持：9223372036854775807      |
| GenericIPAdressField      | 用于存放IP地址类型的数据，同时支持IPv4、IPv6，字符串格式     |
| NullBooleanField          | 类似BooleanField，但是允许填写NULL                           |
| PositiveIntegerField      | 用于存放正数或0的整数类型的数据，范围是：0-2147483647        |
| PositiveSmallIntegerField | 类似PositiveIntegerField，但是范围是：0-32767                |
| SlugField                 | Slug是短标签，只包含字母、数字、下划线或字符，它通常在网址中使用，需要定义max_length |
| SmallIntegerField         | 类似IntegerField，但是范围是：-32768至32767                  |
| TextField                 | 用于存放文本类型的数据                                       |
| TimeField                 | 用于存放时间类型的数据，格式为：HH:MM[:ss[.uuuuuu]]          |
| URLField                  | 用于存放URL地址                                              |
| BinaryField               | 用于存放原始二进制类型的数据                                 |

详细内容可参考官方文档： 
<https://docs.djangoproject.com/en/1.10/ref/models/fields/>

**使用如下命令进行数据库表的初始化和创建（迁移）：**

`\guest>python manage.py makemigrations sign`

`\guest>python manage.py migrate`

### 4.2 admin后台管理

**将上面创建的发布会表、嘉宾表，也加入到admin后台进行图形化的管理：**

修改/guest/sign/admin.py文件：

```
from django.contrib import admin
from sign.models import Event, Guest

# Register your models here.
admin.site.register(Event)
admin.site.register(Guest)
```

通过管理页面随意增加一条发布会记录，我这里在增加一条中文记录的时候，出现了编码错误的提示：

`UnicodeEncodeError at /admin/sign/event/add/`

看这意思是中文编码问题，印象当中之前看Django官方文档的时候，有关于中文编码的问题，于是翻了下之前的笔记，发现是修改/guest/settings.py里的MIDDLEWARE配置，增加一条：

`django.middleware.locale.LocaleMiddleware`

但是这里将Django的admin管理后台的展示从英文变成了本地语言中文，对于上面遇到的错误还是没有帮助，哎，不应该呀，虫师都能在书中增加一条中文记录，没理由我这里完全按照虫师讲解的一步一步操作过来的会遇到错误啊，仔细回头查看models.py发现了一个小问题，虫师在书中也提到了只是自己当时忽略了，虫师使用的是python3，而我使用的是python2，在展示列表的函数里，虫师使用的是__str__，而我完全照搬书中的代码，也使用了__str__，这里就出错了，如果是python2，应该使用__unicode__才对，修改代码：

```
# 发布会表
class Event(models.Model):
    name = models.CharField(max_length=100)             # 发布会标题
    limit = models.IntegerField()                       # 参加人数
    status = models.BooleanField()                      # 状态
    address = models.CharField(max_length=200)          # 地址
    start_time = models.DateTimeField('events time')    # 发布会时间
    create_time = models.DateTimeField(auto_now=True)   # 创建时间（自动获取当前时间）

    #def __str__(self):
    def __unicode__(self):
        return self.name
```

再次添加中文记录，成功，问题解决，后续对于不同版本使用的不同函数，还是要注意啊！

**在管理后台列表中显示更多字段：**

修改/guest/sign/admin.py文件：

```
#! /usr/bin python
# -*- coding:utf-8 -*-

from django.contrib import admin
from sign.models import Event, Guest

# Register your models here.

# 在admin管理后台展示更多字段
class EventAdmin(admin.ModelAdmin):
    list_display = ['id', 'name', 'status', 'address', 'start_time']

class GuestAdmin(admin.ModelAdmin):
    list_display = ['realname', 'phone', 'email', 'sign', 'create_time', 'event']


# 在admin管理后台注册发布会表、嘉宾表
admin.site.register(Event, EventAdmin)
admin.site.register(Guest, GuestAdmin)
```

**注意：**

- 新建EventAdmin类继承admin.ModelAdmin类，其中ModelAdmin提供了很多自定义管理工具；
- list_display数组内是显示的字段名，必须存在于数据表中。

**在管理后台增加搜索栏、过滤器：**

还是修改/guest/sign/admin.py文件：

```
#! /usr/bin python
# -*- coding:utf-8 -*-

from django.contrib import admin
from sign.models import Event, Guest

# Register your models here.

class EventAdmin(admin.ModelAdmin):
    list_display = ['id', 'name', 'status', 'address', 'start_time']    # 在admin管理后台展示更多字段
    search_fields = ['name']                                            # 在admin后台增加搜索栏
    list_filter = ['status']                                            # 在admin后台增加过滤器

class GuestAdmin(admin.ModelAdmin):
    list_display = ['realname', 'phone', 'email', 'sign', 'create_time', 'event']
    search_fields = ['realname', 'phone']
    list_filter = ['sign']


# 在admin管理后台注册发布会表、嘉宾表
admin.site.register(Event, EventAdmin)
admin.site.register(Guest, GuestAdmin)
```

### 4.3 基本数据访问

#### 通过如下方法对数据库中的数据进行访问：

```
root@TEST:/home/test/guest# python manage.py shell
Python 2.7.12 (default, Nov 19 2016, 06:48:10) 
[GCC 5.4.0 20160609] on linux2
Type "help", "copyright", "credits" or "license" for more information.
(InteractiveConsole)
>>> from sign.models import Event, Guest    # 导入sign应用下Model中的Event类、Guest类
>>> Event.objects.all()                     # 通过objects.all()获取全部对象
<QuerySet [<Event: 这是一个event>]>
>>> Guest.objects.all()
<QuerySet [<Guest: 这是一个guest>]>
>>> 
```

可以看到当我们查询Event数据表里的对象时，返回内容是我们添加的记录的name字段，而当我们查询Guest表的对象时，返回的却是对象而不是具体的字段名，这是为什么？

因为在/guest/sign/models.py文件中，我们为Event类定义了返回的字段名，但是Guest类没有定义返回的字段名，所以就造成了上述查询结果的差异。

如果我们也想让Guest在查询的时候返回一个字段名比如realname，而不是返回一个对象object，只需要同样增加如下代码：

```
# 嘉宾表
class Guest(models.Model):
    ……

    def __unicode__(self):
        return self.realname
```

再次查询Guest的结果就变为：

```
>>> Guest.objects.all()
<QuerySet [<Guest: 好友>]>
>>> 
```

#### 插入数据：

```
root@TEST:/home/test/guest# python manage.py shell
Python 2.7.12 (default, Nov 19 2016, 06:48:10) 
[GCC 5.4.0 20160609] on linux2
Type "help", "copyright", "credits" or "license" for more information.
(InteractiveConsole)
>>> from sign.models import Event, Guest
>>> from datetime import datetime
>>> e1 = Event(name='周末聚会', limit=200, status=True, address='北京', start_time=datetime(2017,6,10,15,0,0))
>>> e1.save()
/usr/local/lib/python2.7/dist-packages/django/db/models/fields/__init__.py:1430: RuntimeWarning: DateTimeField Event.start_time received a naive datetime (2017-06-10 15:00:00) while time zone support is active.
  RuntimeWarning)
>>> 
```

这里与书中略有区别，书中是在插入数据时，指定了id=2，实际上id是数据表中自增字段，插入数据的时候也不可能先去查下id现在是多少了，所以忽略该字段，直接插入数据也是可以的。

**忽略结尾时间警告信息：** 修改/guest/settings.py文件，设置USE_TZ=False

上面的步骤，先是创建数据，然后调用save()方法保存至数据表中，也可以将两步合为一步：

```
>>> Event.objects.create(id=3, name='红米MAX发布会', limit=200, status=True, address='北京会展中心', start_time=datetime(2016,9,22,14,0,0))
<Event: 红米MAX发布会>
>>> Guest.objects.create(realname='Andy', phone=13611001101, email='andy@mail.com', sign=False, event_id=3)
<Guest: Andy>
>>> 
```

通过指定Event的id=3、Guest的event_id=3，将两者进行关联。

#### 查询数据：

**table.objects.create()：**一步插入数据。

**table.objects.get()：**查询一条匹配的结果，返回结果为对象，返回结果不存在报DoesNotExist错误。

**table.objects.filter()：**查询匹配的结果，返回结果为对象列表，返回结果不存在返回空列表[]。

```
# 使用get()查询name=红米MAX发布会的记录
>>> e1 = Event.objects.get(name='红米MAX发布会')
# 返回的结果e1是个对象
>>> e1
<Event: 红米MAX发布会>
# 从e1对象获取start_time的值
>>> e1.start_time
datetime.datetime(2016, 9, 22, 14, 0, tzinfo=<UTC>)
# 也可以不赋值给e1，而是直接在get()方法后加上字段名来获取对应的值
>>> Event.objects.get(name='红米MAX发布会').status
True
# 同理查询红米MAX发布会的地点
>>> Event.objects.get(name='红米MAX发布会').address
u'\u5317\u4eac\u4f1a\u5c55\u4e2d\u5fc3'
# 如果查询条件结果不存在则会报错DoesNotExist
>>> Event.objects.get(name='发布会').address
Traceback (most recent call last):
  File "<console>", line 1, in <module>
  File "/usr/local/lib/python2.7/dist-packages/django/db/models/manager.py", line 85, in manager_method
    return getattr(self.get_queryset(), name)(*args, **kwargs)
  File "/usr/local/lib/python2.7/dist-packages/django/db/models/query.py", line 385, in get
    self.model._meta.object_name
DoesNotExist: Event matching query does not exist.
# 使用filter来过滤指定条件获取对象列表，注意name和contains之间是双下划线
>>> e2 = Event.objects.filter(name__contains='发布会')
# 返回结果e2是个结果列表
>>> e2
<QuerySet [<Event: 红米MAX发布会>]>
# 既然是列表就可以用如下方式访问其中数据
>>> e2[0]
<Event: 红米MAX发布会>
# 关联查询示例，先使用get()方法获取查询结果对象
>>> g1 = Guest.objects.get(phone='13611001101')
# 关联查询phone为136……嘉宾所对应的发布会信息event
>>> g1.event
<Event: 红米MAX发布会>
# 该嘉宾对应发布会的名称
>>> g1.event.name
u'\u7ea2\u7c73MAX\u53d1\u5e03\u4f1a'
# 该嘉宾对应发布会的地址
>>> g1.event.address
u'\u5317\u4eac\u4f1a\u5c55\u4e2d\u5fc3'
>>> 
```

#### 删除数据：

**table.objects.get().delete()：**获取指定查询条件的结果对象并删除。

获取查询对象，调用delete()方法进行删除：

```
# 先获取查询对象，再调用delete()方法删除
>>> g2 = Guest.objects.get(phone='13611001101')
>>> g2.delete()
(1, {u'sign.Guest': 1})
>>> 
# 不可以重复删除
>>> Guest.objects.get(phone='13611001101').delete()
Traceback (most recent call last):
  File "<console>", line 1, in <module>
  File "/usr/local/lib/python2.7/dist-packages/django/db/models/manager.py", line 85, in manager_method
    return getattr(self.get_queryset(), name)(*args, **kwargs)
  File "/usr/local/lib/python2.7/dist-packages/django/db/models/query.py", line 385, in get
    self.model._meta.object_name
DoesNotExist: Guest matching query does not exist.
>>> 
```

#### 更新数据：

**table.objects.select_for_update().filter().update()：**过滤指定条件的对象列表并调用update()方法更新指定数据。

```
# 先获取查询对象，再更新指定数据，最后保存至数据表
>>> g3 = Guest.objects.get(phone='13611001101')
>>> g3.realname = 'Andy2'
>>> g3.save()
>>> 
# 直接使用select_for_update方法过滤指定条件对象列表再调用update方法更新数据并保存至数据表
>>> Guest.objects.select_for_update().filter(phone='13611001101').update(realname='Andy2')
1
>>> 
```

### 4.4 SQLite管理工具

**SQLiteManager** 是一个支持多国语言基于Web 的SQLite 数据库管理工具，它的特点包括多数据库管理，创建和连接；表格，数据，索引操作；视图，触发器，和自定义函数管理，数据导入/导出，数据库结构导出等。

在Firefox 浏览器插件库中可以搜索到SQLiteManager，所以，这装起来非常方便。打开Firefox 浏览器，菜单栏“工具”-->“添加组件”，搜索“SQLiteManager”安装，并重启动Firefox 浏览。



**SQLiteStudio** 是一款SQLite 数据库可视化工具，是使用SQLite 数据库开发应用的必备软件，软件无需安装，下载后解压即可使用，很小巧但很好用，绿色中文版本。比起其它SQLite 管理工具，我喜欢用这个。很方便易用，不用安装的单个可执行文件，支持中文。

### 4.5 配置MySQL

#### 安装MySQL

https://www.jianshu.com/p/f6c1ed073ecd

网上也有很多教程啦~

#### 安装MySQL-Python驱动

**如果使用Python2版本：**安装MySQL-Python

**如果使用Python3版本：**安装PyMySQL

#### MySQL基本操作

- show databases; # 查看当前数据库下面的所有库
- use test; # 切换到test库
- show tables; #查看test库下面所有表
- show global variables like ‘port’; #查看MySQL端口号
- CREATE DATABASE guest CHARACTER SET utf8; #创建guest数据库

书中使用如下代码演示了如何通过pymysql库对数据库进行操作，插入数据，但是书中忽略了一个重要前提，我们从开始新建Django项目的时候，就使用的是SQLite3数据库，并没有用MySQL进行过任何操作，这如果直接按代码执行，只会提示错误没有guest表，对啊，是没有啊，上面只是在基本操作里，新建了guest库，但是没有在guest库下新建表，所以在执行插入数据的代码之前，我们应该先新建两个表：sign_event、sign_guest

```
# 创建sign_event表
CREATE TABLE `sign_event` (
    `id` int(11) NOT NULL AUTO_INCREMENT,
    `name` varchar(100) NOT NULL,
    `limit` integer,
    `status` BOOLEAN,
    `address` varchar(200) NOT NULL,
    `start_time` DATETIME, 
    `create_time` DATETIME DEFAULT CURRENT_TIMESTAMP,
    PRIMARY KEY (`id`)
)ENGINE=InnoDB DEFAULT CHARSET=utf8;

alter table `sign_event` add index id(id);
```

```
# 创建sign_guest表
CREATE TABLE `sign_guest` (
    `id` int(11) NOT NULL AUTO_INCREMENT,
    `event_id` int(11) NOT NULL,
    `realname` varchar(64) NOT NULL,
    `phone` varchar(16) NOT NULL,
    `email` varchar(254) NOT NULL,
    `sign` BOOLEAN,
    `create_time` DATETIME DEFAULT CURRENT_TIMESTAMP,
    PRIMARY KEY (`id`),
    FOREIGN KEY (`event_id`) REFERENCES sign_event(id)
)ENGINE=InnoDB DEFAULT CHARSET=utf8;

alter table `sign_guest` add index_id(id);
alter table `sign_guest` add unique(event_id, phone);
```
MySQL语法规则：
--在表中添加索引值
ALTER TABLE 表名字 ADD INDEX 索引名 (列名);
--在sign_guest表的id列上建立名为indx_id的索引
`alter table `sign_guest` add index id(id);`

```
#! /usr/bin python
# -*- coding: utf-8 -*-

from pymysql import cursors, connect

# 连接数据库

conn = connect(host='127.0.0.1',
            user='root',
            password='nsfocus',
            db='guest',
            charset='utf8mb4',
            cursorclass=cursors.DictCursor)

try:
    with conn.cursor() as cursor:
        # 创建发布会数据
        sql = 'INSERT INTO sign_event (name, `limit`, status, address, start_time, create_time) VALUES ("红米MAX发布会", 200, 1, "北京会展中心", "2016-09-22 14:00:00", NOW());'
        cursor.execute(sql)
        # 提交事务
        conn.commit()
        # 创建嘉宾数据
        sql = 'INSERT INTO sign_guest (realname, phone, email, `sign`, event_id, create_time) VALUES ("Tom", 18800110002, "tom@mail.com", 0, 1, NOW());'
        cursor.execute(sql)
        # 提交事务
        conn.commit()
    with conn.cursor() as cursor:
        # 查询刚刚添加的嘉宾
        sql = 'SELECT realname, phone, email, sign FROM sign_guest WHERE phone=%s'
        cursor.execute(sql, ('18800110002',))
        result = cursor.fetchone()
        print result
finally:
        conn.close()
```

**在这个过程中遇到一些问题：**

我们安装了MySQL数据库后，虽然创建了guest数据库，但是并没有创建发布会表、嘉宾表，所以如果按书中直接执行此py文件，一定会报错，提示guest表不存在！！！

如何解决这个问题，很简单，参考之前models.py文件里的设置，先创建好两个数据表event、guest，再来执行这个py文件就不会出错了！

**引申出两个问题：**

1. 如何使MySQL服务端不只是监听本地127.0.0.1这个地址？
   - 这就需要修改配置文件/etc/mysql/mysql.conf.d/mysqld.cnf，将其中的bind-address由127.0.0.1修改为0.0.0.0，重启MySQL服务即可/etc/init.d/mysql restart；
2. 如果通过远程来访问MySQL服务端？
   - 对MySQL实在不熟，发现用起来很多基础知识都不懂，查了好半天的资料才找到问题根本，原本以为在配置文件里修改各什么安全设置就能搞定，然而并不是这么回事，这是由于root用户在mysql数据库中存储的host字段的限制；
   - 首先在本地登录MySQL数据库：mysql -u root -p，然后切换至mysql数据库：use mysql;，查看user表中root帐号的记录：select * from user where user=’root’;，可以发现host字段的值为localhost，这就是为什么远程主机无法使用root帐号来访问的原因了；
   - 解决办法就是给host字段增加远程主机的IP：grant all privileges on *.* to root@”远程主机IP地址” identified by “root帐号对应的密码”;，这就相当于给远程IP赋予了所有的权限，包括远程访问的权限，然后输入：flush privileges;，这相当于是重新加载一下mysql权限；
   - 至此，就又可以从本地访问也能从远程主机访问了。

**pymysql里的几个重要函数：**

- **connect()：**建立数据库连接；
- **cursor()：**获取数据库操作游标；
- **execute()：**执行SQL语句；
- **commit()：**提交数据库执行；
- **close()：**关闭数据库连接；

#### 在Django中配置MyDQL

修改配置文件/guest/settings.py：

```
DATABASES = {
    'default': {
        'ENGINE': 'django.db.backends.mysql',
        'HOST': '127.0.0.1',
        'PORT': '3306',
        'NAME': 'guest',
        'USER': 'root',
        'PASSWORD': '123456',
        'OPTIONS': {
            'init_command': "SET sql_mode='STRICT_TRANS_TABLES'",
        },
    }
}
```

**注意：**需要先把原数据库注释掉。

虫师的是在本章节，才讲解了如何将SQLite3里的数据同步至MySQL里，因为并不能这样复制数据，但是可以使用：

`python manage.py migrate`

命令来重新执行数据库同步，是数据模型重新在MySQL数据库中生成表。这也就是上面我提到的虫师缺少的部分，只不过我上面是用笨方法手工重建MySQL的数据表，而这里是使用数据同步命令。

但是在执行此命令的时候也会遇到问题，即：“如何让Django通过PyMySQL来连接数据库”？

需要在/guest/__init__.py中加入如下代码：

```
import pymysql

pymysql.install_as_MySQLdb()
```

再次执行migrate命令就不会出错了，由于更换了MySQL数据库，所以admin后台的超级管理员的账号也要重建。

#### MySQL管理工具

- Navicat
- SQLyog

至此第4章的全部内容都结束了。本章的内容主要围绕Django的Model层，也就是数据层做简要介绍。主要知识点就是model.py以及数据在管理后台admin.py里如何展示。



### 参考

https://blog.csdn.net/zhaoxz1985/article/details/72877989