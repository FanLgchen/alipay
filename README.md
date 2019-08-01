# alipay
在项目里实现支付宝支付的Demo及说明
## 使用说明
### 支付宝开放平台申请等操作说明
* 支付宝开放平台入口https://open.alipay.com/platform/home.htm
* 创建应用及沙箱环境
* 配置RSA2公私钥
```python
$ openssl
$ OpenSSL> genrsa -out app_private_key.pem 2048  # 制作私钥RSA2
$ OpenSSL> rsa -in app_private_key.pem -pubout -out app_public_key.pem # 导出公钥

$ OpenSSL> exit
```
* 支付宝SDK参数配置ALIPAY_APPID,ALIPAY_URL,ALIPAY_RETURN_URL
### 对接支付宝系统
* 创建支付宝支付对象
```python
alipay = AliPay(
            appid=settings.ALIPAY_APPID,
            app_notify_url=None,  # 默认回调url
            app_private_key_path=os.path.join(os.path.dirname(os.path.abspath(__file__)), "keys/app_private_key.pem"),
            alipay_public_key_path=os.path.join(os.path.dirname(os.path.abspath(__file__)), "keys/alipay_public_key.pem"),
            sign_type="RSA2",
            debug=settings.ALIPAY_DEBUG
        )
```
* 生成登录支付宝连接
```python
order_string = alipay.api_alipay_trade_page_pay(
            out_trade_no=order_id,
            total_amount=str(order.total_amount),
            subject="美多商城%s" % order_id,
            return_url=settings.ALIPAY_RETURN_URL,
        )
```
* 响应登录支付宝连接
```python
 alipay_url = settings.ALIPAY_URL + "?" + order_string
        return http.JsonResponse({'code': RETCODE.OK, 
                                  'errmsg': 'OK', 
                                  'alipay_url': alipay_url})
```


