# CatfishCMS 4.5.7 xss

> 原文：[https://www.zhihuifly.com/t/topic/2837](https://www.zhihuifly.com/t/topic/2837)

# CatfishCMS 4.5.7 xss

## 一、漏洞简介

## 二、漏洞影响

CatfishCMS 4.5

## 三、复现过程

### 漏洞分析

文件：application\config.php

参数：default_filter

![image](img/eab9e6433b76544128bfa0e722a3effa.png)

最后找到一处未过滤的地方

文件：application/index/controller/Index.php

方法：pinglun()

![image](img/864f63082c2dd7b10e4992ef83361ec2.png)

过滤函数

文件：application\index\controller\Common.php

方法：filterJs()

![image](img/4969fcb433d6c5574dcfb78ab12067ab.png)

可以看到只是简单的过滤