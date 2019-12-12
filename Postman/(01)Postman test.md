# Postman Test scripts使用方法

测试脚本是在发送请求之后并且从服务器接收到响应时执行。

```
设置环境变量
pm.environment.set("variable_key", "variable_value");

将一个嵌套的对象设置为一个环境变量
var array = [1, 2, 3, 4];
pm.environment.set("array", JSON.stringify(array, null, 2));

var obj = { a: [1, 2, 3, 4], b: { c: 'val' } };
pm.environment.set("obj", JSON.stringify(obj))

获得一个环境变量
pm.environment.get("variable_key");

获得一个环境变量(其值是一个字符串化的对象)
// These statements should be wrapped in a try-catch block if the data is coming from an unknown source.
var array = JSON.parse(pm.environment.get("array"));
var obj = JSON.parse(pm.environment.get("obj"));

清除一个环境变量
pm.environment.unset("variable_key");

设置一个全局变量
pm.globals.set("variable_key", "variable_value");

获取一个全局变量
pm.globals.get("variable_key");

清除一个全局变量
pm.globals.unset("variable_key");

获取一个变量
该函数在全局变量和活动环境中搜索变量
pm.variables.get("variable_key");

检查响应主体是否包含字符串
pm.test("Body matches string", function () {
    pm.expect(pm.response.text()).to.include("string_you_want_to_search");
});

检查响应体是否等于字符串
pm.test("Body is correct", function () {
    pm.response.to.have.body("response_body_string");
});

检查JSON值
pm.test("Your test name", function () {
    var jsonData = pm.response.json();
    pm.expect(jsonData.value).to.eql(100);
});

Content-Type 存在
pm.test("Content-Type is present", function () {
    pm.response.to.have.header("Content-Type");
});


返回时间少于200ms
pm.test("Response time is less than 200ms", function () {
    pm.expect(pm.response.responseTime).to.be.below(200);
});

状态码是200
pm.test("Status code is 200", function () {
    pm.response.to.have.status(200);
});

代码名包含一个字符串
pm.test("Status code name has string", function () {
    pm.response.to.have.status("Created");
});

成功的POST请求状态码
pm.test("Successful POST request", function () {
    pm.expect(pm.response.code).to.be.oneOf([201,202]);
});

为JSON数据使用TinyValidator
var schema = {
 "items": {
 "type": "boolean"
 }
};
var data1 = [true, false];
var data2 = [true, 123];

pm.test('Schema is valid', function() {
  pm.expect(tv4.validate(data1, schema)).to.be.true;
  pm.expect(tv4.validate(data2, schema)).to.be.true;
});

解码base64编码数据
var intermediate,
    base64Content, // assume this has a base64 encoded value
    rawContent = base64Content.slice('data:application/octet-stream;base64,'.length);

intermediate = CryptoJS.enc.Base64.parse(base64content); // CryptoJS is an inbuilt object, documented here: https://www.npmjs.com/package/crypto-js
pm.test('Contents are valid', function() {
  pm.expect(CryptoJS.enc.Utf8.stringify(intermediate)).to.be.true; // a check for non-emptiness
});

发送异步请求
此函数可作为预请求和测试脚本使用
pm.sendRequest("https://postman-echo.com/get", function (err, response) {
    console.log(response.json());
});

将XML主体转换为JSON对象
var jsonObject = xml2Json(responseBody);
```

