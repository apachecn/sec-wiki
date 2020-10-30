# Cobalt Strike 安装说明

> 原文：[https://www.zhihuifly.com/t/topic/3639](https://www.zhihuifly.com/t/topic/3639)

# Cobalt Strike 安装说明

![image](img/9a224a158a2344a41701955161db71fe.png)

## 安装

Cobalt Strike 分为客户端和服务端，可分布式操作、协同作战。服务器端只能运行在Linux系统中，可搭建在VPS上。

### 服务端

服务端关键的文件是teamserver以及cobaltstrike.jar，将这两个文件放到服务器上同一个目录，然后运行：

> 安装java环境

```
sudo apt install default-jre 
```

> 赋权

```
chmod +x teamserver 
```

> 安装screen

```
apt-get install screen 
```

> 输入

```
screen 
```

> 在新窗口执行我们需要执行的命令

```
./teamserver ip pass 
```

```
按下组合键Ctrl a d关闭窗口并后台执行
screen -ls         查看后台会话
screen -ls         重新进入会话窗口 
```

## 客户端

客户端在Windows、Linux、Mac下都可以运行 (需要配置好Java环境)。启动Cobalt Strike客户端，输入服务端的IP以及端口、连接密码，用户名可以任意设置。

![image](img/7764ea607ec11f7e225a8ae5d3a09295.png)

在控制台所有操作指令都会被记录保留在Cobalt Strike目录logs下。