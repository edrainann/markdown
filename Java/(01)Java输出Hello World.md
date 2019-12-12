# (01)Java输出Hello World

通过Eclipse输出显示hello world

前提：JRE，JDK的环境已经装好

#### 创建Java Project

File -》New -》 Java Projece

![image.png](http://upload-images.jianshu.io/upload_images/1683050-5f0fc10e41c3380c.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

![image.png](http://upload-images.jianshu.io/upload_images/1683050-df0e8b520bce1cfe.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)



#### 创建Package

右键刚刚创建的Project：test00， 选中 Package

![image.png](http://upload-images.jianshu.io/upload_images/1683050-1b80bdab6c8c3aae.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

![image.png](http://upload-images.jianshu.io/upload_images/1683050-cdfc045058967b19.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

#### 创建Class

右键刚刚创建的Package：test001，选中Class

![image.png](http://upload-images.jianshu.io/upload_images/1683050-a62f0e314b4e82e7.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

Class的名字最好开头大写，其余的选项可以暂时不用管。

![image.png](http://upload-images.jianshu.io/upload_images/1683050-53fa7a956227a8ae.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

#### 运行HelloWorld

在test000.java中输入 显示HelloWorld的代码

```
package test001;

public class test000 {
	public static void main(String []args) {
        System.out.println("Hello World");
    }
}
```

点击第五个图标，运行程序。

在Console会显示运行结果
![image.png](http://upload-images.jianshu.io/upload_images/1683050-11e6e5fd210cce58.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

![image.png](http://upload-images.jianshu.io/upload_images/1683050-0a6ce09ec5bc9c82.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)