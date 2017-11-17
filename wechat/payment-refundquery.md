##<span style="color:#2b7db0">查询退款</span>

#### 应用场景

提交退款申请后，通过调用该接口查询退款状态。

退款有一定延时，用零钱支付的退款20分钟内到账，银行卡支付的退款3个工作日后重新查询退款状态。  

注意：如果单个支付订单部分退款次数超过20次请使用退款单号查询  
  
#### 接口地址

api：

```
/wechat/payments/refundquery
```

#### 请求参数
字段|变量名|必填|类型|描述
----|----|----|----|----
接口ID|appid|是|String(32)|由蓝海支付提供的接口应用ID
随机字符串|nonce_str|是|String(32)|随机字符串
签名|sign|是|String(32)|详情见签名规则
商户订单号|out\_trade_no|是|String(32)|商户订单号

#### 返回结果
字段|变量名|必填|类型|描述
----|----|----|----|----
返回码|code|是|String(32)|返回码，请参考返回码表
返回信息|msg|是|String(256)|返回信息，成功信息或错误信息
返回数据|data|否|Array/String|返回数据集或其他提示信息
  
###### 如果code=200,data参数：  
字段|变量名|必填|类型|描述
----|----|----|----|----
商户订单号|out\_trade\_no|是|String(32)|商户订单号
微信订单号|transaction_id|是|String(32)|微信订单号
标价金额|total_fee|是|Int|港币最小单位分
标价币种|fee_type|否|String(32)|HKD
现金支付金额|cash_fee|是|Int|该货币最小币值
现金支付币种|cash\_fee_type|否|String(32)|一般为人民币CNY
退款笔数|refund_count|是|Int|退款笔数
退款金额|refund_fee|是|Int|退款金额
商户退单号|out\_refund_no\_$n|是|String(32)|商户自定义退单号
退款来源|refund\_account\_$n|否|String(32)|REFUND_SOURCE_RECHARGE_FUNDS---可用余额退款/基本账户、REFUND_SOURCE_UNSETTLED_FUNDS---未结算资金退款
退款渠道|refund\_channel_$n|否|String(32)|退回银行、零钱、商户
退款金额|refund\_fee_$n|是|Int|退款金额
微信退单号|refund_id\_$n|是|String(64)|微信退单号
退款入账账户|refund\_recv_accout\_$n|是|String(64)|
退单状态|refund\_status\_$n|是|String(16)|
退单成功时间|refund\_success_time|否|String(20)|例如2017-01-01 08:00:00






