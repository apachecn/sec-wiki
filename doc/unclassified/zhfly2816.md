# （CVE-2017-15715）Apache HTTPD 换行解析漏洞

> 原文：[https://www.zhihuifly.com/t/topic/2816](https://www.zhihuifly.com/t/topic/2816)

# （CVE-2017-15715）Apache HTTPD 换行解析漏洞

## 一、漏洞简介

Apache HTTPD是一款HTTP服务器，它可以通过mod_php来运行PHP网页。其2.4.0~2.4.29版本中存在一个解析漏洞，在解析PHP时，`1.php\x0A`将被按照PHP后缀进行解析，导致绕过一些服务器的安全策略。

## 二、漏洞影响

Apache HTTPD 2.4.0~2.4.29

## 三、复现过程

上传一个名为1.php的文件，被拦截：

![image](img/5fce04a424f14b4e678042ac3c834070.png)

在1.php后面插入一个`\x0A`（注意，不能是`\x0D\x0A`，只能是一个`\x0A`），不再拦截：

![image](img/87ea05d71cf05d3f96a2d504af8729ae.png)

访问刚才上传的`/1.php%0a`，发现能够成功解析，但这个文件不是php后缀，说明目标存在解析漏洞：

![image](img/12901d286de3a08a0ef868c0c58ea616.png)

## 参考链接

> https://vulhub.org/#/environments/httpd/CVE-2017-15715/