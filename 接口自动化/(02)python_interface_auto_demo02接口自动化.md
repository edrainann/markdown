

## 一、实现功能如下：

git地址如下：https://github.com/edrainann/python_interface_auto/tree/master/interface-demo02

demo02更新功能：
1. 通过excel管理测试用例；
2. 支持json、图片格式作为body发送post请求；
3. 对excel的测试数据进行变量处理，可以自动生成测试数据；
4. 保存cookie，方便后续接口使用；
5. 生成log，查看日志。

demo01功能：
1. 对get/post接口进行封装，实现get/post请求；
2. 运行unittest框架，通过HTMLTestRunner生成测试报告；
3. 对报告进行邮件的发送。

## 二、代码分布结构如下：

1. main文件夹：flow02.py主运行文件，执行整体流程，运行之后可以生成相应的测试报告，并以邮件形式发送；  

2. report文件夹：存放测试结果报告；

3. test_case文件夹：存放测试用例数据，比如请求类型get/post、请求url、请求header、请求数据等。test_api_case01.py为用例用法介绍，实际项目中可以按照不同模块新建python package，来存放不同模块的接口用例；

4. data文件夹：
    data_config.py：初始化excel文件。
    get_data.py：获取excel文件中相应数据的方法封装，获取excel中对应表格内的数据，excel的行列数据等
    dependent_data.py：判断用例之间是否存在依赖关系并获取依赖数据

5. data_config：存放请求中涉及到的header、data、cookies等数据；

6. log：存放测试完成之后生成的日志文件，可以查看日志定位问题；

7. util文件夹：对测试接口相关方法的封装。
    HTMLTestRunner.py：生成测试报告进行封装；
    send_mail.py对发送邮件进行封装；
    send_get_post.py对接口请求类型进行封装,根据请求类型的不同执行对应的get/post方法
    common_tool.py：常用def的封装
    use_excel.py：对excel文件的读写操作
    use_header.py：从请求返回数据中拿取数据作为下一个接口的请求header数据
    use_json.py：从json文件中拿取想要的数据

    ![image.png](https://upload-images.jianshu.io/upload_images/1683050-1a6bd7f014d87085.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

## 三、下期欲实现功能：

1. 优化测试数据驱动，存储读取
2. 优化返回值的断言判断
3. 关联数据库查询
