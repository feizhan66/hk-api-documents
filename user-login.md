## Use Login 用户登录

### api

```
/user/login
```

### Parameters

### Response 

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
