## User List 用户列表

### api

```
/user/list
```

### Parameters 请求参数

字段|变量名|必填|类型|描述
----|----|----|----|----
appid|appid|是|String(32)|appid,登录时获取
分页参数|page|否|Int|分页参数:第几页 默认值:1
每页记录数|limit|否|Int|每页条目数 默认值:10
用户类型|type|否|String|默认值: cashier(收银员) 目前设计 只有store类型的用户能够查看同店铺的收银员并对其进行相应操作
签名|sign|是|String|签名

请求参数示例:

```
{
  "appid":"83",
  "sign":"BCA64F4B0A34753497C9DC8943DDFAED"
}
```

### Response 响应示例

#### 获取成功示例

```
{
	"code": 200,
	"message": "success",
	"data": {
		"page": 1,
		"limit": 10,
		"total": 3,
		"items": [
			{
				"id": "1000112",
				"username": "qqqq.com",
				"name": "qqqq.com",
				"type": "cashier",
				"depart_id": "100002",
				"store_id": "4",
				"created_at": "2017-12-25 14:51:49",
				"updated_at": "2017-12-25 15:27:27"
			}
		],
	}
}

```
