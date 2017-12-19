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
接口ID|appid|是|String(32)|由蓝海支付提供的接口应用ID(或通过登录接口获取)
签名|sign|是|String(32)|详情见签名规则
分页参数|page|否|Int|分页参数:第几页 默认值:1
每页记录数|limit|否|Int|每页条目数 默认值:10
门店Id|store_id|否|Int|门店Id, 登录接口获取的store_id > 0 时填写
交易状态|trade_state|否|String|交易状态 如 SUCCESS,REFUND
开始时间|start_time|否|String|按照创建时间查询订单 如 2017-12-12
结束时间|end_time|否|String|按照创建时间查询订单 如2017-12-13

#### trade_state
```
NOTPAY:未支付
SUCCESS:支付成功
REFUND:已退款
CLOSED:已关闭
REVOKED:已撤销
USERPAYING:支付中
PAYERROR:支付异常
```

#### 返回结果

字段|变量名|必填|类型|描述
----|----|----|----|----
返回码|code|是|String(32)|返回码，请参考返回码表
返回信息|message|是|String(256)|返回信息，成功信息或错误信息
返回数据|data|否|Array|返回数据集



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
