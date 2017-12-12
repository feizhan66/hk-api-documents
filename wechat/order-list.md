##<span style="color:#2b7db0">订单列表</span>

#### 应用场景


#### 接口地址

api：

```
/wechat/order/list
```

#### 请求参数

字段|变量名|必填|类型|描述
----|----|----|----|----
接口ID|appid|是|String(32)|由蓝海支付提供的接口应用ID
签名|sign|是|String(32)|详情见签名规则
分页参数|page|否|Int|分页参数:第几页 默认值:1
每页记录数|limit|否|Int|每页条目数 默认值:10

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
交易状态描述|trade_state_desc|是|String(256)|交易状态描述

###### 如果code=200,data参数：  

字段|变量名|必填|类型|描述
----|----|----|----|----
接口ID|appid|是|String(32)|由蓝海支付提供的接口应用ID