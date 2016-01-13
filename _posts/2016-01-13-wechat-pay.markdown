---
layout:     post
title:      "iOS微信支付遇到的坑，只出现确定按钮"
subtitle:   " \"\""
date:       2016-01-13 05:26
author:     "Soson"
header-img: "img/post-bg-2016-1-13.jpg"
tags:
    - 微信支付
---



问题：

- **1.调用微信支付代码，调起不到微信app？**
- **2.调用起微信app，但是只出现了确定按钮？**

-------------------

#### **问题一**

>  NSString *stamp  = [dict objectForKey:@"timeStamp"];

``` 
        //调起微信支付
        PayReq* req             = [[PayReq alloc] init];
        /** 商家向财付通申请的商家id */
        req.partnerId           = [dict objectForKey:@"partnerId"];
        /** 预支付订单id */
        req.prepayId            = [dict objectForKey:@"prepayId"];
        /** 随机串，防重发 */
        req.nonceStr            = [dict objectForKey:@"nonceStr"];
        /** 时间戳，防重发 */
        req.timeStamp           = stamp.intValue;
        req.package             = [dict objectForKey:@"package"];
        /** 商家根据微信开放平台文档对数据做的签名 */
        req.sign                = [scUpdate objectForKey:@"sign"];
        [WXApi sendReq:req];
```

问题原因：返回的值为空，或者匹配不上签名

解决办法：检查服务器返回的值是否正确，是否有空值？有空就调不起微信支付，我遇到是这样。





#### **问题二**

>   NSString *stamp  = [dict objectForKey:@"timeStamp"];

``` 
        //调起微信支付
        PayReq* req             = [[PayReq alloc] init];
        /** 商家向财付通申请的商家id */
        req.partnerId           = [dict objectForKey:@"partnerId"];
        /** 预支付订单id */
        req.prepayId            = [dict objectForKey:@"prepayId"];
        /** 随机串，防重发 */
        req.nonceStr            = [dict objectForKey:@"nonceStr"];
        /** 时间戳，防重发 */
        req.timeStamp           = stamp.intValue;
        req.package             = [dict objectForKey:@"package"];
        /** 商家根据微信开放平台文档对数据做的签名 */
        req.sign                = [scUpdate objectForKey:@"sign"];
        [WXApi sendReq:req];
```

这里的原因在于服务器做的签名，微信写给服务器的签名文档是这样写的，如下：

> 步骤3:统一下单接口返回正常的prepay_id ,再按签名规范重新生成签名后，将数据传输给app，参与的字段名为appId，partnerId，nonceStr，timeStamp，package，注意：package的值格式为Sign＝WXPay

服务器写着签名的这些appId，partnerId，nonceStr，timeStamp等都是驼峰式的命名，但是微信的官方demo是这样的

http://wxpay.weixin.qq.com/pub_v2/app/app_pay.php?plat=ios

微信返回json数据：

> { 
> 
> "appid":"wxb4ba3c02aa476ea1",
> 
> "noncestr":"c7231910e1e20e0a191d1642dad303fc",     "package":"Sign=WXPay", 
> 
> "partnerid":"10000100", "prepayid":"wx20160111150104c44da8bc940979319430", "timestamp":"1452495664", "sign":"1FC0C04F79B9B15B4769751C8CEC2C69" 
> 
> }


> //调起微信支付

``` 
            PayReq* req             = [[PayReq alloc] init];
            req.partnerId           = [dict objectForKey:@"partnerid"];
            req.prepayId            = [dict objectForKey:@"prepayid"];
            req.nonceStr            = [dict objectForKey:@"noncestr"];
            req.timeStamp           = stamp.intValue;
            req.package             = [dict objectForKey:@"package"];
            req.sign                = [dict objectForKey:@"sign"];
            [WXApi sendReq:req];
```

问题原因：微信demo是可以运行的，因为微信使用的是小写的，我们的服务器签名用的是大写签名之后返回回来给客户端再调起微信支付app，所以导致只有确定按钮。

解决办法：服务器的签名sign，全部使用appid，partnerid，noncestr，timestamp，package小些的！