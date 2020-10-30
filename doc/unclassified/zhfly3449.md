# lcx

> 原文：[https://www.zhihuifly.com/t/topic/3449](https://www.zhihuifly.com/t/topic/3449)

# 实现LCX功能的参考方法

## 一.LCX简介

lcx是一款强大的内网端口转发工具，用于将内网主机开放的内部端口映射到外网主机（有公网IP）任意端口。它是一款命令行工具，当然也可以在有权限的webshell下执行，正因如此lcx常被认为是一款黑客入侵工具，lcx在内网入侵渗透中起着重要的角色。lcx进行端口转发的原理就是使不同端口之间形成一个回路。它常用于外网连接内网3389等端口。

## 二.LCX实现思路

### 基于命令的LCX

在windows和linux 中都存在对于端口的转发命令，人们习惯于称呼为映射，英文为DNAT(Destination Network Address Translation)，官方名称为目的地址转换，这种方式在防火墙和路由器中比较常见。

### windows 目的地址转换命令简介

Windows下使用netsh命令,来进行目的地址转换。netsh(Network Shell) 是一个windows系统本身**的功能强大的网络配置命令行工具。它允许从本地或远程显示或修改当前正在运行的计算机的网络配置。Netsh 命令的强大，目的地址映射只是其中小部分功能。如此强大的命令如何使用呢？
命令|含义
---|---
netsh interface portproxy add v4tov4 listenaddress=192.168.1.8 listenport=14941 connectaddress=192.168.1.118 connectport=1494|在IPV4下将监听本地IP 192.168.1.8:14941,接受到的数据转发给192.168.1.118:1494。
netsh interface portproxy delete v4tov4 listenaddress=192.168.1.8 listenport=33891|在IPV4下删除监听本地IP 192.168.1.8:33891
netsh interface portproxy show v4tov4|查看在IPV4下配置的端口转发的策略

### linux 目的地址转换命令简介

Linux下使用iptables命令，来进行目的地址转换。在默认情况下，linux会禁用此类功能，需要手动开启。iptables是Linux 内核集成的 IP 信息包过滤系统，也就是人们常说的LINUX自带防火墙，经常被用于配置ACL(Access Control List)访问控制列表。iptables在不同的LINUX版本中存在不同的名称。2.0.X内核：ipfwadm，2.2.X内核：ipchains，2.4.X内核：iptables。目前linux版本基本为2.4.x以上，低版本的linux比较少见，下面讲解的命令，在2.4.x以上的内核版本linux中有效。

开启目的地址转发功能

临时修改法: echo 1 >/proc/sys/net/ipv4/ip_forward

永久修改法: vi /etc/sysctl.conf，net.ipv4.ip_forward = 1找到并修改值为1，sysctl –p（使之立即生效）

假设用户访问172.16.4.247:728时我想让它转发到172.16.4.97:80：

```
# iptables -t nat -A PREROUTING  -p tcp -d 113.108.110.61 --dport 728 -j DNAT --to-destination 172.16.4.97:80

# iptables -t nat -A POSTROUTING -p tcp -s 172.16.4.97 --sport 80 -j SNAT --to-source 172.16.4.247

# service iptables save（将当前规则保存到 /etc/sysconfig/iptables） 
```

本机间不同的端口转发：

```
iptables -t nat -A PREROUTING -p tcp --dport 729 -j REDIRECT --to-ports 80

service iptables save `service iptables restart` 
```

### 自实现LCX

基于命令的LCX存在无法绕过杀毒软件的拦截，并且需要管理员权限等问题。所以大多数LCX是使用自实现目的地址转换的方式编写的工具。简单说下目的地址转换的原理。

目的地址转换的基本工作原理是当公网主机同一IP包发送至存在目的地址转换策略的主机端口时，将IP包中的目的地址以及端口进行替换，并且代发送给替换后的地址和端口。

采用PYTHON编写LCX工具，工具的使用效果感觉上是非常不错的，但是ptyhon程序打包成exe的体积相对比较庞大。笔者推荐使用asyncore 模块，Python的asyncore模块**了以异步的方式写入套接字服务的客户端和服务器的基础结构，使用比较方便。这个模块是一个以时间驱动的异步I/O，与C++的事件选择模型相似。每当发生写、读事件会交由我们重写的事件函数进行处理。下面开始编写自实现的LCX。
1、第一步需要先定义Forwarder：

```
class PortForwarder(asyncore.dispatcher):#监听本地
    def __init__(self, ip, port, remoteip, remoteport, backlog=5):
        asyncore.dispatcher.__init__(self)
        self.remoteip = remoteip
        self.remoteport = remoteport
        self.create_socket(socket.AF_INET, socket.SOCK_STREAM)#创建一个套接字对象
        self.set_reuse_addr()#设置地址端口可重用
        self.bind((ip, port))#绑定本地ip与端口
        self.listen(backlog)#开始监听

```
def handle_accept(self):#处理接受
    conn, addr = self.accept()#等待接受
    print "Connected to:", addr
    Sender(Receiver(conn), self.remoteip, self.remoteport) 
``` 
```

2、第二步调用Forwarder,并且执行asyncore.loop（用于循环监听网络事件）:

```
PortForwarder(local_host, local_port, remote_host, remote_port)
asyncore.loop() 
```

3、需要接*本地的请求，发给远程主机:

```
class Receiver(asyncore.dispatcher):#接受本地请求数据，发送给远程主机
    def __init__(self, conn):
        asyncore.dispatcher.__init__(self, conn)
        #self被初始化为该连接客户端socket
        #getpeername函数用于获取与某个套接字关联的外地协议地址
        self.from_remote_buffer = ''#保存来自远程主机数据
        self.to_remote_buffer = ''    #保存本地请求数据
        self.sender = None

```
def handle_connect(self):
    pass

def handle_read(self):#接受本地请求
    read = self.recv(BUFSIZE)
    self.to_remote_buffer += read;
    print "receiver read", self.to_remote_buffer

def writable(self):#判断是否有来自远程主机的数据，如果有，调用handle_write
    return (len(self.from_remote_buffer) &gt; 0)

def handle_write(self):#发送来自远程主机的数据给本地主机
    sent = self.send(self.from_remote_buffer)
    print "receiver sent", sent
    self.from_remote_buffer = self.from_remote_buffer[sent:]#发送完成后清空数据

def handle_close(self):
    self.close()
    if self.sender:
        self.sender.close() 
``` 
```

4、需要接受远程主机数据，发给本地请求：

```
class Sender(asyncore.dispatcher):#接受远程主机数据，发送本地请求数据
    def __init__(self, receiver, remoteaddr, remoteport):
        asyncore.dispatcher.__init__(self)
        self.receiver = receiver#建立Sender与Receiver之间联系
        receiver.sender = self    #建立Sender与Receiver之间联系
        self.create_socket(socket.AF_INET, socket.SOCK_STREAM)#创建套接字
        self.connect((remoteaddr, remoteport))#连接远程主机

```
def handle_connect(self):
    pass

def handle_read(self):#接受来自远程主机的数据
    read = self.recv(BUFSIZE)
    self.receiver.from_remote_buffer += read
    print "sender read", self.receiver.from_remote_buffer

def writable(self):#判断是否有本地请求要发送，如果有，调用handle_write
    if len(self.receiver.to_remote_buffer) &gt; 0:
        self.receiver.to_remote_buffer = self.receiver.to_remote_buffer.replace\
            (LOCAL_SERVER_HOST + ':' + str(LOCAL_SERVER_PORT), REMOTE_SERVER_HOST)
        #修改本地请求数据，将本地主机中host改为远程主机地址
    return (len(self.receiver.to_remote_buffer) &gt; 0)

def handle_write(self):#发送本地请求数据
    sent = self.send(self.receiver.to_remote_buffer)
    print "sender write",sent
    self.receiver.to_remote_buffer = self.receiver.to_remote_buffer[sent:]

def handle_close(self):
    self.close()
    self.receiver.close() 
``` 
```

到此一个简单的LCX程序就完成了，如果添加一些命令行交互提示功能，对于使用者将更加友好

## 三.总结

对于LCX原理以及实现思路，简单整理这么多。目前来说LCX这类程序，已经被杀毒软件列为木马。python编写程序的优势在于能够绕过杀毒软件的查杀。对于目的地址转发这项工作来说所需的代码比较少量，但是由于PYTHON打包成EXE时会内置编译器，所以EXE程序会相对比较大。如果将部分代码进行优化，能够比较好的缩减代码量。不过虽然代码量可能会有所减少，但是打包后的体积依旧比较大。对于这点目前并尚未找到妥善的解决方法。