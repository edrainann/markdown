# (07)Robot Framework关于Edit

#### 工作区Edit的组成

首先看测试套件的。选择测试套件后，看右侧工作区的Edit页

![image.png](http://upload-images.jianshu.io/upload_images/1683050-0c61332838c7fd23.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

第一行的Source列出了这个TestSuite的路径

接着有个Settings，现在是隐藏了内容的，点击一下会显示出具体的Setting，下一讲专门介绍Settings。

再往下可以大体分成3个部分。

（1）：加载外部文件

Add Library    ：加载测试库，主要是[PYTHON目录]\Lib\site-packages里的测试库

Add Resource：加载资源，主要是你工程相关的资源文件

Add Variables：加载变量文件，这个没怎么用过。

（2）：定义内部变量

Add Scalar：定义变量

Add List：定义列表型变量

（3）：元数据定义

Add Metadata：定义元数据。我是直接翻译的，这个是新增加的部分，大概看了一下作用是在report和log里显示定义好的内容，格式和document一样。



2、对于project或者说目录的TestSuite来说，他的工作区Edit页和文件型的TestSuite基本是一样的，区别只在于Settings里。

Resources的工作区Edit其实和TestSuite差别也不大，就是没有了metadata的部分，另外2个部分都有的。

 

3、对于TestCase和User Keyword来说，右边的工作区Edit页也是基本一样，区别也只是在Settings里。

TestCase的

 ![image.png](http://upload-images.jianshu.io/upload_images/1683050-a2577c7b94ac8f40.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

User Keyword的

![image.png](http://upload-images.jianshu.io/upload_images/1683050-07c97b585337a4cf.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

可以从图中看到他这边是一个类似于excel的表格一样的区域，这里就是我们的主要写脚本的地方了。

 

4、在写脚本之前，我们先要加载一下测试库，测试库加到哪里其实都是可以的，因为selenium2lib的初始化设置里是GLOBAL，这样运行的时候会作用在全局。

```
ROBOT_LIBRARY_SCOPE = 'GLOBAL'  
```

所以，对于我们这个project来说，加到测试套件和res1.txt上都可以，如果只加了一个，那么在没加测试库的那个下面写的脚本就是黑色的，提示找不到关键字，实际上运行的时候还是会起作用的。

我们先在测试套件来加一下，点击测试套件，在他的工作区点击Add Library

![image.png](http://upload-images.jianshu.io/upload_images/1683050-c3bd7deefc947d4e.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

这样还不算加载成功，成功的标志是按F5键，调出Search Keywords的界面

![image.png](http://upload-images.jianshu.io/upload_images/1683050-9091d9343c8fe3b7.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

点击Source后面的下拉列表，在列表里看到Selenium2Library才算是加载成功。如果按照上面的方法进行没有加载成功的话请确认一下你的版本号，至少在我写本文的时候建议先从0.5.2开始装起。

下面就可以写脚本了，先写个open browser运行一下看看。

![image.png](http://upload-images.jianshu.io/upload_images/1683050-a2577c7b94ac8f40.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

 

看到open browser的颜色与众不同了么？RIDE早一些的版本是蓝色，现在他对各种不同的关键字做了区分，可以自定义颜色。现在这个是默认的测试库的关键字颜色。

------

####  工作区Edit的Settings

1、Project或者说目录型的TestSuite

![image.png](http://upload-images.jianshu.io/upload_images/1683050-43aec06ccefd6317.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

=================================

编辑设置：可以点击文本框或者Edit

清除设置：点击Clear

=================================

 

Documentation：文档，每一项都有。可以给当前的对象加入文档说明。



这里也可以设置简单的格式，两个*中间的文字会被加粗，两个_中间的文字会设为斜体，URL会被转换成可点击的链接，这个跟MD的书写规则很类似。

 

接下来的Setup和TearDown分别表示启动和停止，也就是你可以在对应的文本框设置一个关键字，那么指定的事件触发的时候就会执行这个关键字。

Suite Setup：测试套件启动的时候就执行某个关键字。

Test Teardown：测试案例结束的时候执行某个关键字。

![image.png](http://upload-images.jianshu.io/upload_images/1683050-4499cdd3dd1a025d.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

为了演示，我随便设置一个关键字吧，如上面所示，我在Suite Setup设置了Sleep | 5sec，表示等待5秒，要注意关键字的参数要使用 | 分隔，如果要使用 | 本身而不是让他起分隔参数的作用，那么就使用 \| 即可。

其他几个也是一样的，这里就不一一举例了。

 

Force Tags：最后说说这个强制tag标记，这是个不错的设置，应该也是最近新加的，可以方便的帮我们批量设置tags，tags有什么用呢？在后面运行的时候我们可以选择指定tag的案例来运行。那么这里设置了Force Tags之后呢，他就会强制的给他的所有子元素加上这些tags。先设置上一个tag，名字我随便写的Prject force tag，然后我们看效果。

![image.png](http://upload-images.jianshu.io/upload_images/1683050-56993608dd70f64e.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

 

2、文件型Suite的Settings

首先映入眼帘的应该就是那个红色Force Tags了吧。这就是刚刚我们在上一层的Test中设置了Prj Force这个标记，他就会强制的传递给所有的子元素，包括下面的TestCase。

 这个Settings页面和目录型的基本差不多，重复的就不说了，说一下多出来的几个。

Test Template：测试模版，这是可以指定某个关键字为这个测试套件下所有TestCase的模版，这样所有的TestCase就只需要设置这个关键字的传入参数即可。

在Selenium2Library的Demo里有这个的使用，想了解的童鞋可以自己看一下。

Test Timeout：设置每一个测试案例的超时时间，只要超过这个时间就会失败，并停止案例运行。这是防止某些情况导致案例一直卡住不动，也不停止也不失败。

Force Tags：这里还是要说一下，在文件型Suite这里还可以继续给子元素增加Force Tags，但是他不能删除父元素设置的tags

Default Tags：默认标记，其实和Force Tags没啥区别的，效果都是一样的，只是颜色不同而已。

 我们在这里分别再增加一个Tag：

![image.png](http://upload-images.jianshu.io/upload_images/1683050-face357a986b73f2.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

 

 3、TestCase的Settings

![image.png](http://upload-images.jianshu.io/upload_images/1683050-c7b89a6378beba9a.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

可以看到这里把前面增加的Tags都加上了，很方便懒人啊，以前一个一个的设置好麻烦，现在至少可以批量设置了，然后如果不够可以再针对单个案例设置独特的Tag。

Force Tags和Default Tags的区别在我看来只是颜色上有些区分，其他的区别暂时没有发现。

这里的Setup、Teardown、Timeout、Template其实前面加上一个Test就是和文件型Suite里的是一样的了。因为这已经是在Test Case了，所以省略了。

 

4、Resource的Settings

不多说，这个只有一个documentation。

 

5、User Keywords的Settings

![image.png](http://upload-images.jianshu.io/upload_images/1683050-348b283d07ad01f5.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

和TestCase比较起来，他没有了Tags，也没有Setup和Template，而有2个新的设置就是Arguments : 传入参数

Return Value : 返回值

这样看起来User Keywords其实就是一个函数了，再进一步说，Bulletin的Keywords和TestLib里的Keywords也都是一个个的函数，只是封装在不同层面。后2个是在代码级的封装，将python代码写成的函数封装成可以调用的关键字，而User Keywords就是把这些可调用的关键字进一步的封装，可以理解为应用层面的封装，而且可以层层封装。

到后面你会发现，大部分时间，你其实是和User Keywords在打交道，利用好User Keywords，会方便很多。

#### 参考

http://blog.csdn.net/tulituqi/article/details/7592711

http://blog.csdn.net/tulituqi/article/details/7604931