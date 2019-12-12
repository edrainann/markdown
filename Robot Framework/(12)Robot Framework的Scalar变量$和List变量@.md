# (12)Robot Framework的Scalar变量$和List变量@

#### 转载

[变量的声明、赋值及其使用]( http://blog.csdn.net/tulituqi/article/details/7984642)
[List Variables-List变量及其用法]( http://blog.csdn.net/tulituqi/article/details/7907981)

# 一、Scalar变量$的声明

## 1、变量标识符

每个变量都可以用  **变量标识符{变量名}**    来进行表示，变量标识符在之前用户关键字的地方介绍了一下，Scalar变量用`$` 作为标识符，List型变量用 `@` 作为标识符，不过这只能作为一个初步的区分，使用`$` 的变量，实际上也可以在接收List值后转化成List变量。转化的内容会在List变量里介绍。

## 2、变量声明

其实这里没有什么特别的变量声明，因为RF底层是Python，所以他的语法也有些类似，变量不需要特定声明，只要有初始化赋值即可使用。

如果硬要说有声明，那可以把我们在TestSuite下面手动添加的变量理解为声明吧。比如我们可以在TestSuite上点右键或者在Edit区点Add Scalar或Add List来新增变量。

![image.png](https://upload-images.jianshu.io/upload_images/1683050-0d64421d08b4b080.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

实际上也可以看做另一种形式的变量赋值，一般相当于使用Set Variable进行初始化赋值。

 可以Import需要用到得Library

## 3、变量赋值

赋值也是有几种的，根据自己的需求进行处理吧。

### 1）Set 赋值

通常这种方式主要使用Set Variable或类似的使用了Set的关键字对变量进行赋值。例如：

![image.png](https://upload-images.jianshu.io/upload_images/1683050-3b9f70170113447b.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

### 2）Get 赋值

主要用于返回值上，包括系统关键字的返回值和用户关键字的返回值（从广义上说，Set那个也是返回值），例如：

![image.png](https://upload-images.jianshu.io/upload_images/1683050-e364cbd5d87221fa.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

之前的用户关键字里也有很多例子。

### 3）运行时赋值

详见[流程与数据分离最后一篇](http://blog.csdn.net/tulituqi/article/details/7855978)，使用小v 的argument。

 

特别注意：如果一个变量没有经过赋值，使用时会报错的。

 

## 4、变量的作用域

 通常情况下，每个变量默认都是局部变量。

一个case里的变量，作用域在这个case内部；

一个userkeyword里的变量，作用域在这个userkeyword内部；

一个文件型suite里的变量，作用域在这个suite内部，所有下面的case也都可以使用。

一个目录型suite里的变量，作用域在这个目录内，他下面的文件型suite是无法使用的，所以一般在目录下新增变量没有太大意义。

作用域是可以修改的，即通过一些系统关键字，对变量进行作用域的设定，常用的关键字有：

Set Global Variable ——设定全局级变量

Set Suite Variable ——设定suite级变量

Set Test Variable ——设定case级变量

 

# 二、变量的使用

 

其实使用变量我觉得不需要说太多了，变量的赋值可以看一下用户关键字的返回值部分，里面的单个返回值和多个返回值其实就是给单个变量赋值和多个变量赋值的例子了。

这里再列几种之前没有写过的例子吧。

## 1、在判断中使用

![image.png](https://upload-images.jianshu.io/upload_images/1683050-c0fe75ee24bab76c.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

例如这样写，在第二行的判断中可以直接写判断条件，现在的条件成立，于是运行结果如下：

![image.png](https://upload-images.jianshu.io/upload_images/1683050-9e16638fe82b26b6.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

 

## 2、字符串使用

如果想把变量作为一个字符串的一部分，可以直接这样写

![image.png](https://upload-images.jianshu.io/upload_images/1683050-013545b74c79b491.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

运行结果如下：

![image.png](https://upload-images.jianshu.io/upload_images/1683050-5afbe7e9cfe3847b.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

 

实际上默认情况下RF里的变量都是字符型的，并且两个字符串连接不要加什么符号，直接连起来就行了。

如果你写成aaa+${text}bbb，最终结果就是aaa+8866bbb，他会把你写的任何内容都当作字符串的。

 

## 3、参与运算

看了上面的例子之后，可能有人说我想用123作为数值进行计算该怎么用。

这里要用到一个关键字Evaluate。先看个例子：

![image.png](https://upload-images.jianshu.io/upload_images/1683050-a36399676cf508c5.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

运行结果如下：

![image.png](https://upload-images.jianshu.io/upload_images/1683050-2147dd279873547e.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

 

他的作用是可以让你加载Python的一些库，依赖于Python的语法，相当于直接运行对应的Python语句得到结果。

像上面的例子没有加载库，因为加减乘除这些是Python基本库就有的。下面放一个加载Python库的，如下图，他加载了random和sys两个库，并使用相应的语句生成一个随机数。

![image.png](https://upload-images.jianshu.io/upload_images/1683050-676ea7d0d2e1b73f.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

运行结果：

![image.png](https://upload-images.jianshu.io/upload_images/1683050-a88630c1ff93adc5.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

具体可以参考一下RF的userguide文档中BuiltIn部分。



------

介绍List Variables-List变量及其用法。

# 一、List变量及其用法

 在我们前面几篇文章里用到了很多List变量，相信以后各位也会碰到需要使用的地方。

## 1、List变量赋值

和Scalar类似的赋值，除了用Set Variable还可以用Create List。

 ![image.png](https://upload-images.jianshu.io/upload_images/1683050-da7d304c1827aa82.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

运行一下：

 ![image.png](https://upload-images.jianshu.io/upload_images/1683050-a2fb3b4c01dacfcc.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

 

## 2、List变量使用

在使用上要注意看关键字的参数到底是Scalar的还是List，区别就是看变量名前面是否有 ***** （星号），如图：

![image.png](https://upload-images.jianshu.io/upload_images/1683050-f9a7b893538efee1.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

这个关键字：

**name就是支持传入Scalar型的参数**

**args就是支持可以传入List型值的**。

所谓List型值其实是说支持多个值，即不确定个数的，List-like一样的值，如下图的2、3行：

 ![image.png](https://upload-images.jianshu.io/upload_images/1683050-1a3271add8a5b1a6.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

 运行一下：

![image.png](https://upload-images.jianshu.io/upload_images/1683050-1bd63905c68f586c.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

当然，并不是说没有星号的就不能用List的，只要是多个变量就可以用List传值。

![image.png](https://upload-images.jianshu.io/upload_images/1683050-235069ef8273810a.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

像Log这个关键字，他有2个Scalar型的传入参数，如果我想传List也是可以的，但是必须是一个有2个元素的List，因为Log的第二个参数有默认值，所以如果你传入1个元素的List也是可以的，但是如果传入3个元素的List，那么就会报错了。

![image.png](https://upload-images.jianshu.io/upload_images/1683050-bfa42b1ae4cbc878.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

运行结果：

![image.png](https://upload-images.jianshu.io/upload_images/1683050-2aa2f06b5b36b09c.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

你可以写成上面这样，但是我并不推荐这种做法，这里只是为了讲解List的作用而做的示范。

 

综上，对于有多个参数的关键字，你可以按照需要传入多个Scalar变量，也可以传入List变量，根据你自己的需要选择合适的方式。

 

# 二、变量转换

之前提过了**Scalar变量**用 **$** 作为标识符，**List型变量**用 **@** 作为标识符，而且他们是可以进行转换的，理论上可以互相进行转换，但是也有一点限制。

## 1、List变量转换成Scalar

这种转换的效果是把整个List变成一个Scalar，我觉得作用主要是对于那些只接收Scalar变量参数的关键字，而你又想传List的全部值的时候，或者其他你需要的把List当作Scalar的情况下使用。

例如Fail关键字，他的msg这个参数只能接收Scalar变量。

 ![image.png](https://upload-images.jianshu.io/upload_images/1683050-59f720241f92b455.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

如果你想用Fail，把f这个变量的值都打出来，下面这样写是肯定会报错的。

![image.png](https://upload-images.jianshu.io/upload_images/1683050-3155e8bb44c06ab7.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

运行结果：![image.png](https://upload-images.jianshu.io/upload_images/1683050-b03da73c0cd91d61.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

报错信息为 Keyword 'BuiltIn.Fail' expected 0 to 1 arguments, got 3.

那么我们转换一下，这样写就不报错了。

![image.png](https://upload-images.jianshu.io/upload_images/1683050-2b0fd94e66df5c59.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

运行结果：

![image.png](https://upload-images.jianshu.io/upload_images/1683050-bc32af92b475a144.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

 

## 2、Scalar变量转换成List

其实以前介绍用户关键字的时候也演示过，对于返回值是List的关键字，如果你给要赋值的变量写的是Scalar的，他会自动把他变成List的。

还是上面这个例子，我们把@{f}改成${f}

![image.png](https://upload-images.jianshu.io/upload_images/1683050-193739807f5041e3.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

这样也是OK的。

【Question】这里没懂如何就成功发生转换了。

## 3、转换的限制

但是，转换是有一点限制的。

首先，变量要进行赋值（类似于初始化）之后才能使用，如果没有进行过赋值的RIDE里会有颜色提示，用一个前面的图

![image.png](https://upload-images.jianshu.io/upload_images/1683050-54603d3625a29995.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

图中的紫色${f}就是提示你他没有进行过赋值，此时而系统会默认他就是@{f}的Scalar形式，这个可以看上面的执行结果。

那么这个限制就在Scalar转换成List的地方，系统会默认${f}是@{f}的Scalar形式，但是他并不会默认@{f}是${f}的list形式，即使${f}已经是list了。

看一下例子：

 ![image.png](https://upload-images.jianshu.io/upload_images/1683050-f54cb5d23afaa0e2.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

 这样写的话运行就会报错：Non-existing variable ['@{f}'](mailto:'@%7Bf%7D').

那这时候${f}有什么用呢？除了前面介绍的作为Scalar型变量，值是所有元素，还可以根据index直接获得某个元素。

例如：

![image.png](https://upload-images.jianshu.io/upload_images/1683050-44ad9faaad27443f.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

运行结果：

![image.png](https://upload-images.jianshu.io/upload_images/1683050-4005d112eb4605d8.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

 

看完上面的限制之后，还要补充一点，系统会默认${f}是@{f}的Scalar形式，这个也是有限制，必须是${f}没有被赋值过的情况，如果${f}被赋值过是什么情况呢？

看看例子：

![image.png](https://upload-images.jianshu.io/upload_images/1683050-cf22713e10a2d2e3.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

你觉得${f}会是什么值呢？

![image.png](https://upload-images.jianshu.io/upload_images/1683050-2fcb820e5d251603.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

【Question】这里跟教程的截图，返回结果不一致了。有点蒙，理解还没到位。

下面是教程给出的答案：

![image.png](https://upload-images.jianshu.io/upload_images/1683050-accb8bcb645dfe35.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

 

## 所以，这里要注意，尽量不要让Scalar和List的变量重名，特别是你要用作不同的用途的时候。

一个重要的影响就是会导致下面的List元素获取出现越界的情况。

 

# 三、List元素的获取

List元素的获取只有一种方式，无论是一维List还是二维List，都是用${变量名[index]}，就是用$的变量，在变量名后面加上方括号，里面是序号。

## 1、一维List的例子

![image.png](https://upload-images.jianshu.io/upload_images/1683050-59eaa1efa6272d39.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

运行：

![image.png](https://upload-images.jianshu.io/upload_images/1683050-76a9ae99b71568b9.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

 

## 2、二维List的例子

![image.png](https://upload-images.jianshu.io/upload_images/1683050-253b700aa384911b.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

运行结果：

![image.png](https://upload-images.jianshu.io/upload_images/1683050-f8ed971d3d30a5fe.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

如果log后面只写${f[1]}，那么得到的结果如图：

![image.png](https://upload-images.jianshu.io/upload_images/1683050-299f9c387b194d47.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

参考文章给出的运行结果：

![image.png](https://upload-images.jianshu.io/upload_images/1683050-63fcc4d1cd052682.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

 

总之，就是用方括号来表明你需要的元素，如果是多维List，要想清楚你到底要哪层的，不要写少了或者写多了。

【Question】这运行结果不一致，真是令人困扰。。。

 


