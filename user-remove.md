## Remove User 删除用户

### api

```
/user/remove
```

### Parameters 请求参数

字段|变量名|必填|类型|描述
----|----|----|----|----
appid|appid|是|String(32)|appid,登录时获取
要删除的用户Id|user_id|否|Int| 可选参数 如果不传递，则删除appid对应的用户
签名|sign|是|String|签名

请求参数示例:

```
{
  "appid":"1000050",
  "user_id":"1000051",
  "sign":"BCA64F4B0A34753497C9DC8943DDFCC9"
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
  "message": "用戶已經刪除"
}
```
