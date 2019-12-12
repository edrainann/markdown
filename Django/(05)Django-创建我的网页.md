# Django-创建我的网页

源码地址：<https://github.com/edrainann/Django_website> 

## 一、准备环境

### 1、安装python

### 2、安装Django

```pip install django```

### 3、查看Django是否安装成功

```python
>>> import django
>>> print(django.get_version())
2.2
```

或者在cmd下检查

```bash
edrainsite>django-admin
Type 'django-admin help <subcommand>' for help on a specific subcommand.
Available subcommands:
[django]
    check
    compilemessages
    createcachetable
    dbshell
    diffsettings
    dumpdata
    flush
    inspectdb
    loaddata
    makemessages
    makemigrations
    migrate
    runserver
    sendtestemail
    shell
    showmigrations
    sqlflush
    sqlmigrate
    sqlsequencereset
    squashmigrations
    startapp
    startproject
    test
    testserver
Note that only Django core commands are listed as settings are not properly configured (error: Requested setting INSTALLED_APPS, but settings are not configured. You must either define the environment variable DJANGO_SETTINGS_MODULE or call sett
ings.configure() before accessing settings.).
```
参考网址：https://docs.djangoproject.com/en/2.2/intro/install/

## 二、开始打造自己的网页
参考网址：https://docs.djangoproject.com/en/2.2/intro/tutorial01/
### 1、创建项目

使用 **startproject** 来创建项目

```
> django-admin startproject edrainsite
```
### 2、运行项目
```
> python manage.py runserver
```
这样就可以运行起来啦
打开网址：http://127.0.0.1:8000/ 进行校验
### 3、创建应用
```
> py manage.py startapp hola
```
### 4、创建我的第一个视图
#### 1）hola/views.py 下
```python
from django.http import HttpResponse

def index(request):
    return HttpResponse("Hola! This is index~")

def edrain(request):
    return HttpResponse("Hola! I'm Edwina")
```

#### 2）hola/urls.py下(可能需要创建)
```python
from django.urls import path

from . import views

urlpatterns = [
    path('', views.index, name='index'),
    path('edrain/', views.edrain, name='edrain'),
]
```

#### 3）edrainsite/urls.py下

```python
from django.contrib import admin
from django.urls import path, include

urlpatterns = [
    path('hola/', include('hola.urls')),
    path('admin/', admin.site.urls),
]

```
此时就可以运行```> python manage.py runserver```
然后打开网址：http://127.0.0.1:8000/hola/  或者 <http://127.0.0.1:8000/hola/edrain/> 访问啦