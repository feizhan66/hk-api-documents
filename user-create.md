## Create User 添加用户

### api

```
/user/create
```

### Parameters 请求参数

字段|变量名|必填|类型|描述
----|----|----|----|----
appid|appid|是|String(32)|appid,登录时获取
email|帐号|是|String(32)|登录帐号
password|密码|是|String(32)|
type|帐号类型|是|String(32)| merchant -> 商户 , store -> 门店, cashier -> 收银
sign|签名|是|String(32)| 


请求参数示例:

```
{
  "appid": "100002",
  "email": "test@blueoceanpay.com",
  "password":"123456",
  "type": "store",  
  "sign": "C770EDDEC3F173CE69B5A46D7A009A59"
}
```


### Response 响应示例

#### 成功示例

```
{
  "code": 200,
  "message": "success",
  "data":{
    "id": "1000047",
    "username": "test@blueoceanpay.com",
    "type": "store",
    ...
  }
}

```

#### 失败示例

```
{
  "code": 0,
  "message": "帐号已存在"
}
```
