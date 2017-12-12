##<span style="color:#2b7db0">订单列表</span>

#### 应用场景


#### 接口地址

api：

```
/wechat/order/list
```

#### 请求参数

字段|变量名|必填|类型|描述
----|----|----|----|----
接口ID|appid|是|String(32)|由蓝海支付提供的接口应用ID
门店ID|store_id|否|Int|登录获取的门店id
签名|sign|是|String(32)|详情见签名规则
分页参数|page|否|Int|分页参数:第几页 默认值:1
每页记录数|limit|否|Int|每页条目数 默认值:10

#### 返回结果

字段|变量名|必填|类型|描述
----|----|----|----|----
返回码|code|是|String(32)|返回码，请参考返回码表
返回信息|message|是|String(256)|返回信息，成功信息或错误信息
返回数据|data|否|Array/String|返回数据集或其他提示信息

字段|变量名|必填|类型|描述
----|----|----|----|----
返回状态|code|是|Int|
返回状态描述|message|是|String(256)|
返回数据|data|否|array


## 正确返回结果
```
{
"code": 200,
"message": "success",
"data": {
"page": 1,
"limit": 2,
"items": [
  {
"id": "59177",
"store_id": "0",
"body": "make data",
"out_trade_no": "201712072352183584185113",
"transaction_id": "40088820013659564747",
"total_fee": 602.14,
"trade_state": "SUCCESS",
"fee_type": "HKD",
"create_time": "1512661938",
"finish_time": "1512661938"
},
  {
"id": "59183",
"store_id": "0",
"body": "make data",
"out_trade_no": "201712072317240757116534",
"transaction_id": "40088820018344204968",
"total_fee": 381.78,
"trade_state": "SUCCESS",
"fee_type": "HKD",
"create_time": "1512659844",
"finish_time": "1512659844"
}
],
}
}

```