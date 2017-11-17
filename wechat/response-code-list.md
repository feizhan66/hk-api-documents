##<span style="color:#2b7db0">返回码</span>

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

