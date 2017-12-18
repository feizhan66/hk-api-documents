## PC在线商城接入

### 场景解释
PC端在线商城网站，添加购物车，提交订单，消费者完成付款。

PC在线商城常用的收款场景：
1. 扫码支付（客户主扫）：商城网站系统，生成订单时，并生成临时`收款二维码(code_url)`向消费者展示，消费者主动扫码付款。

### 解决方案
以下开源商城已经实现扩展包(附有安装说明)：
1. OPEN-CART https://github.com/blueoceanpay/opencart-payment-extension
2. EC-SHOP https://github.com/blueoceanpay/ecshop-payment-extension
3. Magento2 https://github.com/blueoceanpay/magento2-blueoceanpay
  
若没有相应解决方案，可以根据贵商城业务逻辑进行定制开发，以下为业务逻辑和对应接口：

### 扫码支付(客户主扫)流程
1. 消费者在商城网站上，将商品添加购物车，结算付款时生成订单；
2. 调用[统一下单]接口，生成付款二维码，供消费者付款；
3. 消费者扫码并支付；
4. 蓝海支付会将支付结果通知贵公司设置的`通知地址notify_url`；   
(如果超时未收到支付结果通知，请主动调用[订单查询接口](/wechat/order-query.md))
5. 收到支付接口通知验证数据签名，并更新订单状态；
6. 建议：浏览器展示收款二维码页面，以AJAX轮询的形式检查订单状态。若支付成功则自动跳转支付后的订单详情页面。


### 扫码支付(客户主扫)接口列表

1. [统一下单接口](/wechat/order-create.md) 
2. [订单查询接口](/wechat/order-query.md)
3. [退款接口](/wechat/payment-refund.md) 
4. [退款查询接口](/wechat/payment-refundquery.md)
5. [支付结果通知](/wechat/payment-notify.md)






