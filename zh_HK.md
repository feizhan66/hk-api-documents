# BlueOcean Pay Api Document

## 1 接口規則

### 1.1 接口協議

#### 調用BlueOcean Pay接口須遵守以下規則：

1. 請求方式一律使用 POST 並將請求的數據參數的JSON字符串以 http body的方式傳遞

2. 請求的數據格式統一使用JSON格式，若參數有json字符串請轉義雙引號（\"）

3. 字符串編碼請統一使用UTF-8

4. 簽名算法MD5
  
 
### 1.2 參數簽名

1.2.1. 假設請求參數如下：

```
{
    "appid":"1000010",
    "name":"BlueOcean Pay",
    "region":"HK",
    "business":"Online payment"
}
```

1.2.2. 將參數按照鍵值對（key=value）的形式排列,按照參數名ASCII字典序排序,並用&連接

```
str = "appid=1000010&business=Online payment&name=BlueOcean Pay&region=HK"
```

1.2.3. 在最後拼接上密鑰字符串 `&key=sxkj0RH9qMxdaxo0sJ8xlbki4ssOjvXb`

```
str += "&key=sxkj0RH9qMxdaxo0sJ8xlbki4ssOjvXb"
```
即 str 為:

```
appid=1000010&business=Online payment&name=BlueOcean Pay&region=HK&key=sxkj0RH9qMxdaxo0sJ8xlbki4ssOjvXb
```

   
1.2.4. 最後計算MD5值並將md5字符串轉換成大寫

```
sign = strtoupper(md5(str))
```

sign的值為:

```
08C612FB4D2D52C8C913EA00E3DABC8B
```

### 1.3 請求頭

為了便於追蹤、統計API請求
需在http頭部設置Uager Agent

#### User Agent 示例

```
# English
BoPayPos/1.1.0 NetType/WIFI Language/en_US

# 簡體中文
BoPayPos/1.1.0 NetType/WIFI Language/zh_CN

# 繁體中文
BoPayPos/1.1.0 NetType/WIFI Language/zh_HK

```
  
### 1.4 PHP簽名示例代碼：


```
<?php
/**
 * 參數數組數據簽名
 * @param array $data 參數
 * @param string $key 密鑰
 * @return string 簽名
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


### 1.5 接口域名

在不同的業務區域，使用不同的域名

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

### 1.6 響應碼說明

響應碼| 響應碼業務說明 | 邏輯處理
-----|--------------|-----------------------------------------------
0   |通用錯誤|未歸類的錯誤，需提示用戶或做相應的容錯處理
200|成功|按照具體業務邏輯處理
201|刷卡交易未知狀態|系統繁忙、用戶支付中、輸入密碼等原因，需調用 `/order/query` 查詢交易狀態
404|接口地址錯誤|根據文檔檢查接口地址
4001|請求數據格式錯誤|請使用JSON數據格式
4002|無效AppId|檢查AppId以及相應的密鑰是否正確
4003|缺少參數|按照接傳遞正確參數
4004|簽名錯誤|檢查簽名邏輯以及相應的密鑰是否正確
4005|通訊出錯|按提示排查代碼錯誤
4006|服務器繁忙|稍後再重新調用該接口
4007|請求方式錯誤|請使用POST方式
40100|無效請求||根據提示處理
40101|訂單已支付|
40102|訂單不存在|
40103|訂單已關閉|
40104|訂單已撤銷|
40105|訂單已退款|
40106|訂單號重復|
40107|餘額不足|檢查商戶賬戶餘額是否不足
40108|訂單超過退款期限|無法完成退款
40109|缺少參數|按照接口提示補全參數
40110|編碼格式錯誤|請使用UTF8編碼格式
40111|每個二維碼只可以用一次|重新獲取用戶支付二維碼
40400|系統繁忙|系統繁忙，稍後重試
40500|支付錯誤|提示用戶相應的信息
40600|未知錯誤|未歸類的錯誤


### 1.7 通用響應數據結構

響應數據為Json格式,Key-Map 描述如下:

屬性    | 說明 | 示例
-------|------|-------
code   | 業務響應碼 | 200
message| 業務提示消息，根據業務結果，可直接使用該屬性值提示用戶 | Success
data   | 業務數據，需根據相應接口進行邏輯處理,有時為空(不存在該屬性) | 

正常示例

```
{
  "code": 200,
  "message": "success",
  "data": {
    "appid": "1000258",
    "attach": "",
    "bank_type": "",
    "body": "Shopping"
  }
}

```


失敗示例

```
{
  "code": 40500,
  "message": "餘額不足"
}
```


## 2 接口列表

### 2.1 支付

Pos機根據使用場景，提交相應請求參數，完成相應支付業務

### Api: 

```
/payment/pay
```
### Parameters 請求參數

字段|變量名|必填|類型|描述
----|----|----|----|----
appid|appid|是|String|appid,由商戶後台獲取，或者登錄獲取
簽名|sign|是|String參考簽名方法
支付方式|payment|是|String|micropay,alipay.qrcode,wechat.qrcode
訂單金額|total_fee|是|Int|支付金額 單位為"分" 如10即0.10元
優惠金額|discount|否|Int|優惠金額 單位"分" 如25即0.25元,默認為0
支付授權碼|code|可選|String|payment為"micropay"時填寫 如支付寶 288271620985824610 微信 134519771100657507 服務端據此參數值區分
商戶自身訂單號|out\_trade\_no|可選|String|如果商戶有自己的訂單系統，可以自己生成訂單號，否則建議交由藍海支付後台自動生成
異步通知url|notify_url|可選|String|異步通知url

#### payment 參數說明

參數值|描述
----------|----------
micropay|刷卡支付 此時需傳遞支付授權碼 `code` 參數
alipay.qrcode|支付寶二維碼
wechat.qrcode|微信二維碼
blueocean.qrcode|混合二維碼 可以直接跳轉到qrcode對應的網址支付，也可以生成二維碼供用戶掃描

支付返回後，檢查交易狀態trade_state,並根據其結果，決定是否調用訂單查詢接口進行結果查詢處理

### 訂單支付狀態 trade_state 說明

```
NOTPAY:未支付
SUCCESS:支付成功
REFUND:已退款
CLOSED:已關閉
REVOKED:已撤銷
USERPAYING:支付中
PAYERROR:支付異常
```

### 正確響應數據說明

響應結果response.data數據說明

字段|變量名|類型|描述
----|----|---|----
訂單id|id|Int|如:10357
藍海訂單編號|sn|String|示例: 11201802091347484054542598 唯一標號，可以用於查詢，關閉，退款等操作
貨幣|fee_type|String|交易貨幣 如 HKD,AUD
商戶名稱|mch_name|String|如 "BlueOcean Pay"
商戶Id|out\_trade\_no|String|商戶訂單號 如: "11201802091347484054542598"
支付方交易號|transaction_id|String| P5631VZG299QZN94JD
支付提供方|provider|String|如:alipay,wechat
訂單時間|create_time|Int|時間戳 如: 1518155270
支付時間|time_end|Int|成功支付的時間戳 如1518155297
交易狀態|trade_state|String| NOTPAY
二維碼文本|qrcode|String|掃碼支付時存在，客戶端使用第三方工具，將該內容生成二維碼，供用戶掃描付款 如 "https://qr.alipay.com/bax03112k12liy7lrysg2004", "weixin://wxpay/bizpayurl?pr=HBXdDeM"
實際支付金額|total_fee|Int|用戶需要支付的金額 如:10
優惠金額|discount|Int|優惠金額，用於商家自身系統集成，顯示 如:2
數據簽名|sign|String|如"7FB42F08C85670A86431F97109DE8683",用於本地校驗



### 請求示例

#### 支付寶二維碼示例:

請求參數

```
{
  "appid": "1000322",
  "payment": "alipay.qrcode",
  "total_fee": 10,
  "wallet": "CN",
  "sign": "520489313B46B5D403CCD8A01267B6C7"
}
```

響應結果

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

#### 混合二維碼示例

請求參數

```
{
  "appid": "1000343",
  "discount": 0,
  "notify_url": "https://payment.comenix.com/index/debug",
  "payment": "blueocean.qrcode",
  "total_fee": 13,
  "sign": "1FBFA9773ACEA258829477ED333381BD"
}
```

響應

```
{
  "code": 200,
  "message": "success",
  "data": {
    "appid": 1000343,
    "attach": "",
    "bank_type": "",
    "body": "",
    "cash_fee": "0",
    "cash_fee_type": "",
    "create_time": "1526957792",
    "detail": "",
    "discount": "0",
    "fee_type": "HKD",
    "id": "97736",
    "is_subscribe": "N",
    "mch_name": "BlueOcean Pay",
    "nonce_str": "kROW6XeRn6",
    "out_trade_no": "11201805221056323300637881",
    "pay_amount": "13",
    "provider": "blueocean",
    "qrcode": "http://api.hk.blueoceanpay.com/order/qrcode/97736",
    "sn": "11201805221056323300637881",
    "time_end": 0,
    "total_fee": "13",
    "trade_state": "NOTPAY",
    "trade_type": "NATIVE",
    "transaction_id": "",
    "wallet": "",
    "sign": "8663CC409008CA4ED66D1F97BDB94414"
  }
}
```



#### 刷卡支付示例 

請求參數

```
{
  "appid": "1000322",
  "code": "134602370743606195",
  "payment": "micropay",
  "total_fee": 5,
  "sign": "5D8883E85FB4D721A0CF53BBD0A12905"
}
``` 

響應

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

當交易發生之後一段時間內，由於買家或者賣家的原因需要退款時，賣家可以通過退款接口將支付款退還給支付用戶，

支付提供方(支付寶，微信等)將在收到退款請求並且驗證成功之後，按照退款規則將支付款按原路退還支付用戶帳號。

#### Api: 

```
/payment/refund
```
#### Parameters 請求參數

字段|變量名|必填|類型|描述
----|----|----|----|----
appid|appid|是|String|appid,登錄時獲取
簽名|sign|是|String|
訂單編號|sn|否|String|與out\_trade\_no二選一,優先使用sn
商戶訂單號|out\_trade\_no|否|String|商戶訂單號
退款金額|refund_fee|否|Int|可選參數，默認為訂單總額，即全額退款 支付寶不支持部分退款
退款描述|refund_desc|否|String|退款說明
退款密碼|password|是|String|退款密碼

#### 返回結果

字段|變量名|必填|類型|描述
----|----|----|----|----
返回碼|code|是|String(32)|返回碼，請參考返回碼表
返回信息|message|是|String(256)|返回信息，成功信息或錯誤信息
返回數據|data|否|Array/String|返回數據集或其他提示信息
  
###### 如果code=200,data参数：  

字段|變量名|必填|類型|描述
----|----|----|----|----
支付提供方訂單號|transaction_id|是|String(28)|微信，支付寶訂單號
商戶訂單號|out\_trade_no|是|String(32)|商戶訂單號
商戶退單號|out\_refund_no|是|String(32)|商戶退單號
微信退單號|refund_id|是|String(64)|微信退單號
標價金額|total_fee|是|Int|最小單位港幣分
標價幣種|fee_type|是|String(8)|一般是HKD
退款金額|refund\_fee|是|Int|最小單位港幣分
退款幣種|refund\_fee_type|是|String(8)|貨幣如 HKD
現金支付金額|cash_fee|是|Int|最小單位 人民幣分
現金支付幣種|cash\_fee_type|是|String(8)| 支付貨幣如 CNY
現金退款金額|cash\_refund_fee|是|Int|最小單位人民幣分
現金退款幣種|cash\_refund\_fee_type|是|String(8)|默認是CNY
匯率|rate|是|String(16)|匯率


#### 响应示例

```
{
   "appid": "1000258",
   "attach": "",
   "bank_type": "",
   "body": "",
   "cash_fee": "0",
   "cash_fee_type": "",
   "create_time": "1523501255",
   "detail": "",
   "discount": "0",
   "fee_type": "HKD",
   "id": "64260",
   "is_subscribe": "N",
   "mch_name": "BlueOcean Pay",
   "nonce_str": "LYuqnmuIlk",
   "out_refund_no": "11201804121050043286306633",
   "out_trade_no": "11201804121047337839520818",
   "pay_amount": "10",
   "provider": "alipay",
   "qrcode": "",
   "refund_desc": "",
   "refund_fee": "10",
   "refund_status": "SUCCESS",
   "refund_time": "1523501406",
   "sn": "11201804121047337839520818",
   "time_end": 1523501255,
   "total_fee": "10",
   "trade_state": "REFUND",
   "trade_type": "MICROPAY",
   "transaction_id": "2NMJVPOM96DMZ70RL8",
   "wallet": "",
   "sign": "FC173A8B25C8AACF1BD3CFFF908F3632"
}

```

### 2.3 訂單操作

訂單狀態操作(查詢，關閉，撤銷)，使用類似的參數

### Api: 

```
查詢
/order/query
關閉
/order/close
撤銷
/order/reverse
```
### Parameters 請求參數

字段|變量名|必填|類型|描述
----|----|----|----|----
appid|appid|是|String|appid,登錄時獲取
簽名|sign|是|String|
訂單編號|sn|否|String|與out\_trade\_no二選一,優先使用sn
商戶訂單號|out\_trade\_no|否|String|商戶訂單號


請求示例

查詢

```
{
  "appid": "1000322",
  "sn": "11201802071854269363947431",
  "sign": "9CC7EB3EBE33D11421FE63298F2EEB94"
}

```

結果

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


### 2.4 訂單列表

訂單列表

### Api: 

```
/order/list
```
### Parameters 請求參數

字段|變量名|必填|類型|描述
----|----|----|----|----
接口ID|appid|是|String|由藍海支付提供的接口應用ID(或通過登錄接口獲取)
簽名|sign|是|String|詳情見簽名規則
支付服務提供方|provider|否|String|默認查詢所有 alipay,wechat
分頁參數|page|否|Int|分頁參數:第幾頁 默認值:1
每頁記錄數|limit|否|Int|每頁條目數 默認值:10
門店Id|store_id|否|Int|門店Id, 登錄接口獲取的store_id > 0 時填寫
交易狀態|trade_state|否|String|交易狀態 如 SUCCESS,REFUND
開始時間|start_time|否|String|按照創建時間查詢訂單 如 2017-12-12
結束時間|end_time|否|String|按照創建時間查詢訂單 如2017-12-13


#### 返回結果

字段|變量名|必填|類型|描述
----|----|----|----|----
返回碼|code|是|String|返回碼，請參考返回碼表
返回信息|message|是|String|返回信息，成功信息或錯誤信息
返回數據|data|否|Array|返回數據集

#### 請求參數示例

```
{
    "appid": "1000322",
    "limit": 10,
    "page": 1,
    "store_id": 1000321,
    "sign": "55F40D7150AD3013E4AB479745FAE542"
}
```

### 返回結果示例

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

#### 請求參數

字段|變量名|必填|類型|描述
----|----|----|----|----
接口ID|appid|是|String|
簽名|sign|是|String|詳情見簽名規則
分頁參數|page|否|Int|分頁參數:第幾頁 默認值:1
每頁記錄數|limit|否|Int|每頁條目數 默認值:10
支付提供方|provider|否|String|alipay,wechat
退款狀態|refund_status|否|String|退款狀態 如 SUCCESS
開始時間|start_time|否|String|按照退款時間查詢訂單 如 2017-12-12
結束時間|end_time|否|String|按照退款時間查詢訂單 如2017-12-13

#### trade_state
```
SUCCESS:退款成功
PROCESSING:退款處理中
REFUNDCLOSE:退款關閉
CHANGE:退款異常
```

#### 返回結果

字段|變量名|必填|類型|描述
----|----|----|----|----
返回碼|code|是|String(32)|返回碼，請參考返回碼表
返回信息|message|是|String(256)|返回信息，成功信息或錯誤信息
返回數據|data|否|Array|返回數據集或其他提示信息


請求參數
```
{
  "appid": "1000322",
  "limit": 2,
  "page": 1,
  "sign": "74CD2AAB31B3FB76A68FEBF8FC56AF70"
}
```

響應結果

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


### 2.6 用戶登錄

### api

```
/user/login
```

### Parameters 請求參數

字段|變量名|必填|類型|描述
----|----|----|----|----
email|email|是|String(32)|登錄帳號
password|password|是|String(32)|密碼

#### 使用 POST 並將請求的數據參數的json字符串以 http body的方式傳遞

#### 該方法無需簽名，可以在chrome 瀏覽器插件 Advanced REST client 中測試

請求參數示例:

```
{"email":"abc@blueoceanpay.com","password":"123456"}
```
該帳號不存在，實際使用中，請由商戶後台獲取。

### Response 響應示例

#### 商戶帳號登錄正確返回示例

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

#### 門店帳號登錄正確返回示例

```
{
  "code": 200,
  "message": "success",
  "data": {
    "appid": 100018,
    "app_key": "TVqec7ZcuG59HepqjQbTi68S78sdLvUh",
    "name": "御龍集團",
    "type":"store",//登錄的帳號類型: merchant -> 商戶, store -> 門店, cashier -> 收銀
    "store_id": 100011,
    "store_name": "中環店"
  }
}
```

### 2.7 添加用戶

### api

```
/user/create
```

### Parameters 請求參數

字段|變量名|必填|類型|描述
----|----|----|----|----
appid|appid|是|String(32)|appid,登錄時獲取
sign|sign|是|String(32)| 
email|email|是|String(32)|登錄帳號
password|password|是|String(32)|
type|type|是|String(32)| merchant -> 商戶 , store -> 門店, cashier -> 收銀

請求參數示例:

```
{
  "appid": "100002",
  "email": "test@blueoceanpay.com",
  "password":"123456",
  "type": "store",  
  "sign": "C770EDDEC3F173CE69B5A46D7A009A59"
}
```


### Response 響應示例

#### 成功示例

```
{
  "code": 200,
  "message": "success",
  "data":{
    "id": "1000047",
    "username": "test@blueoceanpay.com",
    "type": "store",
    ...
  }
}

```

#### 失敗示例

```
{
  "code": 0,
  "message": "帳號已存在"
}
```

### 2.8 設置密碼

### api

```
/user/password
```

### Parameters 請求參數

字段|變量名|必填|類型|描述
----|----|----|----|----
appid|appid|是|String(32)|appid,登錄時獲取
sign|sign|是|String(32)|
email|email|是|String(32)|登錄郵箱帳號
old password|old|是|String(32)|密碼
new password |new|是|String(32)|
repeat password|repeat|是|String(32)|



請求參數示例:

```
{
  "old": "123456",
  "new": "654321",
  "repeat": "654321",
  "sign": "355954524EA6FF44D8582DC50BECF85F"
}
```


### Response 響應示例

#### 修改成功示例

```
{
  "code": 200,
  "message": "success"
}

```

#### 修改失敗示例

```
{
  "code": 0,
  "message": "兩次密碼錯誤"
}
```


### 2.9 刪除用戶

### api

```
/user/remove
```

### Parameters 請求參數

字段|變量名|必填|類型|描述
----|----|----|----|----
appid|appid|是|String(32)|appid,登錄時獲取
簽名|sign|是|String|簽名
要刪除的用戶Id|user_id|否|Int| 可選參數 如果不傳遞，則刪除appid對應的用戶

請求參數示例:

```
{
  "appid":"1000050",
  "user_id":"1000051",
  "sign":"BCA64F4B0A34753497C9DC8943DDFCC9"
}
```

### Response 響應示例

#### 修改成功示例

```
{
  "code": 200,
  "message": "success"
}

```

#### 修改失敗示例

```
{
  "code": 0,
  "message": "用戶已經刪除"
}
```

### 2.10 用戶列表
### api

```
/user/list
```

### Parameters 請求參數

字段|變量名|必填|類型|描述
----|----|----|----|----
appid|appid|是|String(32)|appid,登錄時獲取
簽名|sign|是|String|簽名
分頁參數|page|否|Int|分頁參數:第幾頁 默認值:1
每頁記錄數|limit|否|Int|每頁條目數 默認值:10
用戶類型|type|否|String|默認值: cashier(收銀員) 目前設計 只有store類型的用戶能夠查看同店鋪的收銀員並對其進行相應操作


請求參數示例:

```
{
  "appid":"83",
  "sign":"BCA64F4B0A34753497C9DC8943DDFAED"
}
```

### Response 響應示例

#### 獲取成功示例

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



## Update

2018.03.29