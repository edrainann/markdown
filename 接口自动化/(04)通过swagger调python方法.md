# [原] (04)通过swagger调python方法

### 一、需要安装

```
pip install flask_restplus
```

### 二、构建简单的API

```
from flask import Flask, request
from flask_restplus import Api, Resource

app = Flask(__name__)
api_app = Api(app=app, version='1.0', title='MainTitle', description='Main APIs')
ns = api_app.namespace(name='hello', description='The hello world APIs EndPoint.')



@ns.route('/')
class HelloWorld(Resource):
    def get(self):
        """这是一个get请求"""
        return {'status': 'you get a request.'}

    def post(self):
        return {'status': 'you post a request.'}
  
@ns.route('/get')
class HelloGet(Resource):
    def get(self):
        """访问httpbin的get请求"""
        url = "http://www.httpbin.org/get"
        response = requests.get(url)
        # data = jsonify(response.text)
        data = json.loads(response.text)
        return data


if __name__ == "__main__":
    app.run(debug=True)

```

然后打开http://127.0.0.1:5000，直接就可以看到swagger的页面，点击"Try it out"按钮，就可以看得到想要的页面了。