## Setting Password 设置密码

### api

```
/user/password
```

### Parameters 请求参数

字段|变量名|必填|类型|描述
----|----|----|----|----
appid|appid|是|String(32)|appid,登录时获取
email|帐号|是|String(32)|登录邮箱帐号
old|原始密码|是|String(32)|密码
new|新密码|是|String(32)|
repeat|重复密码|是|String(32)|
sign|签名|是|String(32)|


请求参数示例:

```
{
  "old": "123456",
  "new": "654321",
  "repeat": "654321",
  "sign": "355954524EA6FF44D8582DC50BECF85F"
}
```


### Response 响应示例

#### 修改成功示例

```
{
  "code": 200,
  "message": "success"
}

```

#### 修改失败示例

```
{
  "code": 0,
  "message": "两次密码错误"
}
```
