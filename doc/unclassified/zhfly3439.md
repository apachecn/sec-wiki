# ew

> 原文：[https://www.zhihuifly.com/t/topic/3439](https://www.zhihuifly.com/t/topic/3439)

# EW正向代理

Web服务器的设置

```
如果是Linux系统
./ew_for_linux64 -s ssocksd -l 1080  #监听本地的1080端口 `如果是Windows系统

ew_for_Win.exe -s ssocksd -l 1080   #监听本地的1080端口` 
```

主机的设置

```
 如果是Linux系统，配置proxychains代理链的配置文件，将代理设置成 100.100.10.12的1080端口：socks5 100.100.10.12 1080 
然后命令前面加上 proxychains即可。如：proxychains curl 192.168.10.19 `如果是Windows系统，直接浏览器中设置代理为 100.100.10.12的1080端口，或者利用  Proxifier 、sockscap64 设置全局代理` 
```

# EW反向代理

Web服务器的设置

```
如果是Linux系统：
./ew_for_linux64 -s rssocks -d 100.100.10.13 -e 8888  #将本机的流量全部转发到100.100.10.13的8888端口

如果是Windows系统：

ew_for_Win.exe -s rssocks -d 100.100.10.13 -e 8888  #将本机的流量全部转发到100.100.10.13的8888端口 
```

主机的设置

```
 如果是Linux系统：
./ew_for_linux64 -s rcsocks -l 1080 -e 8888  #将本机的8888端口的流量都转发给1080端口，这里8888端口只是用于传输流量
然后配置proxychains代理链的配置文件，将代理设置成 127.0.0.1的1080端口：socks5 127.0.0.1 1080 
然后命令前面加上 proxychains即可。如：proxychains curl 192.168.10.19

如果是Windows系统

ew_for_Win.exe -s rcsocks -l 1080 -e 8888   #将本机的8888端口的流量都转发给1080端口，这里8888端口只是用于传输流量

然后浏览器中设置代理为 100.100.10.12的1080端口，或者利用  Proxifier 、sockscap64 设置全局代理 
```