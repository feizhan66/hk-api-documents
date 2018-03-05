# BlueOcean Pay Api Document

## 1 接口规则

### 1.1 接口协议

#### 调用BlueOcean Pay接口须准守以下规则：

1. 请求方式一律使用 POST 并将请求的数据参数的json字符串以 http body的方式传递

2. 请求的数据格式统一使用JSON格式，若参数有json字符串请转义双引号（\"）

3. 字符串编码请统一使用UTF-8

4. 签名算法MD5
  
 
### 1.2 参数签名

1.2.1. 假设请求参数如下：

```
{
    "appid":"1000010",
    "name":"BlueOcean Pay",
    "region":"HK",
    "business":"Online payment"
}
```

1.2.2. 将参数按照键值对（key=value）的形式排列,按照参数名ASCII字典序排序,并用&连接

```
str = "appid=1000010&business=Online payment&name=BlueOcean Pay&region=HK"
```

1.2.3. 再最后拼接上密钥字符串&key=sxkj0RH9qMxdaxo0sJ8xlbki4ssOjvXb

```
str += "&key=sxkj0RH9qMxdaxo0sJ8xlbki4ssOjvXb"
```
即 str 为:

```
appid=1000010&business=Online payment&name=BlueOcean Pay&region=HK&key=sxkj0RH9qMxdaxo0sJ8xlbki4ssOjvXb
```

   
1.2.4. 最后MD5签名,字符串转换成大写

```
sign = strtoupper(md5(str))
```

sign的值为:

```
08C612FB4D2D52C8C913EA00E3DABC8B
```

  
### 1.3 PHP示例代码：


```
<?php
/**
 * 参数数组数据签名
 * @param array $data 参数
 * @param string $key 密钥
 * @return string 签名
 */
function signData($data,$key){	
	$ignoreKeys = ['sign', 'key'];
    ksort($data);
    $signString = '';
    foreach ($data as $k => $v) {
        if (in_array($k, $ignoreKeys)) {
            unset($data[$k]);
            continue;
        }
        $signString .= "{$k}={$v}&";
    }
    $signString .= "key={$key}";
	return strtoupper(md5($signString));
}
```


### 1.4 接口域名

```
HongKong
https://api.hk.blueoceanpay.com

Australia
https://api.au.blueoceanpay.com

United States
https://api.us.blueoceanpay.com

United Kingdom
https://api.uk.blueoceanpay.com

```

### 1.5 响应错误码

返回码|提示信息|原因|解决方案
----|----|----|----
200|成功|正常|按照具体业务逻辑处理
201|刷卡结果未知状态|系统繁忙、用户支付中等原因|等待15s通过【查询订单API】查询
404|找不到该接口|接口地址错误|根据文档检查接口地址
|||
4001|数据格式错误|数据格式不符|请使用JSON数据格式
4002|无效APPID|APPID不存在|检查APPID是否错误
4003|缺少参数|缺少参数|按照提示补全参数
4004|签名错误|签名验证错误|检查代码签名逻辑
4005|通讯出错|微信服务器错误|按提示排查代码错误之后，向蓝海技术人员反馈协助您解决问题
4006|服务器繁忙|服务器繁忙|稍后再重新调用该接口
4007|请求方式错误|请求方式错误|请使用POST方式
|||
40100|无效请求||根据提示处理
40101|订单已支付||按照具体业务逻辑处理
40102|订单不存在||按照具体业务逻辑处理
40103|订单已关闭||按照具体业务逻辑处理
40104|订单已撤销||按照具体业务逻辑处理
40105|订单已退款||按照具体业务逻辑处理
40106|订单号重复||商户重新生成订单号
40107|余额不足||检查商户账户余额是否不足
40108|订单超过退款期限||无法完成退款
40109|缺少参数||按照提示补全参数
40110|编码格式错误||请使用UTF8编码格式
40111|每个二维码只可以用一次||重新获取用户支付二维码
|||
40400|系统繁忙|系统繁忙|系统繁忙，稍后重试
40500|微信错误||向蓝海技术人员反馈协助您解决问题
40600|未知错误|(未收录错误)|向蓝海技术人员反馈协助您解决问题




## 2 接口列表

### 2.1 统一支付

Pos机根据使用场景，提交相应请求参数，完成相应支付业务

### Api: 

```
/payment/pay
```
### Parameters 请求参数

字段|变量名|必填|类型|描述
----|----|----|----|----
appid|appid|是|String|appid,由商户后台获取，或者登录获取
支付方式|payment|是|String|micropay,alipay.qrcode,wechat.qrcode
订单金额|total_fee|是|Int|支付金额 单位为"分" 如10即0.10元
优惠金额|discount|否|Int|优惠金额 单位"分" 如25即0.25元,默认为0
支付授权码|code|可选|String|payment为"micropay"时填写 如支付宝 288271620985824610 微信 134519771100657507 服务端据此参数值区分
商户自身订单号|out\_trade\_no|可选|String|如果商户有自己的订单系统，可以自己生成订单号，否则建议交由蓝海支付后台自动生成
签名|sign|是|String参考签名方法


#### payment 参数说明

参数值|描述
----------|----------
micropay|刷卡支付 此时需传递支付授权码 `code` 参数
alipay.qrcode|支付宝二维码
wechat.qrcode|微信二维码

支付返回后，检查交易状态trade_state,并根据其结果，决定是否调用订单查询接口进行结果查询处理

### trade_state

```
NOTPAY:未支付
SUCCESS:支付成功
REFUND:已退款
CLOSED:已关闭
REVOKED:已撤销
USERPAYING:支付中
PAYERROR:支付异常
```

### 正确响应数据说明

响应结果response.data数据说明

字段|变量名|类型|描述
----|----|---|----
订单id|id|Int|如:10357
蓝海订单编号|sn|String|示例: 11201802091347484054542598 唯一标号，可以用于查询，关闭，退款等操作
货币|fee_type|String|交易货币 如 HKD,AUD
商户名称|mch_name|String|如 "BlueOcean Pay"
商户Id|out\_trade\_no|String|商户订单号 如: "11201802091347484054542598"
支付方交易号|transaction_id|String| P5631VZG299QZN94JD
支付提供方|provider|String|如:alipay,wechat
订单时间|create_time|Int|时间戳 如: 1518155270
支付时间|time_end|Int|成功支付的时间戳 如1518155297
交易状态|trade_state|String| NOTPAY
二维码文本|qrcode|String|扫码支付时存在，客户端使用第三方工具，将该内容生成二维码，供用户扫描付款 如 "https://qr.alipay.com/bax03112k12liy7lrysg2004", "weixin://wxpay/bizpayurl?pr=HBXdDeM"
订单总金额|total_fee|Int|如:10
优惠金额|discount|Int|如:2
实际支付金额|pay_amount|Int|(total_fee-discount) 如:8
数据签名|sign|String|如"7FB42F08C85670A86431F97109DE8683",用于本地校验



### 请求示例

#### 支付宝二维码示例:

请求参数

```
{
  "appid": "1000322",
  "payment": "alipay.qrcode",
  "total_fee": 10,
  "wallet": "CN",
  "sign": "520489313B46B5D403CCD8A01267B6C7"
}
```

响应结果

```
{
  "code": 200,
  "message": "success",
  "data": {
    "appid": "1000258",
    "attach": "",
    "bank_type": "",
    "body": "",
    "cash_fee": "0",
    "cash_fee_type": "",
    "create_time": 1518155270,
    "detail": "",    
    "fee_type": "HKD",
    "id": "13152",
    "is_subscribe": "N",
    "mch_name": "BlueOcean Pay",
    "nonce_str": "JnFOwTyJLm",
    "out_trade_no": "11201802091347484054542598",
    "total_fee": 10,
    "discount": 0,
    "pay_amount": 10,
    "provider": "alipay",
    "qrcode": "https://qr.alipay.com/bax03112k12liy7lrysg2004",
    "sn": "11201802091347484054542598",
    "time_end": 0,    
    "trade_state": "NOTPAY",
    "trade_type": "NATIVE",
    "transaction_id": "P5631VZG299QZN94JD",
    "sign": "7FB42F08C85670A86431F97109DE8683"
  }
}
```

#### 刷卡支付示例 

请求参数

```
{
  "appid": "1000322",
  "code": "134602370743606195",
  "payment": "micropay",
  "total_fee": 5,
  "sign": "5D8883E85FB4D721A0CF53BBD0A12905"
}
``` 

响应

```
{
  "code": 200,
  "message": "OK",
  "data": {
    "appid": "1000258",
    "attach": "",
    "bank_type": "",
    "body": "Shopping",
    "cash_fee": "0",
    "cash_fee_type": "",
    "create_time": 1517996545,
    "detail": "",
    "fee_type": "HKD",
    "id": "11763",
    "is_subscribe": "N",
    "mch_name": "BlueOcean Pay",
    "nonce_str": "5L0CLFvTA1",
    "out_trade_no": "11201802071742242458779416",
    "provider": "wechat",
    "qrcode": "",
    "sn": "11201802071742242458779416",
    "time_end": 0,
    "total_fee": 5,
    "discount":2,
    "pay_amount":3,
    "trade_state": "NOTPAY",
    "trade_type": "MICROPAY",
    "transaction_id": "",
    "sign": "9E93F481EBD5E06081882E7CFAE2E0FF"
  }
}
```


### 2.2 退款

当交易发生之后一段时间内，由于买家或者卖家的原因需要退款时，卖家可以通过退款接口将支付款退还给支付用户，

支付提供方(支付宝，微信等)将在收到退款请求并且验证成功之后，按照退款规则将支付款按原路退还支付用户帐号。

#### Api: 

```
/payment/refund
```
#### Parameters 请求参数

字段|变量名|必填|类型|描述
----|----|----|----|----
appid|appid|是|String|appid,登录时获取
订单编号|sn|否|String|与out\_trade\_no二选一,优先使用sn
商户订单号|out\_trade\_no|否|String|商户订单号
退款金额|refund_fee|否|Int|可选参数，默认为订单总额，即全额退款 支付宝不支持部分退款
退款描述|refund_desc|否|String|退款说明
退款密码|password|是|String|退款密码
签名|sign|是|String|

### 2.3 订单操作

订单状态操作(查询，关闭，撤销)，使用类似的参数

### Api: 

```
查询
/order/query
关闭
/order/close
撤销
/order/reverse
```
### Parameters 请求参数

字段|变量名|必填|类型|描述
----|----|----|----|----
appid|appid|是|String|appid,登录时获取
订单编号|sn|否|String|与out\_trade\_no二选一,优先使用sn
商户订单号|out\_trade\_no|否|String|商户订单号
签名|sign|是|String|

请求示例

查询

```
{
  "appid": "1000322",
  "sn": "11201802071854269363947431",
  "sign": "9CC7EB3EBE33D11421FE63298F2EEB94"
}

```

结果

```
{
  "code": 200,
  "message": "success",
  "data": {
    "appid": "1000258",
    "attach": "",
    "bank_type": "",
    "body": "Shopping",
    "cash_fee": "0",
    "cash_fee_type": "",
    "create_time": 1518000867,
    "detail": "",
    "fee_type": "HKD",
    "id": "11767",
    "is_subscribe": "N",
    "mch_name": "BlueOcean Pay",
    "nonce_str": "Wu2vrRowHs",
    "out_trade_no": "11201802071854269363947431",
    "provider": "wechat",
    "qrcode": "weixin://wxpay/bizpayurl?pr=wDmjTFq",
    "sn": "11201802071854269363947431",
    "time_end": 0,
    "total_fee": 10,
    "discount":0,
    "pay_amount":10,
    "trade_state": "CLOSED",
    "trade_type": "NATIVE",
    "transaction_id": "",
    "sign": "C15B790CBCFE8268212D08A9F2AFA55D"
  }
}
```


### 2.4 订单列表

订单列表

### Api: 

```
/order/list
```
### Parameters 请求参数

字段|变量名|必填|类型|描述
----|----|----|----|----
接口ID|appid|是|String|由蓝海支付提供的接口应用ID(或通过登录接口获取)
支付服务提供方|provider|否|String|默认查询所有 alipay,wechat
分页参数|page|否|Int|分页参数:第几页 默认值:1
每页记录数|limit|否|Int|每页条目数 默认值:10
门店Id|store_id|否|Int|门店Id, 登录接口获取的store_id > 0 时填写
交易状态|trade_state|否|String|交易状态 如 SUCCESS,REFUND
开始时间|start_time|否|String|按照创建时间查询订单 如 2017-12-12
结束时间|end_time|否|String|按照创建时间查询订单 如2017-12-13
签名|sign|是|String|详情见签名规则

#### 返回结果

字段|变量名|必填|类型|描述
----|----|----|----|----
返回码|code|是|String|返回码，请参考返回码表
返回信息|message|是|String|返回信息，成功信息或错误信息
返回数据|data|否|Array|返回数据集

#### 请求参数示例

```
{
    "appid": "1000322",
    "limit": 10,
    "page": 1,
    "store_id": 1000321,
    "sign": "55F40D7150AD3013E4AB479745FAE542"
}
```

### 返回结果示例

```
{
  "code": 200,
  "message": "success",
  "data": {
    "page": 1,
    "limit": 10,
    "total": 43,
    "items": [
      {
        "id": "11316",
        "sn": "11201802061441100008508698",
        "provider": "alipay",
        "blue_mch_id": "1000258",
        "store_id": "1000321",
        "body": "",
        "out_trade_no": "11201802061441100008508698",
        "transaction_id": "3PG7YQKYM8J5KDXJRV",
        "total_fee": 10,
        "discount":2,
        "pay_amount":8,
        "trade_state": "NOTPAY",
        "trade_type": "QRCODE",
        "fee_type": "HKD",
        "create_time": 1517899273,
        "time_end": 0
      },
      {
        "id": "11315",
        "sn": "11201802061439532332362699",
        "provider": "wechat",
        "blue_mch_id": "1000258",
        "store_id": "1000321",
        "body": "BlueOceanPay",
        "out_trade_no": "20180206143947260296",
        "transaction_id": "4200000062201802068214812373",
        "total_fee": 2,
        "discount":0,
        "pay_amount":2,
        "trade_state": "SUCCESS",
        "trade_type": "MICROPAY",
        "fee_type": "HKD",
        "create_time": 1517899193,
        "time_end": 1517899193
      }
    ]
  }
}
```


### 2.5 退款列表

api：

```
/refund/list
```

#### 请求参数

字段|变量名|必填|类型|描述
----|----|----|----|----
接口ID|appid|是|String|
签名|sign|是|String|详情见签名规则
分页参数|page|否|Int|分页参数:第几页 默认值:1
每页记录数|limit|否|Int|每页条目数 默认值:10
支付提供方|provider|否|String|alipay,wechat
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


请求参数
```
{
  "appid": "1000322",
  "limit": 2,
  "page": 1,
  "sign": "74CD2AAB31B3FB76A68FEBF8FC56AF70"
}
```

响应结果

```
{
  "code": 200,
  "message": "success",
  "data": {
    "page": 1,
    "limit": 2,
    "total": 88,
    "items": [
      {
        "id": "12662",
        "provider": "wechat",
        "blue_mch_id": "1000258",
        "store_id": "0",
        "out_trade_no": "a257f51afe1694b20180208164501288",
        "transaction_id": "4200000060201802089377289242",
        "out_refund_no": "a8d95666c735f412018020816452292",
        "refund_id": "50000205722018020803496119655",
        "total_fee": "2",
        "refund_fee": "2",
        "discount": "0",
        "fee_type": "HKD",
        "refund_status": "SUCCESS",
        "refund_desc": "",
        "refund_time": 1518079526,
        "pay_amount": 2
      },
      {
        "id": "12647",
        "provider": "alipay",
        "blue_mch_id": "1000258",
        "store_id": "1000321",
        "out_trade_no": "11201802081148468407623096",
        "transaction_id": "MJ86QYO39MXWOEP3RG",
        "out_refund_no": "11201802081449331411459094",
        "refund_id": "",
        "total_fee": "10",
        "refund_fee": "10",
        "discount": "0",
        "fee_type": "HKD",
        "refund_status": "SUCCESS",
        "refund_desc": "",
        "refund_time": 1518072577,
        "pay_amount": 10
      }
    ]
  }
}
```


### 2.6 用户登录

### api

```
/user/login
```

### Parameters 请求参数

字段|变量名|必填|类型|描述
----|----|----|----|----
email|帐号|是|String(32)|登录帐号
password|password|是|String(32)|密码

#### 使用 POST 并将请求的数据参数的json字符串以 http body的方式传递

#### 该方法无需签名，可以在chrome 浏览器插件 Advanced REST client 中测试

请求参数示例:

```
{"email":"abc@blueoceanpay.com","password":"123456"}
```
该帐号不存在，实际使用中，请由商户后台获取。

### Response 响应示例

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
    "type":"store",//登录的帐号类型: merchant -> 商户, store -> 门店, cashier -> 收银
    "store_id": 100011,
    "store_name": "中环店"
  }
}
```


### Update

2018.03.05












