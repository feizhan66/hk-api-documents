##<span style="color:#2b7db0">退款列表</span>

#### 应用场景


#### 接口地址

api：

```
/wechat/refund/list
```

#### 请求参数

字段|变量名|必填|类型|描述
----|----|----|----|----
接口ID|appid|是|String(32)|由蓝海支付提供的接口应用ID(或通过登录接口获取)
签名|sign|是|String(32)|详情见签名规则
分页参数|page|否|Int|分页参数:第几页 默认值:1
每页记录数|limit|否|Int|每页条目数 默认值:10
门店Id|store_id|否|Int|门店Id, 登录接口获取的store_id > 0 时填写
退款状态|refund_status|否|String|退款状态 如 SUCCESS
开始时间|start_time|否|String|按照退款时间查询订单 如 2017-12-12
结束时间|end_time|否|String|按照退款时间查询订单 如2017-12-13

#### trade_state
```
SUCCESS:退款成功
PROCESSING:退款处理中
REFUNDCLOSE:退款关闭
CHANGE:退款异常
```

#### 返回结果

字段|变量名|必填|类型|描述
----|----|----|----|----
返回码|code|是|String(32)|返回码，请参考返回码表
返回信息|message|是|String(256)|返回信息，成功信息或错误信息
返回数据|data|否|Array|返回数据集或其他提示信息



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
    "id": "104",
    "blue_mch_id": "100018",
    "store_id": "0",
    "out_trade_no": "201712180934024536697669",
    "transaction_id": "40088820010029888480",
    "out_refund_no": "201712180705068744292344",
    "refund_id": "201712180705068744292344",
    "total_fee": 80.65,
    "refund_fee": 80.65,
    "fee_type": "HKD",
    "refund_status": "SUCCESS",
    "refund_desc": "",
    "refund_time": "2017-12-18 07:05:06"
},
  {
    "id": "100",
    "blue_mch_id": "100018",
    "store_id": "0",
    "out_trade_no": "20171215101436282896",
    "transaction_id": "4200000020201712154360049510",
    "out_refund_no": "20171215101436282896",
    "refund_id": "50000605352017121502629376350",
    "total_fee": 0.02,
    "refund_fee": 0.02,
    "fee_type": "HKD",
    "refund_status": "SUCCESS",
    "refund_desc": "",
    "refund_time": "2017-12-15 10:15:10"
}
],
}
}
```
