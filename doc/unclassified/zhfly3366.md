# 微擎 低权限后台getshell

> 原文：[https://www.zhihuifly.com/t/topic/3366](https://www.zhihuifly.com/t/topic/3366)

# 微擎 低权限后台getshell

## 一、漏洞简介

## 二、漏洞影响

## 三、复现过程

/web/index.php?c=site&a=editor

这个文件可以编辑html，然后前台会解析成php

没测试最新版

比如编辑专题：/web/index.php?c=site&a=editor&do=page&multiid=0

上架抓包

![image](img/6090007431422b10ece75e3f0421a3a1.png)

改html内容为php

![image](img/74a0c2a025c2db62eb203a0de339ca21.png)

复制前台url

![image](img/150f907f4130b9745782504931983858.png)

访问之

![image](img/eee86ed9a93313e51dd77e500ea808bd.png)