<span style="color:#2b7db0">BlueOcean Pay HongKong Api documents</span>

## 0. 场景解释与流程

1. [POS机客户端接入](scene/pos-client.md)
2. 刷卡支付(客户被扫) `待完善`
3. 扫码支付(客户主扫) `待完善`
4. 公众号支付  `待完善`

（点击以上蓝字可以跳转）

## 1. 接口规则

### 1.1 接口协议

#### 调用蓝海支付接口必须准守以下规则：

1. 请求方式一律使用 POST 并将请求的数据参数的json字符串以 http body的方式传递

2. 请求的数据格式统一使用JSON格式，若参数有json字符串请转义双引号（\"）

3. 字符串编码请统一使用UTF-8

4. 签名算法MD5

5. 所有请求要签名，所有接收要验证签名
  
 
### 1.2 参数签名

1.2.1. 假设请求参数如下：

```
{
      "appid":10000,
      "body":"test"
}
```

1.2.2. 将参数按照键值对（key=value）的形式排列,按照参数名ASCII字典序排序,并用&连接

```
string = appid=10000&body=test
```

1.2.3. 再最后拼接上密钥字符串&key=secretkeyXXX

```
stringTemp = string + '&key=' + secretkeyXXX
```

   
1.2.4. 最后MD5签名,字符串转换成大写

```
sign = strtoupper(md5(stringTemp))
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
	ksort($data);
	$uri = http_build_query($data);
	$uri = $uri.'&key='.$key;
	return strtoupper(md5($uri));
}
```
   

## 2. Api List

### Api Endpoints

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


### 2.1 Order (订单)

2.1.1 [Order create (统一下单)](wechat/order-create.md)
 
```
/wechat/order/create
```
    
2.1.2 [Order query (订单查询)](wechat/order-query.md)

```
/wechat/order/query
```

2.1.3 [Order close (关闭订单)](wechat/order-close.md)

```
/wechat/order/close
```

2.1.4. [Order reverse (撤销订单)](wechat/order-reverse.md)

```
/wechat/order/reverse
```

2.1.4. [Order List (订单列表)](wechat/order-list.md)

```
/wechat/order/list
```

### 2.2 Payment (支付)

2.2.1 [Micropay (刷卡支付)](wechat/payment-micropay.md)

    /wechat/payment/micropay
    

2.2.2 [Refund (退款)](wechat/payment-refund.md)

```
/wechat/payment/refund
```

2.2.3 [Refund query (退款查询)](wechat/payment-refundquery.md)

```
/wechat/payment/refundquery
```

2.2.4 [Refund List (退款列表)](wechat/refund-list.md)

```
/wechat/refund/list
```

### 2.3 Checkout (收银台)

用户扫描商家门店二维码之后打开的移动端h5页面

```
/checkout/:id
```
e.g. 商户门店id为100001的商家门店收银台

```
/checkout/100001

```

### 2.4 Misc (其他)

2.4.1 [汇率查询]

```
/wechat/exchangerate/query
```

2.4.2 [用户登录](user-login.md)

```
/user/login
```

### 2.5 请求参数参考

[api request params examples ](wechat/api-request-params-examples.md)

