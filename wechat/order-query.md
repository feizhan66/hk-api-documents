##<span style="color:#2b7db0">查询订单</span>

#### 应用场景

该接口提供所有微信支付订单的查询，商户可以通过该接口主动查询订单状态，完成下一步的业务逻辑。 
 
需要调用查询接口的情况：  

1. 当商户后台、网络、服务器等出现异常，商户系统最终未接收到支付通知；  

2. 调用支付接口后，返回系统错误或未知交易状态情况；  

3. 调用被扫支付API，返回USERPAYING的状态；  

4. 调用关单或撤销接口API之前，需确认支付状态；  

#### 接口地址

api：

```
/wechat/order/query
```

#### 请求参数

字段|变量名|必填|类型|描述
----|----|----|----|----
接口ID|appid|是|String(32)|由蓝海支付提供的接口应用ID
随机字符串|nonce_str|是|String(32)|随机字符串
签名|sign|是|String(32)|详情见签名规则
微信订单号|transaction_id|是|String(32)|微信的订单号，优先使用
商户订单号|out\_trade_no|否|String(32)|微信订单号、商户订单号二选一

#### 返回结果

字段|变量名|必填|类型|描述
----|----|----|----|----
返回码|code|是|String(32)|返回码，请参考返回码表
返回信息|message|是|String(256)|返回信息，成功信息或错误信息
返回数据|data|否|Array/String|返回数据集或其他提示信息
  
###### 如果code=200,data.trade_state=NOTPAY或USERPAYING时：  

字段|变量名|必填|类型|描述
----|----|----|----|----
交易状态|trade_state|是|String(32)|SUCCESS、REFUND等详情参考附表
交易状态描述|trade\_state\_desc|是|String(256)|交易状态描述

###### 如果code=200,data参数：  

字段|变量名|必填|类型|描述
----|----|----|----|----
接口ID|appid|是|String(32)|由蓝海支付提供的接口应用ID
随机字符串|nonce_str|是|String(32)|随机字符串
签名|sign|是|String(32)|详情见签名规则
交易状态|trade_state|是|String(32)|SUCCESS、REFUND等详情参考附表
交易类型|trade_type|是|String(16)|交易类型为 JSAPI或NATIVE
付款银行|bank_type|是|String(16)|银行类型
现金支付金额|cash_fee|是|Int|订单现金支付金额
现金支付币种|cash\_fee_type|是|String(16)|现金支付的币种，默认为人民币
商户订单号|out\_trade\_no|是|String(32)|商户系统自定义的订单号
微信支付订单号|transaction_id|是|String(32)|微信支付订单号
标价币种|fee_type|否|String(16)|标价币种
标价金额|total_fee|是|Int|标价金额
支付完成时间|time_end|是|String(14)|支付完成时间时间格式为YYYY-MM-DD HH:mm:ss
用户标识|openid|否|String(128)|用户标识
是否关注公众号|is_subscribe|否|String(1)|是否关注了公众号
设备号|device_info|否|String(32)|设备号
商家数据包|attach|否|String(128)|商家数据包，原样返回