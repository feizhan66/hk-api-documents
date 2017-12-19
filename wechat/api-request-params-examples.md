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
  "auth_code": "135753487434252033",
  "sign":"8C31CB157403F9DE73A10822BB5ED21B"
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
  "refund_desc":"refund",
  "sign": "75F21CC379348C27C4D7671960D1928A"
}

```


### payment refund query

api: `/wechat/payment/refundquery`

```
{
  "appid": "100018",
  "out_trade_no": "20171019173009269929",
  "sign": "7D7B97885AFD52345DB0BBA40F7C9563"
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
  "trade_type":"NATIVE",
  "sign": "0E4F15BF0DB527B17BEA1C6E3854EDEE"
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
  "out_trade_no": "201710161001",
  "sign": "9AE48D02F6C4AB78E6E88E69864218CE"
}

```

### Order close

api: `/wechat/order/close`

request data:

```
{
  "appid": "100018",  
  "out_trade_no": "20171020152851532725",
  "sign": "879BCAFA20E0B1D63507125E48039D08"
}

```


### Order reverse

api: `/wechat/order/reverse`

request data:

```
{
  "appid": "100018",  
  "out_trade_no": "20171020151457874992",
  "sign": "0DC7D82A0A621075D3729281FA61CF12"
}

```

### 以上示例中的sign参数因key的不同可能会不可用，实际调试过程中，用自己的key去计算签名。








