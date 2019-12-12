# (01)Robot Framework介绍

#### 介绍

Robot Framework 官方网站：<http://robotframework.org/>

Robot Framework 的架构是一个通用的验收测试和验收测试驱动开发的自动化测试框架（ATDD）。它具有易于使用的表格来组织测试过程和测试数据。

#### 特点

它使用关键字驱动的测试方法。

- 使用简单

- 非常丰富的库

- 可以像编程一样写测试用例

- 支持开发系统关键字


通过open browser 、input text、click button 和 close browser，等“关键字”，这些关键字由 <font color="red">**robotframework-selenium2library** </font>类库所提供。

当然，我们也可以自定义关键字。
其检测能力可以通过测试库实现可以使用 Python 或 Java 的扩展，用户可以使用相同的语法，用于创建测试用例创建新的更高层次的现有的关键词。
Robot Framework 的操作系统和应用独立框架。核心框架是使用 Python 和运行在 Jython（JVM）和IronPython（.NET）。



#### 模块化&架构

![Modular](http://otfah9orz.bkt.clouddn.com/rf_modular.png)

Robot framework 本质上是基于 Python 语言开发的一个框架，它提供了一套独立的语法。它本身只提供基础的一些功能。比如，它自带的 **Builtin** 库中提供的关键字告诉你如何定义变量、数组和字典，打印信息，分支语句和循环等。以及框架本身所提供的 “自动化” 功能，如何组织用例，生成测试报告。

如果你想实现某一类型的自动化测试，如中接口、UI 或 移动APP的自动化，需要通过第三方Library完成。

#### 支持的 Library

Robot Framework 所支持的库主要分 **标准库** 、 **扩展库** 和 其它 。 标准库提供基本功能，扩展库提供特定领域的操作。

因为 Robot Framework 所支持的测试库非常多，这里例一些常用的。

- **Web 自动化测试**：SeleniumLibrary，Selenium2Library，Selenium2Library for Java、watir-robot 等。
- **Windows GUI 测试**：AutoItLibrary。
- **移动测试**：Android library、iOS library、AppiumLibrary 等。
- **数据库测试**：Database Library (Java)、Database Library (Python)、MongoDB library 等。
- **文件对比测试**：Diff Library。
- **HTTP 测试**：HTTP library (livetest)、HTTP library (Requests)等。

查看所有[Librarys](http://robotframework.org/#libraries)

#### 例子

Robot Framework语法

```Robot Framework
*** Settings ***
Suite Setup       Start Selenium Server
Suite Teardown    Stop Selenium Server
Library           C:/Python36-32/Lib/site-packages/Selenium2Library
Resource          keywords.txt

*** Test Cases ***
baidu01
    Open Browser    http://www.baidu.com    chrome
    百度搜索    root    boot
    Close Browser

baidu02
    Open Browser    http://www.baidu.com    chrome
    Input Text    id=kw    framework
    Click Element    id=su
    sleep    1
    ${title}    get title
    Should Contain    ${title}    robot
    Close Browser

```

#### 参考

http://www.testclass.net/rf/introduce/