#  (03)Robot Framework菜单栏&工具栏

目前放弃python3去折腾RIDE了，不兼容的地方还蛮多，各种报错。

还是老老实实先用python2用RIDE吧。

我把python3的环境变量全部都删除之后，win10系统就自动用上了python2

![image.png](http://upload-images.jianshu.io/upload_images/1683050-87eda738708d6e7a.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)



推荐两个快捷键：

F5 ：Search Keywords搜索关键字

Ctrl + Alt + 空格 ：content assistance 帮助查找

------

我把RIDE的界面大致分了四个区域：菜单栏、工具栏、案例及资源区、工作区，如下图


![image.png](http://upload-images.jianshu.io/upload_images/1683050-8b05541ccb321ce1.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


 update：现在第5个run改成macros宏命令了

![image.png](http://upload-images.jianshu.io/upload_images/1683050-7bfa5958e3443340.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)



菜单栏：RIDE所有的功能都在这里面；

工具栏：比较常用的功能，可以快捷操作；

案例及资源区：这里将会是一个目录一样的树形结构（当前目前是刚打开的样子，里面只有一个空的external resources）

工作区：这里是我们主要编辑案例，运行案例的操作区。

 

其实我一直在想，这个工具怎么讲大家更容易理解，最方便的应该是带着实际的例子结合操作来讲解，这个肯定会有。不过我觉得还是要对工具全面介绍一下先，当然对于菜单栏和工具栏，只是罗列功能描述，因为后面都会用到的；对于案例区和工作区，我会以实际案例进行讲解。

 

#### 菜单栏&工具栏

这里很多大家都熟悉的常用命令就不细说了。其实大部分的命令在后面的讲解中还会出现的。所以每个菜单里我找点重点来说。

1、File 文件

![image.png](http://upload-images.jianshu.io/upload_images/1683050-2c6547ec2c4ddc14.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


| Open           | 打开一个文件 |
| -------------- | ------------ |
| Open Directory | 打开一个目录 |

对应工具栏这2个图标：![image.png](http://upload-images.jianshu.io/upload_images/1683050-6cf5ad4525a6067b.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)




| Reload Directory | 重新加载目录 |
| ---------------- | ------------ |
|                  |              |

对应工具栏的图标![image.png](http://upload-images.jianshu.io/upload_images/1683050-6abde62ce6ce7ccb.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


这个功能是一个不错的改进，在用0.42的时候还没看到过，应该是最近新增加的。作用就是重新加载整个目录（或者说工程）。

在0.42的时候我一般都是用截图里Exit上面的最近打开的工程这个来做用作重新加载。

Save和Save All，对应图标：![image.png](http://upload-images.jianshu.io/upload_images/1683050-e3a72c62a5214a86.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
，应该不用细说了，你修改了多个文件，用右边的图标可以全部保存。

 

2、Edit 编辑

![image.png](http://upload-images.jianshu.io/upload_images/1683050-193ab578d5fbb470.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

这里很多功能大家都很熟悉了，不多说了。

 

3、Tools 工具

![image.png](http://upload-images.jianshu.io/upload_images/1683050-356c27885f12f26e.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


| Manage Plugins         | 管理插件             |
| ---------------------- | -------------------- |
| Search Unused Keywords | 搜索没有使用的关键字 |
| Preferences            | 参数配置             |
| Preview                | 预览                 |
| Content Assistance     | 内容助手             |
| Search Keywords        | 搜索关键字           |
| View RIDE Log          | 查看RIDE日志         |

| Run Test Suite | 运行Test Suite |
| -------------- | -------------- |
| Stop Running   | 停止运行       |

最后2个的对应图标为![image.png](http://upload-images.jianshu.io/upload_images/1683050-e10a1582fbf19c3b.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


常用的：最后2个肯定常用。

另外我认为比较常用的一个是**F5-Search Keywords**，对于我们在写案例的时候可以方便的查找测试库的关键字及其参数和样例等。

![image.png](http://upload-images.jianshu.io/upload_images/1683050-dea8e665e7d33295.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


另一个是**content assistance**，不过这个功能有点不太方便，主要是快捷键，因为Ctrl+空格和我们的中英文切换冲突了，而Ctrl+alt+空格又按着不太习惯。我的做法是把输入法的Ctrl+空格改成别的，这样就可以使用了。

![image.png](http://upload-images.jianshu.io/upload_images/1683050-ff6c5926271b3825.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


这里有个新增的功能Search unused Keywords=查找没有用过的关键字，关键字多了以后可能会有些没有用过的，这个可以比较方便的查找。

 

4、Navigate 导航

![image.png](http://upload-images.jianshu.io/upload_images/1683050-0e57517fc2b77e85.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


后退和前进，可以方便的在案例区进行跳转，对应图标![image.png](http://upload-images.jianshu.io/upload_images/1683050-5cf30795e218c437.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

 

5、Run 运行（其实我觉得这个名字不太合适，8过我也想不出叫啥名字更贴切，功能可以看下面的介绍）

![image.png](http://upload-images.jianshu.io/upload_images/1683050-2a0fb6f1995fff0e.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

 这个设置我没怎么用过，简单研究了一下，就是可以自己写好一些命令行的语句，可以直接通过菜单来运行。主要是针对通过命令行方式运行的一些命令，可以自定义菜单。

比如下面我简单配置了2个


![image.png](http://upload-images.jianshu.io/upload_images/1683050-0ced55db621abbff.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


再看run的菜单，就多了这2个了，可以直接点击运行。


![image.png](http://upload-images.jianshu.io/upload_images/1683050-212fdeea99aa5000.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
点第二个运行一下看看，他直接在工作区增加了一个tab页，运行结果也显示在上面了，以后有空我们再深入研究这个。


![image.png](http://upload-images.jianshu.io/upload_images/1683050-104640815df7b551.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)




6、Help 帮助

![image.png](http://upload-images.jianshu.io/upload_images/1683050-63dd5878c9e9f7ae.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


这个不用说了吧。

 

小结：简单介绍了一下RIDE的菜单功能，后面我们还会用到他们，先熟悉一下。



#### 参考

http://blog.csdn.net/tulituqi/article/details/7584795