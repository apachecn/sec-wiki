# （CVE-2019–17621）D-Link DIR-859 rce

> 原文：[https://www.zhihuifly.com/t/topic/3387](https://www.zhihuifly.com/t/topic/3387)

# （CVE-2019–17621）D-Link DIR-859 rce

## 一、漏洞简介

D-Link DIR-859是中国台湾友讯（D-Link）公司的一款无线路由器。 D-Link DIR-859 1.05版本和1.06B01 Beta01版本中涉及UPnP请求的代码存在安全漏洞。远程攻击者可借助特制的HTTP SUBSCRIBE请求利用该漏洞以root权限执行系统命令。

## 二、漏洞影响

D-Link DIR-859 1.05版本和1.06B01 Beta01版本

## 三、复现过程

https://github.com/ianxtianxt/D-Link-DIR-859-RCE

### SUBSCRIBE Exploit

![image](img/1ab4988adc79b41bd95b00d1aefdeb2f.png)

![image](img/0f759bfd2455623b5d5179f28d79a542.png)

### M-SEARCH Exploit

#### Targets

![image](img/fa8583273a44005728d98f74416ce11d.png)

### Payload with (URN:)

![image](img/407879108ec75243aee7922c593a02bf.png)

### Payload with (UUID:)

![image](img/0743913e8004c31806cbbe0c76e2290c.png)