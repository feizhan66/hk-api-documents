#<span style="color:#2b7db0">BlueOceanPay Api test</span>

### Api EndPoint

[https://api.hk.blueoceanpay.com](https://api.hk.blueoceanpay.com)

# Api request data

## 1. Payment Controller

### Micropay

api: `/wechat/payment/micropay`

request data:

```
{
  "appid": "100018",
  "device_info": "iPhone 8",
  "body": "BlueOceanPay",
  "detail": "HEHE",
  "out_trade_no": "201710161001",
  "total_fee": 100,
  "spbill_create_ip": "127.0.0.1",
  "auth_code": "135753487434252033"
}

```


### payment refund

api: `/wechat/payment/refund`

request data:

```
{
  "appid": "100018",
  "nonce_str":"",
  "sign":"",
  "transaction_id":"4200000030201710199028128455",
  "out_trade_no": "20171019173009269929",
  "out_refund_no":"2017101917303010001",
  "total_fee":"2",
  "refund_fee":"2",
  "refund_desc":""
}

```


### payment refund query

api: `/wechat/payment/refundquery`

```
{
  "appid": "100018",
  "out_trade_no": "20171019173009269929"
}

```


## 2. Order Controller

### Order create

api: `/wechat/order/create`

request data:

```
{
  "nonce_str":"abc",
  "device_info":"Restful Client",
  "body":"BlueOcean Pay",
  "detail":"detail",  
  "appid": "100018",  
  "out_trade_no": "20171103161001",
  "total_fee":2,
  "spbill_create_ip":"127.0.0.1",
  "trade_type":"NATIVE"
}
```

response data:

```
{
  "code": 200,
  "message": "请求成功",
  "data": {
    "prepay_id": "wx20171103155938c8c2cd47800423251652",
    "code_url": "weixin://wxpay/bizpayurl?pr=tIVlUpD",
    "trade_type": "NATIVE"
  }
}
```



### Order query

api: `/wechat/order/query`

request data:

```
{
  "appid": "100004",  
  "out_trade_no": "201710161001"
}

```

### Order close

api: `/wechat/order/close`

request data:

```
{
  "appid": "100018",  
  "out_trade_no": "20171020152851532725"
}

```


### Order reverse

api: `/wechat/order/reverse`

request data:

```
{
  "appid": "100018",  
  "out_trade_no": "20171020151457874992"
}

```





