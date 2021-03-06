# (08)《Web 测试囧事》——从测试角度照照开发的灯下黑

转自：https://testerhome.com/topics/9716



> 雷辉：ThoughtWorks高级质量保证工程师，多年研发与测试经验，丰富的大型商业软件的测试工作实战经验，精通自动化、性能和探索式测试，以在软件中发现各种隐藏Bug为乐。

我是TW一名测试人员，不敢说是老司机，还算资深吧，因缘际会入了IT，做得测试这一行。其实按照大学的专业，我现在应该是一名杰出的销售大师，哈哈。然而，一入IT深似海，回头已是中年身。我有多年的Java开发经验，后来转为测试，对开发和测试都有了解，所以今天打算从测试角度出发，为开发人员照照灯下黑。

## 当我们在本机自测时，通常漏测的是什么

完成一个功能卡的简化流程是这样的：程序员开发→自测→QA测试。通常开发一张卡的工作量为1～3天，少量卡需要5天左右。从过去的数据看，从开发接手的卡，90%以上都有Bug，没有Bug的卡比较少，要么是功能特简单，不易出错，要么是开发经验老道又细心。

又年轻、工作年限又短的工程师们通常漏测的点在哪里呢？

#### 1. 自测过程中没有跑通所有的分支业务流程

有时系统需要和第三方的系统进行集成，开发工程师对第三方系统不熟悉，自测过程中，有不少分支流程没有执行，恰巧就在这里隐藏着很多Bug。当系统业务过于复杂，开发只熟悉某一部分，在这种情况下自测时，需要放在完整的业务流程中测试，真实地去使用，会碰到意想不到的事情发生。我们团队开发一个类似Jenkins的持续集成平台，最近给构建过程增加了多阶段（Stage）构建的功能，用户可以在每个Stage执行不同的任务。

开发自测时，通常都会覆盖主流程，如：

- 多Stage正常构建；
- 多Stage构建中出现失败的流程；
- 多Stage构建过程中被取消构建。

还有更多的流程分支需要测试：

**示例1** ： **构建包的使用原则** 。当一个Stage有过一次成功构建，即便该Stage下一次构建失败，它之后的Stage仍然可以使用上一个Stage最后一次构建成功的包。

**示例2** ： **特殊流水线** 。系统中有多个流水线，其中发布流水线与其他流水线实现不同，需要单独测试。

**示例3** ： **被忽视的运行方式** 。开发自测时常用手动或者自动模式进行构建，但系统还提供了定时运行方式，需要测试定时运行时多Stage是否构建正常。

**示例4** ： **修改Stage后对构建的影响** 。删除或修改某个Stage信息，是否会对构建产生错误的影响。此外需要查看在"历史页面"模块，是否正确显示对Stage修改的各个历史版本记录。

**示例5** ： **与其他模块的关联** 。只有一个Stage时，报表可以显示构建过程中单元测试结果。变成多个Stage后，如果每个Stage做了不同的单元测试，报表还能正确显示吗？

开发人员自测时候没有跑通所有的业务分支流程，主要原因在于企业开发都是团队协作，一个小组十多个人，每人日常工作中，做的功能卡比较散：今天给页面加滚动条，明天做XML的解析。对功能卡的上下游使用场景难以了解得十分清楚。

这是一个困难点，但换一个思路，将来出现了Bug谁来修？大多数情况还得自己去修复。到那时还是要去熟悉和了解上下游的使用场景，但修复的成本就会变大很多。

开发一个功能时，多花费时间去了解上下游使用的场景，看似多投入的时间，可以有效减少Bug的出现，此外，还可以让自己更了解整个产品的全景图，了解每张卡给用户产生的业务价值。因此，认真分析功能卡的上下游场景，对产品质量和个人成长都很有好处。

#### 2. 不熟悉用户真实使用的场景和方式

一个小例子：开发一个系统对外提供的Web Service服务，当我们把这个接口的使用场景都分析到，才能做到Bug更少。

需要测试的场景包括：

- 用户使用时会输入错误数据，后台要做校验。否则把这脏数据插入数据库中，会给系统带来不可预知的问题。
- 使用这个接口的用户常常需要根据返回的HTTP Code做下一步操作，我们自己需要验证返回的HTTP Code是否都用对了场景。别把该返回401Not Found的场景用了500 HTTP ERROR。
- 接口有在外网使用的场景，需要在HTTP和HTTPS加密的情形下都测试。
- 在外网使用，黑客会制造特殊的字符来做注入攻击，系统对这些特殊字符有没有过滤？
- 服务接口会被多个第三方程序调用，线程安全做好了吗？会不会把A的数据插入到B的名下？
- 服务的响应能力应该怎样？50个并发每秒，还是200个并发每秒？

**做每种软件的测试，都需要仔细分析其使用场景。**

**示例1** ：开发只在Chrome浏览器上进行了自测，而用户使用的是除此之外的其他浏览器。浏览器兼容性的问题非常多。

**示例2** ：用户使用手机软件时，常因坐姿不同而旋转屏幕。自测时就需要关注屏幕旋转对界面布局的影响。

**示例3** ：自测时只在界面上建了3～5个组件，而用户可能会新建N个。在实际使用中发现，新建组件一旦超过30个页面，反应就会明显变慢，存在性能问题。

**示例4** ：一个Web Service服务要被用在公网上，却只在HTTP下进行自测，没有在使用HTTPS加密的场景中进行测试 。

……

#### 3. 对测试数据的边界值选择不够充分

大部分的Bug出现在边界值附近。需要理解业务逻辑，理解输入数据的含义，来确定边界值在哪里。

> 例如，一个医保本允许登记0～10家医院，那0个、1、 9、10、11这些数字都是这个功能的边界值，此外还要随机挑选1～10中的任一数字进行测试。

测试完上面的场景，还要注意的就是：

- 如果这个数字超过数字允许的最大最小值范围，会出现错误吗？
- 超过Integer允许存储的字节长度后，系统会怎么处理？
- 如果超出了数字的范围，使用文本做测试数据，后台有正确处理吗？
- 换成空字符串呢，如果这个空字符串又恰巧是NULL这个程序常用关键字呢？

除此之外，那些特殊字符：引号、星号、问号是否都被正确处理了呢？别小看这些引号、星号等的处理，如果没处理好，可能会抛出HTTP 500 ERROR异常，还可能会暴露后台代码异常堆栈，给黑客提供可循的机会。

对一个英文系统来说，它的文本处理边界也许是所有的英文字符，这时候可以输入中文和德文字符，看会不会造成系统问题。

#### 4. 开发的功能属于页面的一部分时，需要回归测试整个页面

功能开发过程中，可能会破坏以前的功能。最好快速的对页面的其他功能做回归测试，不能只关心当前完成的部分。

#### 5. 不够细心

软件界面上经常看到按钮布局没有对齐、位置差几个像素，或者是页面上的树形结构展开时，垂直虚线中间有中断的现象；或者是字体和系统其他模块使用的字体有差异等。这些可以归到一个字上—— **慌** 。开发通常会得到UI设计稿，而UI设计稿中对字体、高度等都有细到像素级别的描述。页面元素较多，可能是导致开发变慌的原因。

从使用上来说，把自己当做用户，就能轻而易举地发现这些不细心之处。 如果是金融和银行软件，出现此类细微错误，作为用户怕是不敢把钱放进去的。重视细节才能凸显专业。

#### 6. 开发软件前，缺少对同类产品的对比研究。

经过对同类产品的详细对比分析，我们能快速发现自己开发的软件有哪些不足，以及有哪些可以提升的地方。

## 如何让测试变得更有趣，更高效？

#### 1. 不喜欢用文字描述测试用例，此话没毛病

我不喜欢写测试用例，工作中也很少写测试用例。除非是某个功能业务比较复杂，需要用文字帮助梳理和记忆思路的时候，才写一点点测试用例。

测试用例的主要问题在于，用文字描述各种逻辑分支、界面，使用的文字量大，还容易变身老版本，需要维护。好一点的方式是用BDD（我用的是Cucumber+Selenium+Java）去描述测试用例，然后用后台的自动化代码去把测试用例自动化起来，这样就不容易变成老版本了。

#### 2. 喜欢探索性的测试过程

探索性测试第一步，先选择最简单的Happy Path，看系统反馈的直观感受。这个过程中，我注意到界面上的下拉选择框不是通用的HTML控件，如果是自定义的，这时就要担心自定义控件的JavaScript代码是否都能正常工作。

第二步，用这个下拉框选择一个最简单的选项，看选择过程是否流畅（如果不流畅，先记上一笔，可能是JavaScript代码中存在性能问题，待功能测试完成后，再回头看这里的性能）。此外，检查选中后移除选择项的删除等按钮是否正常。最基本的功能测试完了，再选择多个选项，用特殊的数据测试。

如果这个控件所在的页面，有其他HTML控件，当从别的页面跳转到这个页面时，有的控件的值没有预填写，就要测测这个自定义的控件，在同样流程中，能否把值预填写成功。

最后，界面上这个控件宽度被固定，就要考虑如果选择项值是一个150个字符的合法数据，会怎么显示。页面布局是否会被破坏？

就这样，一步一走，通过系统的反馈，点燃新的测试思路，并一步步执行测试。

在探索性测试过程中，最重要的是：测试人员要有能力根据系统给出的反馈，想出更多的测试路径去验证，或者有选择地去重点测试某一个部分。

这种能力是建立在大量的IT认知上的，IT知识越丰富，被系统激发出的思路就越多。这就好比，让一个不了解玉石的人去检验一块玉石的真伪一样，只有具备丰富的玉石知识后，才能练就一双火眼金睛来。

#### 3. 足够了解后台的实现原理

当我们足够了解一个东西时，我们才更清楚它的薄弱环节在哪儿。做过很多开发工作，有很多开发经验的人，拿到一个软件，差不多就知道这个软件的实现过程，以及该软件可能出现问题的位置。

**示例1** ：一个测试Web服务返回HTTP Code的功能卡，因为返回值计算过程简单，出错概率不大。但由于返回有十多种值，需要写很多分支，这种情况程序员往往会漏测一两个分支。

**示例2** ：知道JSON数据的格式，就应联想到把大括号、中括号等作为数据的一部分进行测试。

**示例3** ：有缓存机制，在性能测试时不用同一个关键字进行压力测试，而要改变关键字。

**示例4** ： 如果单元测试用了Mock机制，要清楚Mock往往在系统交接处漏测Bug，需要另外进行端到端的完整流程测试才行。这就要求我们熟知Mock的使用原理。

**示例5** ： 自定义的HTML组件，需要写大量的JavaScript代码，这里容易出错，需要着重测试，而通用的HMTL组件的测试就不用花费过多精力。

**示例6** ： 了解测试环境的差异，比如后台部署多台服务器会带来Session复制分发的问题，这就需要在多台服务器的测试环境中，测试Session复制分发是否正常。

#### 4. 善用各种测试工具

**示例** ：测试发送邮件时，需要验证多种邮件客户端接收后的界面排版是否正常。有这样的工具可以帮助我们只发一封邮件，它给你显示出各种邮件客户端的排版。但是也不能全信它们，使用前最好自己做抽样检查，看看这个工具是否真如它所说的那样好用。

- Charles可以改变发送数据包的内容，帮助我们达到快速测试目的。
- 页面加载的性能测试，使用当地的服务器访问，自动求多次请求后的平均值的工具也能帮我们节省很多时间。
- 优选测试工具，可以让测试工作事半功倍。

#### 5. 关注整个开发流程，有效减少Bug出现

① 在程序员开发一张卡时，首先大家会坐在一起，包括QA、BA、DEV、UI等，讨论这张卡的接收条件，避免一开始写代码就走偏的情况。

② 开发完成后，会在本机给QA演示功能，QA给出反馈，减少Bug出现在部署环境后才被发现的可能。

③ 小组的Code Review可以帮助QA减少测试工作，Code Review能及早发现代码的潜在问题。

关注单元测试覆盖率。单元测试覆盖率不高，QA的工作会越做越累，每次新加的代码都可能破坏之前的功能。

此外，还需要关注UI自动化的运行情况，它可以弥补单元测试没有覆盖到的地方。

#### 6. 对于重复的手工测试，尽量自动化它

自动化的方式包括：

- 单元测试；
- UI自动化测试（需要考虑如何写出高可复用的代码）；
- 接口自动化（例如使用Jmeter的BeanShell生成数据、从CSV或者数据库中读取数据、进行断言等）；
- 自己写程序生成测试数据等。

以上是我的一些拙见，以作引玉之砖，欢迎大家补充和探讨：bug_big_bang_in_web_[testing@outlook.com](mailto:testing@outlook.com)。

本以为日子就在找地鼠和打地鼠的日子中流逝，直到有一天，黄勇问我，要不要一起写书，写关于测试的书。于是有了这本《测试囧事》，也希望本书为大家的开发和测试之路点亮了一排路灯。

[![img](https://testerhome.com/uploads/photo/2017/bc1e6bf4-63ed-4ad0-8c73-a28f765eb1f0.jpg)](https://testerhome.com/uploads/photo/2017/bc1e6bf4-63ed-4ad0-8c73-a28f765eb1f0.jpg)