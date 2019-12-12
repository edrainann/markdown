# (05)Robot Framework分层思想1



谈到Robot  Framework 分层的思想，就不得不提“关键字驱动”。

####  关键字驱动

关键字驱动： 通过调用的关键字不同，从而引起测试结果的不同。

在selenium API 中所介绍的方法其实就是关键字，如“open browser” 就是一个关键字。从底层去看它就是一个通过编程去现实的一个方法。

```
def  open_browser(url，browser):
     #通过browser找到相应的浏览器驱动，调用浏览器，借助python的httplib、urllib模块将url传递给浏览器。从而实现open brwoser 的目的。
```

通过上面的伪代码表述的“关键字”的底层其实还是程序定义的方法。

 

　　回到分层的思想上，在程序设计的讲究设计模式，设计模式其实就是根据需求使用抽象与封装，其实就是分层思想。把一个实现过程分成不同多层。提高的灵活性，从而达到可扩展性和可维护性。

 

　　再回到自动化的话题上，我们可以把操作步骤封装一个一个的方法（关键字），通过调用关键字来实现测试用例。

 

<http://www.cnblogs.com/fnng/p/3871712.html>

参考本系列的第一节创建一条百度搜索的测试用例。

写好一条之后，点击右键 ->copy 出5条百度搜索的用例

![image.png](http://upload-images.jianshu.io/upload_images/1683050-d87b343e49434f5b.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

 　　可以在Search测试套件下创建5条测试用例。其实对于每一条测试用例来说，只是搜索的内容不同，脚本步骤是完全一样的。这样做无疑增加的脚本的冗余，而且不便于维护。假如，百度输入框的定位方式变了，我不得不打开每一条用例进行修改。

 

我们可以过创建关键字的方式，从而实现分层的思想来解决这个问题。

 

#### 创建Robot  Framework 关键字

 

**1、创建资源**

右键“testbaidu”的projec选择“**new resource**”创建资源，下面的“External Resources”只能“Add Resource”

![img](https://images0.cnblogs.com/blog/311516/201409/131640145434083.png)

输入资源名称：

 ![image.png](http://upload-images.jianshu.io/upload_images/1683050-6dd7ed5058736804.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

 

 

**2、创建关键字**

**把User Keyword存放到Resource里**

右键“业务关键字”选择“new User Keyword” 来创建用户关键字。

 ![img](https://images0.cnblogs.com/blog/311516/201409/131640404815391.png)

 输入关键字的名称：

 ![img](https://images0.cnblogs.com/blog/311516/201409/131641015592518.png)

 此时就会看到一个齿轮增加在**业务资源recourse**的下面。

![image.png](http://upload-images.jianshu.io/upload_images/1683050-1292c99ede413c50.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

此时我们保存一下，然后到Windows目录下看一看我们的工程所在目录

 ![image.png](http://upload-images.jianshu.io/upload_images/1683050-70c66ddc6eee268c.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

可以看到业务资源resource.txt和search_suite.txt，都在里面了。他们一个是Resource，一个是TestSuite。

从区别来看，Resource可以当作是一个不能有TestCase的TestSuite，你可以对比一下前面的右键菜单截图，TestSuite只是多了一个New TestCase。

当然，Resource还多了一个Find Usages，这个功能的作用是找出这个Resource都在哪里使用了，所以Resource只是没有TestCase而已。

从图标上看，Resource多了一个齿轮，而齿轮正是User Keyword的图标，意思就是Resource是用来保存User Keyword的。虽然TestSuite下面也可以新增User Keyword，但是我们并不建议大家这样做。

**3、编辑关键字**

![image.png](http://upload-images.jianshu.io/upload_images/1683050-37f0651f37c13bef.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

分析：

对于一个测试用例来说，用户关心的是输入什么内容，得到什么结果。

所以，对于“百度搜索”关键字来说，需要创建两个接口变量*search_key* 和*result* 两个变量，用于接收输入内容和预期结果。`${search_key} | ${result}`

点击Arguments输入框，定义变量，**多个变量从用“|”隔开**。

 在百度用户中使用参数化变量。 

在“业务资源recourse”下的“百度搜索”中，填入需要的关键字、变量、值等。

![image.png](http://upload-images.jianshu.io/upload_images/1683050-d382bc03804ca24c.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

 在keywords中不支持ctrl+alt+空格的联想，所以可以先将代码在case中写好后，粘贴到keywords的text edit中。

![image.png](http://upload-images.jianshu.io/upload_images/1683050-eae85290d727910b.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

**4、添加创建的资源**

切换到测试套件（Search）页面，添加资源（业务关键字.txt）

> tips：没有在suite添加Resource之前，resource的名字都是灰色的，添加成功了之后会变成黑色的。



![image.png](http://upload-images.jianshu.io/upload_images/1683050-2cac01707cb19b17.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

点击Browse找到“业务资源rescourse”所对应的路径，并添加。

![image.png](http://upload-images.jianshu.io/upload_images/1683050-491c656668ce703d.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

 

**5、调用关键字**

现在就可以在测试用例中使用创建的关键字了（百度搜索）。

一个资源rescourse里面可以有多个关键字keywords

![image.png](http://upload-images.jianshu.io/upload_images/1683050-16b4f3b621654665.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

 

对于每一条用例来说，调用“百度搜索”关键字，输入搜索内容，输入预期结果即可。不同关心用例是如何执行的。如果百度输入框的定位发生了变化，只用去修改“百度搜索”关键字即可，不用对每一条用例做任何修改。大大提高的用例的维护性和扩展性。

 

 

继续分层的设计：

 ![img](https://images0.cnblogs.com/blog/311516/201409/131643025278157.png)

 

要记住Project、TestSuite的区别和关系，TestCase和TestSuite的关系；Resource和TestSuite有什么相同和不同。

简单来说：

- 一个project里面有很多testsuite
- 一个testsuite里面有很多testcase
- 它们都可以添加keywords，但recourse不能添加testcase





#### 参考

http://www.cnblogs.com/fnng/p/3969978.html

http://blog.csdn.net/tulituqi/article/details/7585387###