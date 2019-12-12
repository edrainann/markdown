# (04)Robot Framework第一个用例



#### 创建测试项目Project

 选择菜单栏file----->new Project

![image.png](http://upload-images.jianshu.io/upload_images/1683050-c2fb7818ffbec8f0.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

Name 输入项目名称，会在该路径下生成一个对应的文件夹，Type 选择Directory。

![image.png](http://upload-images.jianshu.io/upload_images/1683050-92c29795ea9543b7.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

左侧 ：工程名、父目录、创建路径。

从Tpye上来说，分为文件和目录两种，区别嘛，看你的工程定，如果内容很简单，File类型的就可以，如果内容较多，从方便管理的角度来说，选Directory的比较合适。

从Format上来说，分为ROBOT、txt、tsv、html，比较推荐robot和txt，在安装vs code的插件后查看`.robot`文档会自动语法高亮，也便于维护和版本管理，后续的案例。

 

#### 创建测试套件Suite

  右键点击“test01”选择new Suite 选项，建立一个测试套件

![image.png](http://upload-images.jianshu.io/upload_images/1683050-728335dc78007578.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

![image.png](http://upload-images.jianshu.io/upload_images/1683050-c8ae32deb35b569e.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

Name 输入项目名称。

Type 选择File。

------

看到这个图，比较一下和前面New Project的图有区别么？

其实，从根本上说，Project和Test Suite是一样的，如果硬要区分一下的话，我的意见是目录型的是Project，文件型的是Test Suite。

借用一下吴博PPT里的关系图来说明一下：

![image.png](http://upload-images.jianshu.io/upload_images/1683050-7da9544aec1513cf.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


图中，Project和Dir Test Suite是相同的，当然这里的Project也是Dir的，如果是File Project，那么他其实是等同于File Test Suite。

作为一个目录型的Test Suite来说，他们下面可以包含文件Test Suite或者目录Test Suite，层级可以很深。但我们最终要用的TestCase只能在文件型的TestSuite下面。

大家可以自己创建一些复杂的目录结构来体验一下Project、TestSuite和TestCase的关系，同时为了印证我说的“Project和Dir Test Suite是相同的”，你可以找一个你创建的目录型的TestSuite，使用RIDE的File下面的Open Directory打开你的这个目录。你会发现他和Project是一样的
同时，你在目录型的TestSuite上点右键，是看不到新增TestCase的选’项

------

#### 创建测试用例Case

  右键点击“test suite01”选择new Test Case 

![image.png](http://upload-images.jianshu.io/upload_images/1683050-deaa3ec75d5bd43e.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)



![image.png](http://upload-images.jianshu.io/upload_images/1683050-f120a9efa72442f2.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

用例只需要输入用例name ，点击OK即可。

 ![image.png](http://upload-images.jianshu.io/upload_images/1683050-b59ce0b2e37ab320.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

 看到测试套件前面多了个*么？这表示他是有了新的修改，还没有保存。我们先保存一下。

#### 导入selenium2library库

因为RF框架编写基于web 的测试用例，所以，我们需要selenium 的库支持。所以，我们在使用的过程中需要加载selenium2library库。

![image.png](http://upload-images.jianshu.io/upload_images/1683050-1fb38679b7f48fcc.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

![image.png](http://upload-images.jianshu.io/upload_images/1683050-24344ef39c87655a.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

点击`Settings`可以切换页面，方便后续导入selenium2library

可以直接在Name中输入`Selenium2Library`

![image.png](http://upload-images.jianshu.io/upload_images/1683050-db94155e30c46282.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

或者直接通过Browse导入，一般路径是在`F:\Python27\Lib\site-packages`

![image.png](http://upload-images.jianshu.io/upload_images/1683050-d382147322edd6b2.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)



在“测试套件”的Edit标签页，点击“Library”按钮，弹出输入框，Name输入：Selenium2Library ，点击OK 完。

如果导入的库显示为红色，表示导入的库不存在。如果是黑色则表示导入成功。

 ![image.png](http://upload-images.jianshu.io/upload_images/1683050-e126bc3ef73dfa17.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

 

#### 编写用例

 　　下面就可以开始写我们的用例了，可是怎么写呢？我们可以通过按**F5** 快捷键来查询脚本的关键字。如果你接触过QTP 或 selenium IDE 等自动化工具的话，应该会有一些思路。

![img](https://images0.cnblogs.com/i/311516/201407/271803093852035.png)

 　　如上图，自动化脚本从打开浏览器开发，如上图，我想打开一个浏览器，想的是“open”为关键字进行搜索，结果找到了一个“Open Browser”的关键字，点击这个关键字，想显示它的用法和说明。

记得可以通过`Ctrl + Alt + 空格` 来联想看有哪些关键字喔~

![image.png](http://upload-images.jianshu.io/upload_images/1683050-51e3ed0c9b21bc34.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

根据说明，我们来尝试创建这个打开浏览器的操作吧：

![image.png](http://upload-images.jianshu.io/upload_images/1683050-aa11114eaa85ad6d.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

　　“Open Browser”变蓝了，说明它是一个合法的关键字，后面有一个方框是红色的，表示这个参数不能缺省的。通过说明信息中，我发现它需要一个url 地址是必填的，当然还需要指定browser （默认不填为 friefox）

　　更多关键的使用，请参考相关API 文档。这里不过多介绍。按照上面的方法。创建百度搜索用例如下：

![image.png](http://upload-images.jianshu.io/upload_images/1683050-b66b414a9235ecdc.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

 



#### 运行测试用例 

　　勾选当前需要运行的测试用例，点击工具栏运行按钮，如果只运行单个用例的话，也可以切换到用例的Run标签页，点击“start”按钮。

![image.png](http://upload-images.jianshu.io/upload_images/1683050-53ed4694df49b5e0.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

 

运行信息：

![image.png](http://upload-images.jianshu.io/upload_images/1683050-d8bfec3bb35898ca.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

　　运行信息显示会生成三个文件：Output.xml、Log.html、Report.html

　　我们重点查看Log.html和Report.html ，Log.html更关注脚本的执行过程的记录，Report.html更关注脚本的执行结果的展示。

​	赶快打开你的测试报告看看效果吧！

![image.png](http://upload-images.jianshu.io/upload_images/1683050-0dbbc0a33d55277d.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

------

#### Q&A

但是奇怪的是ride没有显示Message log，而运行的时候明明已经把✔选中了

查看log发现有这样的报错

![image.png](http://upload-images.jianshu.io/upload_images/1683050-da6ed40fdfb2537c.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

```
DevTools listening on ws://127.0.0.1:12676/devtools/browser/7a3c54fe-81d1-495f-8c3d-ed3eef4bd71c
Traceback (most recent call last):
  File "F:\Python27\lib\site-packages\robotide\contrib\testrunner\testrunnerplugin.py", line 370, in OnTimer
    self._test_runner.get_output_and_errors(self.get_current_profile())
  File "F:\Python27\lib\site-packages\robotide\contrib\testrunner\testrunner.py", line 250, in get_output_and_errors
    stdout, stderr, returncode = self._process.get_output(), \
  File "F:\Python27\lib\site-packages\robotide\contrib\testrunner\testrunner.py", line 305, in get_output
    return self._output_stream.pop()
  File "F:\Python27\lib\site-packages\robotide\contrib\testrunner\testrunner.py", line 400, in pop
    return result.decode('UTF-8')
  File "F:\Python27\lib\encodings\utf_8.py", line 16, in decode
    return codecs.utf_8_decode(input, errors, True)
UnicodeDecodeError: 'utf8' codec can't decode byte 0xb0 in position 124: invalid start byte
```

按照下面链接尝试，大意是修改里面的代码。

但修改保存之后Run的功能都没有显示了，而且启动ride会报错。

https://jingyan.baidu.com/article/380abd0a08c9511d90192c15.html

http://blog.csdn.net/titiyufeng/article/details/53187791

原代码如下：

![image.png](http://upload-images.jianshu.io/upload_images/1683050-329f8be9fdb40db7.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)



于是我又把文件还原（tips：记得养成好习惯，把原文件备份好），在sublime中Ctrl+B执行一下testrunner.py，重新启动ride就可以显示Message log，没想明白为啥，神奇。。

![image.png](http://upload-images.jianshu.io/upload_images/1683050-206579a52532ca18.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

------

Update：

终于找到了RIDE下方不显示Message log的原因~

参考https://www.jianshu.com/p/a6f31416982d的Q&A部分 

============================================================================

错误：

command: pybot.bat --argumentfile c:\users\keikei\appdata\local\temp\RIDEama2ym.d\argfile.txt --listener D:\Python27\lib\site-packages\robotide\contrib\testrunner\TestRunnerAgent.py:52418 E:robot\测试项目

解决：

将“C:\Python27\Scripts ”添加到PATH环境变量中。命令提示符号查看，RF版本。提示pybot 不是内部命令，说明环境变量设置有问题。

![img](https://images0.cnblogs.com/i/311516/201407/271806155724955.png)





#### 参考

http://blog.csdn.net/tulituqi/article/details/7585387

http://www.cnblogs.com/fnng/p/3871712.html