# (11)Robot Framework用户关键字



我们新建一个案例，并针对用户关键字的用法进行介绍。

#### 1、先新建一个用户关键字，不用带arguments。

![image.png](http://upload-images.jianshu.io/upload_images/1683050-b9d93b1dc240fde3.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

在Setting下面的这些呢，Documentation就不多说了。

主要看下面4个设置：

**Arguments**: 设置传入参数

**Teardown**: 设置完成时的动作，比如写上Close All Browsers，表示在这个用户关键字执行完成之后会执行什么关键字。

**Timeout**: 设置超时时间，如写上1min，表示1分钟超时，如果这个关键字执行超过1分钟则认为失败。

**Return Value**: 设置返回值

 

一直以来我都觉得用户关键字就像一个函数一样，有传入参数Arguments，有返回值Return Value，而且还可以用Teardown作为函数完成后的清理动作。

所以我们也要像函数一样来使用User Keyword。

 

#### 2、传入参数Arguments



​    0）变量类型

顺带提一下变量的类型，RF里的变量分两种。

一种是Scalar，可以理解为单值变量。Scalar型变量用$符号开头。

一种是List Variable，list型变量，或者可以理解为数组变量。List型变量用@符号开头。

 

​    1）必填参数

给我们的关键字先添加4个参数，参数之间使用 |  进行分隔。（如果想在默认值里使用 | 作为一个值而不是分隔符，那么就要使用 \| 来表示|的值。

 ![image.png](http://upload-images.jianshu.io/upload_images/1683050-b6503cf51789b0c8.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

如果是这样添加参数呢，表示这些参数都是必填的，我们建一个case3，并调用关键字测试看一下，这里的关键字测试后面的4个格子都是红色，提示参数必填。

![image.png](http://upload-images.jianshu.io/upload_images/1683050-c430d895db22ffcd.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

 

我自己添加了case01，但是后面的4个格子并没有变红。

![image.png](http://upload-images.jianshu.io/upload_images/1683050-fbb5776b06ecfc52.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

​    2）选填参数

假设我们想把其中第2个参数设成选填参数，选填参数就是给参数加上默认值。

默认值使用 =  加上值，如果想默认为空，只写等号就行了。

（默认值的意思不需要细讲了哈，就是如果你在调用这个关键字的时候，如果不给这个选填参数传值，那么就使用默认值作为参数的值）

![image.png](http://upload-images.jianshu.io/upload_images/1683050-8d2460da7abc2766.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

如果我们直接这样加的话，点击OK就会报错。意思是说必填参数不允许在选填参数后面。

也就是说**如果某一个参数设置成了选填参数，那么他后面的参数都必须是选填参数，不能是必填参数。**

在我们这里就是第2个参数如果要可选，要么把3、4参数也都设置上默认值，要么就把第2个参数调整到最后去。我先调整成下面这样，就不会报错了。

![image.png](http://upload-images.jianshu.io/upload_images/1683050-c0a2aac50b49045b.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

 

​    3）List变量参数

List变量也是可以作为参数，但是List变量只能放在最后一位。如果放在前面，就会报错。

![image.png](http://upload-images.jianshu.io/upload_images/1683050-9ad18c15a91d29f9.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

我把arg4改成了@{arg4}这样的List变量，保存时就会提示List变量只能作为最后一个参数。

为啥只能是最后一个呢？从英文的角度看是the last argument，没有s。。。。![得意](http://static.blog.csdn.net/xheditor/xheditor_emot/default/proud.gif)

如果一定要试试最后2个都放list变量呢，也是会报错的。

![image.png](http://upload-images.jianshu.io/upload_images/1683050-6315b57c515ff289.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

 

这是为什么呢？首先，List变量本来来说，他是一个可变的，即List的成员数量不确定。而List作为参数的话，有几个成员就是相当于几个单值参数，他实际上是提供了一种参数个数可变的方式。那么既然可变，如果放在前面的话（见第一个例子的图），他就没法确定传入的参数到底哪些是给arg4的，哪个是给arg2的了；同理，如果放2个List也是不行的（见第二个例子的图），因为也是无法区分传入的参数到底哪些是给arg4，哪些是给arg2。

而且2个可变和1个可变没什么差别滴，所以**最终限定是只能有1个List参数，并且必须放在最后**。

> 但是我最后2个都放list变量居然添加成功了，有点蒙。
> 关于@{List}变量 和 ${Scalar}单值变量

![image.png](http://upload-images.jianshu.io/upload_images/1683050-32f92b6336791d75.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)






最后看一下List参数的实例，其他几种实例应该没什么复杂的，大家自己练习一下即可。

为了看的方便，我把参数数量减少点，只放2个参数，${arg1} | @{arg2}，然后用了个Fail，目的是想把值打印在运行界面上。

![image.png](http://upload-images.jianshu.io/upload_images/1683050-d7acc9b5dbd37c34.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

 

然后我们在case01里给他传几个值。

![image.png](http://upload-images.jianshu.io/upload_images/1683050-dece2fed67cbb384.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

按住Ctrl, 把鼠标移动到关键字上，可以看到agr2前面有个*，表示他可以接受任意个参数，Python里其实也是这样处理多参数的。

运行一下案例，看一下打印出来的内容。

![image.png](http://upload-images.jianshu.io/upload_images/1683050-0d8a336ac5e7c83a.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)





------



继续介绍User Keyword里面的Teardown和Return Value的内容

 

#### 3、Teardown测试完成回收清理

如果有用过Junit的朋友应该记得Junit的案例一般都是有一个Setup、一个Test、一个Teardown的，同样我们的TestCase也是有这2个的，大家可以自己看一下case的Setting。

> python的unittest也是有setUp和tearDown的

那么这个User Keyword的Teardown，作用都是差不多的，如果是Case的Teardown，那么是案例结束的时候做回收清理；而User Keyword的Teardown就是用在调用User Keyword完成之后才会做的数据回收清理。

当然，用于什么可以根据你自己的需求自己调整，并不一定是数据回收清理，是系统关键字或者用户关键字都可以。总之可以理解为关键字完成后还要调一个关键字。参数要用 | 分隔。

我这里改造一下上一篇写的关键字测试，以便于演示Teardown的作用。

![image.png](http://upload-images.jianshu.io/upload_images/1683050-d8768188d090a67b.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

改完的效果是先把arg1的值赋给temp，然后**把arg2的列表第三项值**赋给arg1（这里算个小bug，如果传的参数少于3个就会报错了，这里只是演示，请忽略），然后log打印出arg1和arg2的值。

在Teardown这里我使用了***Set Suite Variable*** 或者***Set Global Variable*** 方法，把temp的值覆盖arg1，并把arg1设为Suite级别的变量，然后我再case01里调用完关键字测试后又用log打印出arg1的值。

![image.png](http://upload-images.jianshu.io/upload_images/1683050-1234580167a57a69.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

 

如果不用***Set Suite Variable*** 或者***Set Global Variable*** 这两个方法的话，在case01里是找不到arg1这个变量的。

![image.png](http://upload-images.jianshu.io/upload_images/1683050-e40820126b013755.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)



接着我们执行一下案例，看看报告。

![image.png](http://upload-images.jianshu.io/upload_images/1683050-fa510f11b6ebaa4a.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

如上面所说，

> 首先把arg1的值111赋给了temp;
>
> 然后又把arg2的第3个值444赋给了arg1;
>
> 最后再把temp的值赋给arg1。

这个就算是一种清理吧，让参数的值在关键字内部发生变化，在Teardown的时候把他的值恢复回来。

 

Teardown这里只能写一行语句，如果你要执行多行语句的话，那最好是再专门写一个关键字，然后在Teardown这里调用这个新增的关键字。

 

#### 4、Return Value返回值

这个应该不用怎么解释了，只是说说用法，因为我们传入参数的时候可以用单值变量，也可以用List变量，那么在Return Value的时候也是可以用单值变量和List变量的。

下面分别用一个例子来说明吧。

 

​    1）单个单值变量返回

我把arg1放到Return Value里了，因为我挺想看看是先Return还是先Teardown，不过我觉得应该还是先Return，顺便验证一下。

![image.png](http://upload-images.jianshu.io/upload_images/1683050-0d133405b4ccc1c2.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

在case02里，我们用一个变量arg5获取这个返回值，然后打印出来。其他的代码都不动。

![image.png](http://upload-images.jianshu.io/upload_images/1683050-7fbcab4278091fd9.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

运行之后，我们看一下日志打印的效果。

![image.png](http://upload-images.jianshu.io/upload_images/1683050-e672f3290a203858.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

看来还是我猜错了，是**在Teardown之后才Return的**.

大家可以看到我标记的地方，是先执行了给arg1赋值111，然后又把这个arg1返回给了arg5，所以arg5的值也是111。我本来还以为应该是444的，所以这里大家也要注意一下。

 

这种单个单值变量返回的是一种比较普遍的，后面几种稍微少见一些，但是也比较有用处。

 

​    2）多个单值变量返回

在设置返回值的时候可能大家已经看到了，这里是支持多个变量返回的，也是用 | 分隔的。

​      2.1）这里我们继续改造，再增加一个变量的值返回。

![image.png](http://upload-images.jianshu.io/upload_images/1683050-491aeb58c8a780cd.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

 

同样的，我们在case3那里也要做一下相应的修改，当然不修改也可以，这里就会比较自由了。我们先看看不改的。图和上面的case3一样，我们直接看执行的结果。

![image.png](http://upload-images.jianshu.io/upload_images/1683050-474c318961426968.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

虽然我们用的是${arg5}，但是因为返回了2个值，他自动的转换成了list

 

​      2.2）那么可能有人觉得这里返回的是list，我们最好用list变量来接收，那么我们把第一个${arg5}改成@{arg5}

![image.png](http://upload-images.jianshu.io/upload_images/1683050-9c6fff1789be305e.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)



再运行一下，看看结果

![image.png](http://upload-images.jianshu.io/upload_images/1683050-a6556d231ef4e5c2.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

差别基本不大，只是在与${arg5}和@{arg5}的显示不同。

 

​      2.3）那么还有一个方式，因为我们知道有2个返回值，所以可以用2个变量来获取值。

![image.png](http://upload-images.jianshu.io/upload_images/1683050-f3c842d544fe79ae.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)



接着运行一下，看看日志

![image.png](http://upload-images.jianshu.io/upload_images/1683050-e6c130237efd745c.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

这样可以直接使用对应的变量了。

 

上面的3种方式前两种差不多一样，第三种是最好能知道返回值的个数，这样可以一一对应，如果不一样怎么办呢？

**A：返回值个数大于取值变量个数。**

**这种放2个变量返回多余2个的用法在新版本Robotframework里已经不支持**

实际上在2.1里已经有了一个类似的，${arg5}会自动转为list。这是只有一个取值的变量，那么我们试试多个的。

![image.png](http://upload-images.jianshu.io/upload_images/1683050-4a65064bb02ed689.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)



在case06里还是用2个变量取值

![image.png](http://upload-images.jianshu.io/upload_images/1683050-853b02736ccea01d.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)运行一下看看结果。

![image.png](http://upload-images.jianshu.io/upload_images/1683050-5587c881d0b0776a.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

我这里运行会报错` Cannot set variables: Expected 2 return values, got 3.`

我把值改为`@{arg6}`, 接收list之后便可以成功运行了。

![image.png](http://upload-images.jianshu.io/upload_images/1683050-9c2f0ff74d12c961.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

而参考文章是直接运行成功，我再看了确认了一下文章。

**这种放2个变量返回多余2个的用法在新版本Robotframework里已经不支持**

所以失败是很正常的。

他会先把第一个值给了arg5，然后把后面的值给了arg6，于是arg6变成了list变量。

所以可以得出结论，对于多个取值变量的个数少于返回值的个数，他会先把前面的值一一对应的给前面的取值变量赋值，这些变量仍然是单值变量，而最后一个变量会转成list变量接收剩下的值。

 

**B：返回值个数小于取值变量个数。**

我们把return value改成只有一个${arg1}，然后运行一下，看看结果。

![image.png](http://upload-images.jianshu.io/upload_images/1683050-37ce7064ccfecfb4.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

这种就会报错了，因为只传回来一个值，而这里期待的是多个值，list-like的像list的变量。

 

因此，知道返回值的个数比较重要，如果不确定返回值的个数，如同接下来的那种情况，最好使用list变量或单个变量来取值，避免出现返回值个数少于取值变量个数的情况。

 

​    3）list变量返回

因为list变量本身就是不确定有多少个成员，所以对于这种返回值，最好使用list变量或单个变量来取值（即2.1和2.2的用法）。那么返回值里返回1个list还是多个list都无所谓了，因为他还会组装成一个大的list。

稍微改造一个复杂的：

![image.png](http://upload-images.jianshu.io/upload_images/1683050-c7ad54b36cfe73df.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

返回值里我们放了一个list变量加上一个单值变量再加上一个list变量，而在case3里的取值就用1个变量就可以了。

![image.png](http://upload-images.jianshu.io/upload_images/1683050-30a8bfd0f1505ca9.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

最后运行一下，查看结果。

![image.png](http://upload-images.jianshu.io/upload_images/1683050-9b6149c1be06671b.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)









#### 转载：

http://blog.csdn.net/tulituqi/article/details/7906130

http://blog.csdn.net/tulituqi/article/details/7907770