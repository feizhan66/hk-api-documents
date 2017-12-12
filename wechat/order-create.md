##<span style="color:#2b7db0">统一下单接口</span>

#### 应用场景

除被扫支付场景以外，商户系统先调用该接口在蓝海支付服务后台生成预支付交易单，

返回正确的预支付交易回话标识后再按扫码、JSAPI等不同场景生成交易串调起支付。


#### 接口地址

api：

```
/wechat/order/create
```

#### 请求参数

字段|变量名|必填|类型|描述
----|----|----|----|----
接口ID|appid|是|String(32)|由蓝海支付提供的接口应用ID
随机字符串|nonce_str|是|String(32)|随机字符串
签名|sign|是|String(32)|详情见签名规则
交易类型|trade_type|是|String(16)|交易类型为 JSAPI或NATIVE
公众号ID|mp_appid|否|String(32)|如果需要在支付成功之后获取用户的openid这里为必填
设备号|device_info|否|String(32)|终端设备号
商品描述|body|是|String(128)|商品或支付单简要描述
商品详情|detail|否|String(8192)|商品详细列表，使用Json格式，格式请参考【商品详情格式】
附加数据|attach|否|String(127)|附加数据，在查询API和支付通知中原样返回，该字段主要用于商户携带订单的自定义数据
商户订单号|out\_trade_no|是|String(32)|商户系统自定义订单号
标价金额|total_fee|是|Int|此处单位为港币最小单位分
终端IP|spbill\_create\_ip|是|String(16)|提交用户端ip
交易结束时间|time_expire|否|String(14)|时间格式为YYYYMMDDHHmmss  20170101080000
订单优惠标记|goods_tag|否|String(32)|商品标记，代金券或立减优惠功能的参数
通知地址|notify_url|否|String(256)|接收微信支付异步通知回调地址
商品ID|product_id|否|String(32)|
用户标识|openid|否|String(128)|交易类型为JSAPI时，本项必填


#### 返回结果
字段|变量名|必填|类型|描述
----|----|----|----|----
返回码|code|是|String(32)|返回码，请参考返回码表
返回信息|message|是|String(256)|返回信息，成功信息或错误信息
返回数据|data|否|Array/String|返回数据集或其他提示信息
  
###### 如果code=200,data参数：  

字段|变量名|必填|类型|描述
----|----|----|----|----
交易类型|trade_type|是|String(16)|调用接口提交的交易类型，取值如下：JSAPI，NATIVE
预支付交易会话标识|prepay_id|是|String(64)|微信生成的预支付会话标识，用于后续接口调用中使用，该值有效期为2小时
二维码链接|code_url|是|String(64)|trade_type为NATIVE是有返回，可将该参数值生成二维码展示出来进行扫码支付