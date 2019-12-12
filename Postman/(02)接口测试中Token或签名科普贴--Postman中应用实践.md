# (02)接口测试中 Token 或签名科普贴--Postman 中应用实践

转自: <<https://testerhome.com/topics/14495>> 

[必读]首先了解下有关Token或签名的几个问题：

1、是什么？

2、干什么的，有什么用？

3、实现原理是什么？

4、Postman中怎么实现？

1、是什么？

Token Wiki解释： An object (in software or in hardware) which represents the right to perform some operation.

Token 百度解释：在计算机身份认证中是令牌（临时）的意思，在词法分析中是标记的意思。

接口请求中的Token或签名，是一串加密后的字符串，作为请求体或请求头中的一个参数放在Request中。

2、有什么用？

请求中带Token或签名，无非就是在服务端验证当前请求是否合法，防止恶意请求或刷单等不良行为。

3、实现原理是什么？

简单理解如下：

请求发出方（Request），通过对特定的字符串进行加密，然后将加密后的字符串放在请求（Request）中，发送给服务端；

服务端收到该请求，同样对特定字符串进行加密，然后将加密后的结果，同request中传过来的参数进行比对，相等则返回正常的Response。

********************************* 分 ************************* 割 ************************* 线 *********************************

4、Postman中怎么实现？

4.1 待加密字符串的组成

这个一般是由请求双方协商好的规则，将请求中的一些参数拼装成一个特定的字符串，然后对特定字符串进行加密。

待加密字符串的组成可能包含Request Header 或 Body 中的一个或多个参数，或者是多次加密。

具体的规则要同自家的开发获取。

4.2 待加密字符串的获取

Postman中如何获取Header、Body中的参数？

在Postman中借www.baidu.com举例

[![img](file:///C:/Users/zhaoq/AppData/Local/Temp/msohtmlclip1/01/clip_image001.png)](https://testerhome.com/uploads/photo/2018/6b4e8f5d-6ecf-446d-b946-2c8a668abd86.png!large)

图一

[![img](file:///C:/Users/zhaoq/AppData/Local/Temp/msohtmlclip1/01/clip_image002.png)](https://testerhome.com/uploads/photo/2018/1907ddfb-6a73-4711-87a9-844ef2267a25.png!large)

图二

[![img](file:///C:/Users/zhaoq/AppData/Local/Temp/msohtmlclip1/01/clip_image003.png)](https://testerhome.com/uploads/photo/2018/c78b9309-bcfa-4355-94bc-218fb4cef16a.png!large)

图三

4.2.1 假设规则

待加密字符串：request headers中的user-agent + 请求方法 + 请求发送时间 + saltBase64。

将request Body中的c参数作为salt，对其进行Base64加密，并拼接到待加密字符串中，即上述saltBase64。

规则：对上述待加密字符串进行MD5加密，生成校验值signTest，将signTest放在request Body的sign参数中。

4.2.2 如何获取参数：

//获取request中headers某个请求参数
 request.headers["headersName"]
 //获取request中body某个请求参数
 console.log("Request Body: " + request.data["a"])
 //获取request方法
 console.log("method: " + request.method)
 //获取request的url
 console.log("url: " + request.url)

下图四对应代码：

console.log("Resquest Headers user-agent: " + request.headers["user-agent"])
 console.log("Resquest Headers: " + request.headers["cache-control"])

[![img](file:///C:/Users/zhaoq/AppData/Local/Temp/msohtmlclip1/01/clip_image004.png)](https://testerhome.com/uploads/photo/2018/2adabb2f-bf9c-45c3-b76b-1a0a51505e93.png!large)

图四

下图五对应代码：

//获取request headers中的 accept-encoding 信息
 console.log("Resquest Headers accept-encoding: " + request.headers["accept-encoding"])
 //获取request的全部cookie信息
 console.log("Resquest Headers cookie: " + request.headers["cookie"])

//获取response headers中的 Date 信息
 console.log("Response Headers Date: " + postman.getResponseHeader("Date"))
 //获取response中的Cookie 中某个key对应的value值
 console.log("Cookie: " + postman.getResponseCookie("PSTM"))

[![img](file:///C:/Users/zhaoq/AppData/Local/Temp/msohtmlclip1/01/clip_image005.png)](https://testerhome.com/uploads/photo/2018/463dc179-5bb8-4f1d-bea1-b35d5adb54eb.png!large)

图五

PS：

以上获取参数的方法有些可以在 Pre-request Script 和 Tests 中使用，有些只能在 Tests 中使用，上述示例代码位置可详见截图。

详情参考Postman[官方文档](https://www.getpostman.com/docs/v6/postman/scripts/postman_sandbox)

4.3 加密代码怎么写：

//获取request headers中的user-agent
 var strUser = request.headers["user-agent"];

//获取request的请求方法
 var strMethod = request.method;

//获取环境变量中的当前时间CurrentTime
 var strTime = globals.CurrentTime;

//将请求Body中的参数c作为salt，并进行Base64加密
 var salt = CryptoJS.enc.Utf8.parse(request.data["c"]);
 var strSalt = CryptoJS.enc.Base64.stringify(salt);

//拼接字符串，并MD5加密
 var strSign = strUser + strMethod + strTime + strSalt;
 var md5Str = CryptoJS.MD5(strSign).toString();

//将加密后的值signTest设置到环境变量
 postman.setEnvironmentVariable("signTest", md5Str);

4.4 写在哪里：

上述生成Token或签名的代码，一般要写在 Pre-request Script 中。

但随着接口增多，每个 Pre-request Script 中写那么多代码又很烦，怎么办呢？

可以将上述代码稍作调整，作为一个变量放在环境变量中，用eval()执行。

在 Pre-request Script 中只需要写一行代码就可以了

eval(globals.keyName);

附录很重要：

1、Postman支持哪些加密算法？

支持的加密算法：AES, DES, EvpKDF, HMAC-MD5, HMAC-SHA1/3/256/512, MD5, PBKDF2, Rabbit, SHA1/3/224/256/512, TripleDES

2、为什么支持这么多，怎么做到的？

因为Postman 引入了 CryptoJS 库，CryptoJS 支持以上多种加密算法，所以 Postman 也支持。

CryptoJS: standard and secure cryptographic algorithms. Supported algorithms: AES, DES, EvpKDF, HMAC-MD5, HMAC-SHA1/3/256/512, MD5, PBKDF2, Rabbit, SHA1/3/224/256/512, TripleDES

引自Postman[官方文档](https://www.getpostman.com/docs/v6/postman/scripts/postman_sandbox)

3、CryptoJS 支持这么多加密算法，怎么实现的？

各位自行深挖。。。

4、Postman支持的其它库

Lodash: JS utility library

cheerio: A fast, lean implementation of the core jQuery API (available in versions 4.6.0 and up)

tv4 JSON schema validator: Validates JSON objects against v4 of the json-schema draft

引自Postman[官方文档](https://www.getpostman.com/docs/v6/postman/scripts/postman_sandbox)

 



 