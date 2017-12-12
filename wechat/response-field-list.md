##<span style="color:#2b7db0">字段解释与规范</span>

###### detail 商品详情

商品详细列表，使用Json格式，传输签名前请务必将JSON文本串转义 " 为 \\"。  

goods_detail 服务商必填 []：  
└ goods_id String 必填 32 商品的编号  
└ wxpay\_goods\_id String 可选 32 微信支付定义的统一商品编号  
└ goods_name String 必填 256 商品名称  
└ quantity Int 必填 商品数量  
└ price Int 必填 商品单价，单位为分  

##### 注意： 
 
  a、单品总金额应<=订单总金额total_fee，否则会导致下单失败。  

  b、 单品单价，如果商户有优惠，需传输商户优惠后的单价

例如： 
 
```json
{
    "goods_detail":[
        {
            "goods_id":"iphone6s_16G",
            "wxpay_goods_id":"1001",
            "goods_name":"iPhone6s 16G",
            "quantity":1,
            "price":528800
        },
        {
            "goods_id":"iphone6s_32G",
            "wxpay_goods_id":"1002",
            "goods_name":"iPhone6s 32G",
            "quantity":1,
            "price":608800
        }
    ]
}
```
注意：务必转义 " 为 \\"。

###### scene_info 场景信息

该字段用于统一下单时上报场景信息，目前支持上报实际门店信息。
使用Json格式，传输签名前请务必将JSON文本串转义 " 为 \\"。 

```
{
    "store_id": "NO001",  //门店唯一标识，选填，String(32)
    "store_name":"蓝海支付餐厅" //门店名称，选填，String(64)
}
```

注意：务必转义 " 为 \\"。