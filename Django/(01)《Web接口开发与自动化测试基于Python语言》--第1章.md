# (01)《Web接口开发与自动化测试基于Python语言》--第1章

下面的文章是来自虫师的书籍《Web接口开发与自动化测试基于Python语言》做的读书笔记。

![image.png](https://upload-images.jianshu.io/upload_images/1683050-492d388cf7726634.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

自己啃这本书，也啃了两三遍，但感觉其中不理解的地方还有很多。

于是又重头从第一章看起，梳理一下本书的逻辑和知识。温故而知新。

Python的基础要打牢固啊~

跟着虫师的书，敲的代码附上[Github地址](https://github.com/edrainann/learn_testing/tree/master/python/Django/guest)，如有需要可以进行参考~

好啦，下面就正式开始重头学习啦

------



## 前言

**为什么学习本书：**

- 是否想学习 web 开发而找不到很好的入门教程。

- 是否想做一个漂亮的网站出来炫耀。
- 是否很想知道当你点击一个按钮后，程序到底经过哪些过程把你想要的页面展现在你面前。
- 是否想告诉别人，不就是开发嘛，我也会。
- 是否想知道接口到底是什么，如何对它进行测试。

**为什么是 Django？**

- 我们总是很难去决定一件事情。比如，我到底该学 Java 呢还是 C# 呢？ 

- 到底测试的工资高还是开发的高？
- 我学习了 Python 之后，想学习 web 开发，是 Django 还是 Flask 好学、强大和灵活？
  搞 Java 开发的老程序员会告诉你，他只需要几周时间就可以学会用 C#开发程序。反之也是一样。
- 一直用 Flask 写 web 的应用同学有一天突然想换成 Django 来开发 web 应用，结果看了一下 Django 的文档就开始写代码了。

好吧！选择总会有理由，我的理由很简单，只是因为 Django 资料更丰富，当遇到了问题更容易找到答案。

**为什么是接口测试？**
接口常被开发人员挂在嘴边，在开发过程中无处不在，但对于测试人员来说，它又如此朦胧，无形无色无味，难以触碰。

但它到底是什么？如何对它进行测试？一直是困扰许多测试新手的问题。之所以看不清接口是什么，主要是因为我们不了解应用是如何被开发出来的。

所以，对于想学接口测试的同学，我都建议他们学习一下 web 开发，当然，我们目的不是想抢程序员的饭碗，如果，你愿意，在学完本书后也未尝不可。



## 第一章 Python 学习必知

### 1.1 Python2.x 与 与 Python3.x  选择

从近两年来看，官方的态度有所改变，Python2.x 的开发进入消极状态，版本更新速度明显要比 Python3.x 慢得多，而且不再加入新的特性，以维护为主。

Python 3.x是未来大势所趋，目前主流的库基本都已经支持了 Python3.x，不支持的库也在积极的向 Python3.x 迁移。
Python 2.x是目前主流开发版本。原因是因为第三方库对Python 2.x的支持更好。 



### 1.2 Python的安装

####1.2.1Window  下安装 Python
在 Windows 系统中，安装好的 Python 提供了四个选项。
- IDLE(Python 3.5 64-bit) ：该选项为 Python 自带的 IDE，推荐新手使用。
- Python 3.5 (64-bit)：该选项会直接在 window 名称提示符下进入 Python Shell 模式。
- Python 3.5 Manuals(64-bit) ：该选项为 Python 自带的官方文档。
- Python 3.5 Module Docs(64-bit)：该选项为 Python 的模块文档。它自动启动一个服务，并打Web 形式的文档。
####1.2.2安装 Python2.x 和 和 Python3.x 
如果系统中同时安装了Python 2和Python 3的话，分别使用如下命令进入对应版本：

`python` – 进入Python 2.7.x

`python3` – 进入Python 3.6.x
![image.png](https://upload-images.jianshu.io/upload_images/1683050-b3913d45ff6f62da.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

需要说明的是，Python 可执行文件的名称是可以随意修改的。如果你愿意可以将其改成任意名称

#### 1.2.3  ‘python’ 不是内部或外部命令

这个时候你就要找一找你的 Python 到底安装到哪个目录下了，并且把这个目录添加到系统环境变量 Path下面。

### 1.3 扩展库的安装

**注意：**

pip是一个安装和管理Python包的工具，通过pip来管理Python包非常简单，省去搜索-查找版本-下载-安装等繁琐步骤。

同样，pip也和python命令一样，通过pip和pip3来区分对应版本。

#### 1.3.1 pip 安装扩展库

当安装 Python 完成，在 Windows 命令提示符下输入 pip 命令：

- 安装Django库：

`pip install django`

- 安装指定版本的Django库：

`pip install django==1.10.3`

- 使用pip查看当前安装库版本：

`pip show django`

- 使用pip卸载Django库：

`pip uninsatll django`

#### 1.3.2 tar.gz文件安装

并不是所有的扩展库都支持 pip 命令安装。对于个别库来可能只提供了压缩包文件，或者我们安装的环境并不能上网。这个时候就不能 pip 命令安装了。

![image.png](https://upload-images.jianshu.io/upload_images/1683050-310ac01d118c0cec.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

点击 Django-1.10.2.tar.gz 文件进行下载，然后进行解压，进入解压目录，通过“python”
命令安装。

- 使用源码安装Django库：

`python setup.py install`

#### 1.3.3 whl文件安装

wheel 本质上是一个 zip 包格式，它使用 .whl 扩展名，用于 Python 模块的安装。

Django 提供了.whl 文件的安装包。同样先下载 Django-1.10.2-py2.py3-none-any.whl 文件。`.whl`文件的安装，同样需要使用 pip 命令。

- 使用whl文件安装Django库：

`pip install Django-1.10.3-py2.py3-none-any.whl`

**注意：**

wheel本质上是一个zip包格式，它使用.whl扩展名，用于Python模块的安装，pip提供了一个wheel子命令来安装wheel包。

- 使用git安装库：

`git clone https://github.com/defnngj/pyse` 

### 1.4 开发工具选择

#### 1.4.1 Python IDLE

打开 Python 自带的 IDLE，就可以编写 Python 程序了

#### 1.4.2 Sublime Text3

Sublime Text 是一款通用型轻量级编辑器，支持多种编程语言。有许多功能强大的快捷键（如 Ctrl+d），支持丰富的插件扩展。

#### 1.4.3 PyCharm

PyCharm 是 Python 重量级 IDE，功能强大，自动检测语法，可以帮助我们写出更规范的代码。对于处女座的开发者来说是个不错的选择。笔者使用半天过后果断拥抱之。
前面介绍的两款 IDE 适合编写一些简单的 Python 程序，而如果想开发 Python 项目，那么 PyCharm 会是很好的选择。

#### 1.4.4 Atom

Atom 由目前全球范围内影响力最大的代码仓库/开源社区 GitHub 开发。它开源免费跨平台，并且整合GIT 并提供类似 SublimeText 的包管理功能，支持插件扩展，可配置性非常高。

Atom：<https://atom.io/>，开源、免费、跨平台，并且整合Git，提供类似SublineText的包管理功能，支持插件扩展，可配置项非常高。 



> 我自己主要用的是Vscode开源的 + Pycharm破解版， 根据不同的需要交替着来使用。



### 1.5 程序报错不要慌

#### 1.5.1 缩进错误

Python 对程序中，我们不需要“｛｝”来表示一个语句体，也不需要“;”表示一个语句的结束。这就要求我们对程序的缩进有着严格的要求。但有时候，看上去我们的程序格式没有问题，但程序依旧报错。

- 注意区分Tab和四个空格。

#### 1.5.2 引包错误

- 目录下的同名文件、模块引用错误等。

#### 1.5.3 编码错误

- Python文件编码一般为UTF-8或GBK、IDE编辑器自身编码；
- Python读取文件的编码，decode的作用是将其他编码的字符串转换成Unicode编码，encode的作用是将Unicode编码转换成其他编码的字符串。

在开发 Python 程序的过程中，会涉及到三个方面的编码：

- Python 程序文件的编码。

我们在编写的程序本身也存在编码问题，一般的解决方式是在程序的开头加上`#coding=utf-8`或`#coding=gbk`来使程序统一为 UTF-8 或 GBK 编码。

- Python 程序运行时环境（IDE）的编码。

不是管是 Sublime Text 或是 PyCharm 也它，使用的 IDE 工具也存在编码问题。如果你不确定是否是 IDE的编码引起程序出错的，根据我的经验，建议你切换回 Python IDLE 去执行程序。

- Python 程序读取外部文件、网页的编码。

当然，最容易出现编码问题应该是在读取外部数据或文件的时候。首先要确定读取的数据或文件的编码，然后通过 **decode()**和 **encode()**方法来进行编码转换。

​   **decode** 的作用是将其他编码的字符串转换成 Unicode 编码。

​   **encode** 的作用是将 Unicode 编码转换成其他编码的字符串。

当我们在遇到 Python 的编码问题时，从以上三个方法分析就会很容易找到解决编码问题的办法。

#### 1.5.4 学会分析错误



在面对程序抛出的一大堆报错时不知如何分析，如果**认真阅读报错信息**，你将很容易找到错误原因。

其实，比起一大堆的报错，最难解决的问题是没有任何报错信息，而程序却无法正确的执行。

------

### 参考

http://blog.csdn.net/zhaoxz1985/article/details/72780085