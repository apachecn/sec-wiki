# （CVE-2019-11043）PHP 远程命令执行

> 原文：[https://www.zhihuifly.com/t/topic/3072](https://www.zhihuifly.com/t/topic/3072)

# （CVE-2019-11043）PHP 远程命令执行

## 一、漏洞简介

NgiNginx 上 fastcgi_split_path_info 在处理带有 %0a 的请求时，会因为遇到换行符 \n 导致 PATH_INFO 为空。而 php-fpm 在处理 PATH_INFO 为空的情况下，存在逻辑缺陷。攻击者通过精心的构造和利用，可以导致远程代码执行。
nx 上 fastcgi_split_path_info 在处理带有 %0a 的请求时，会因为遇到换行符 \n 导致 PATH_INFO 为空。而 php-fpm 在处理 PATH_INFO 为空的情况下，存在逻辑缺陷。攻击者通过精心的构造和利用，可以导致远程代码执行。

## 二、漏洞影响

Nginx + php-fpm 的服务器，在使用如下配置的情况下，都可能存在远程代码执行漏洞。

location ~ [^/].php(/|$) {
fastcgi_split_path_info ^(.+?.php)(/.*)$;
fastcgi_param PATH_INFO $fastcgi_path_info;
fastcgi_pass php:9000;
...
}
}

## 三、复现过程

https://github.com/ianxtianxt/CVE-2019-11043

```
./phuip-fpizdam.exe [url] 
```

![image](img/101eca7cba4371cb7090a5fd4c3cadee.png)