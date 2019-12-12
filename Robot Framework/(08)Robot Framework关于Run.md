#  (08)Robot Framework关于Run

1、先看看截图

 ![image.png](http://upload-images.jianshu.io/upload_images/1683050-ebd874466b4acf41.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)



在截图中，我们依次来说。

1) Execution Profile：选择运行方式，里面有pybot、jybot和custom script。其中我们默认是用pybot来运行案例，jybot需要安装Jython的支持。custom script是选择自定义的脚本来运行。就目前而言，我们不用修改了，默认pybot即可，以后我们再研究一下其他2个。

2) Start和Stop:这两个应该不用说了，运行和停止案例。

3) Report和Log: 报告和日志，要运行之后才能点击。他们的区别么，我的感觉是报告更多是结果上的展示，日志更多是过程的记录，更多使用的还是日志。

4) Autosave: 自动保存，如果不勾选，在修改了案例之后如果没有保存的话，运行案例时会提示是否保存。勾选则在运行时自动保存了。

5) Arguments: pybot的参数（或者jybot等），比如我后面截图里加上了一个参数。完整版的参数可以在doc命令行输入pybot.bat --help

6) Only Run Tests with these Tags: 只运行这些标记的测试案例。

7) Skip Tests with these Tags: 跳过这些标记的测试案例。6和7这2个就和我们前面讲过的Tags有关系了，当你的案例多了以后，用tags来管理和运行案例是比较方便的，你可以根据需要只运行某些标记的案例或者跳过他们。（当然前提是你的案例要能独立运行，没有互相依赖。后面我们的案例设计也是要一样要在这个大前提下进行）

最后下面的2个区域，左边的是选择运行哪些案例的，如果不选就是全部。右边的区域是运行信息输出区域，运行过程的某些输出信息以及运行结果都会在这里显示。

 

2、下面我们来运行一下看看。

![image.png](http://upload-images.jianshu.io/upload_images/1683050-494eb363f8829911.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

为了简单演示一下刚才说的Arguments，我加了个参数 -t baidu02。

如果只有1个case的话，其实加不加效果是一样的。但是现在有多加个案例的话，rf就只会按照我给的参数只运行叫baidu02的案例。

再往下的三行：

```
Output:  c:\users\a\appdata\local\temp\RIDE2zeep_.d\output.xml
Log:     c:\users\a\appdata\local\temp\RIDE2zeep_.d\log.html
Report:  c:\users\a\appdata\local\temp\RIDE2zeep_.d\report.html

```



第一个是output.xml，具体的作用么，可以运用在jenkins的集成上，用这个文件来输出报告的，目前没有使用过。http://blog.csdn.net/tulituqi/article/details/7632560

另外2个html文件就是和Run页面看到的2个按钮有关联的了，在图2运行完成之后，那2个按钮也都可以点击了，点击后会显示对应那个html文件。

------

####  Q&A

有时候运行Robot Framework不会出现output.xml的文件

有时候运行Robot Framework在底部不会出现message log

google说是python编码的错误，暂时未找到解决方案。

之前也有尝试解决该问题（https://www.jianshu.com/p/b23076315df0的Q&A部分）

修改testrunner.py文件的第400行pop方法之后，会出现这样的错误，没有run。

参考链接：https://jingyan.baidu.com/article/380abd0a08c9511d90192c15.html

 ![image.png](http://upload-images.jianshu.io/upload_images/1683050-c438852f86bf2a4c.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

可能是因为网上直接copy的代码格式有点问题

自己手动把代码敲了一遍，如下：

```
    def pop(self):
        result = ""
        for _ in xrange(self._queue.qsize()):
            try:
                result += self._queue.get_nowait()
            except Empty:
                pass
        try:
            result=result.decode('UTF-8')
        except UnicodeDecodeError:
            pass
        return result
```

但是我这样试了之后仍然没有message log 在底部，只不过log 中关于utf-8的错误提示消失了。



又尝试另一种网上流传较多的方法，终于找到解决使用robot framework + selenium测试时，RIDE运行一次后不显示log的问题。

参考链接：http://blog.csdn.net/qq_15055139/article/details/72871886

简要概括：

使用chrome浏览器运行case结束后，chromedriver.exe进程仍在运行中。

此时我们可以在任务管理器中，将chromedriver.exe进程结束掉。

![image.png](http://upload-images.jianshu.io/upload_images/1683050-d9fb94849dfb0b25.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


这样终于可以显示Message log成功了，也不会出现没有output.xml的情况了~

------





#### 参考

http://blog.csdn.net/tulituqi/article/details/7632560