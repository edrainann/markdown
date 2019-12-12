# (01)Jmeter录制脚本-BlazeMeter的安装及初步使用



### 关于Jmeter的脚本录制方式

1）Jmeter+Badboy：方便，兼容性较差

2）Jmeter系统自带的录制方式：操作较复杂

3）Jmeter + 其余插件：兼容性教高

目前我使用的是方法三

### BlazeMeter Chrome插件官方简介

BlazeMeter是一款可以记录所有HTTP流量并在10分钟内创建一个负载测试并且与Apache JMeter兼容的chrome插件。目前普通的使用方法就是一种脚本录制工具可以辅助Jmeter完成脚本的创建。BlazeMeter扩展提供了两种测试模式：一种是FollowMe模式，在这种模式下不需要设置和脚本。 只需浏览您的网站，一群虚拟用户就会按照您的操作 - 模拟即时测试的负载。 查看实时报告并即时调整您的测试，一键即可进行动态测试；另一种模式是为您为您创建JMeter脚本，记录所有浏览活动以创建JMeter脚本并自动将其上传到BlazeMeter。 单击执行或将其导出为JMeter .jmx文件。

BlazeMeter的功能和要求：
\- 记录浏览器发出的所有HTTP请求；
\- Apache JMeter兼容；
\- 运行没有测试脚本应用程序的测试；
\- 为多达1,000,000个并发用户运行测试；
\- 需要BlazeMeter帐户（免费）；

所需工具：

1）下载安装Chrome浏览器

地址：http://www.google.cn/intl/zh-CN/chrome/browser/desktop/index.html 

2）离线安装BLazeMeter插件 （该插件是针对Chrome浏览器可用）

地址：http://www.cnplugins.com/devtool/blazemeter-the-load-testi/download.html 

若通过Chrome自带的“扩展程序”下载需要 科学上网...

软件使用截图

![image.png](https://upload-images.jianshu.io/upload_images/1683050-e663b4a8e6f65480.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

注意，该界面前四个按钮在非登陆的状态下都可使用，但是.jmx按钮是录制完成后导出.jmx文件，只有在登录BlazeMeter状态下才可以使用。 

BlazeMeter登录 需要科学上网注册账号 >.<

而且有时候导出的时候，会出现网络错误...这点比较坑的，但是方便算是比较大的优点吧

 ![image.png](https://upload-images.jianshu.io/upload_images/1683050-299f6ecb9b9d1849.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)



