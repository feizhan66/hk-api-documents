## Use Login 用户登录

### api

```
/user/login
```

### Parameters 请求参数

字段|变量名|必填|类型|描述
----|----|----|----|----
email|帐号|是|String(32)|登录邮箱帐号
password|password|是|String(32)|密码

#### 使用 POST 并将请求的数据参数的json字符串以 http body的方式传递

#### 该方法无需签名，可以在chrome 浏览器插件 Advanced REST client 中测试

请求参数示例:

```
{"email":"abc@blueoceanpay.com","password":"123456"}
```
该帐号不存在，实际测试中，我们会提供相应的测试帐号

### Response 响应示例

#### 商户帐号登录正确返回示例

```
{
  "code": 200,
  "message": "success",
  "data": {
    "appid": 100018,
    "app_key": "TVqec7ZcuG59HepqjQbTi68S78sdLvUh",
    "name": "御龍集團",
    "store_id": 0,
    "store_name": ""
  }
}

```

#### 门店帐号登录正确返回示例

```
{
  "code": 200,
  "message": "success",
  "data": {
    "appid": 100018,
    "app_key": "TVqec7ZcuG59HepqjQbTi68S78sdLvUh",
    "name": "御龍集團",
    "store_id": 100011,
    "store_name": "中环店"
  }
}
```

### 错误返回示例
