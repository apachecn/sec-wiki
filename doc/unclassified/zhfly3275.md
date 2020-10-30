# XDCMS 1.0 xss漏洞

> 原文：[https://www.zhihuifly.com/t/topic/3275](https://www.zhihuifly.com/t/topic/3275)

# XDCMS 1.0 xss漏洞

## 一、漏洞简介

## 二、漏洞影响

XDCMS 1.0

## 三、复现过程

漏洞文件：`system\modules\xdcms\template.php`，URL：`index.php?m=xdcms&c=template&f=edit&file=footer.html`

![image](img/ced7418c2d9685d6d7b5d4c0e809a470.png)

插入xss平台代码

![image](img/9cc75b77484feaf4fec594ecf1165c3b.png)

成功接受到信息

![image](img/246550c7b06c3e66faa9940aaf4445b2.png)