# weblogic爆破

> 原文：[https://www.zhihuifly.com/t/topic/3220](https://www.zhihuifly.com/t/topic/3220)

# weblogic爆破

## 一、部署weblogic

现有的redhat环境7.0,jdk版本1.7。

![image](img/812575e7064f0f641d9c484eccec8e89.png)

## 二、weblogic下载

操作系统：RedHat7

weblogic版本：10.3.6

## 三、安装weblogic

### 1、weblogic安装

创建一个用户

useradd weblogic

passwd weblogic

chmod a+x wls1036_generic.jar

su weblogic

java -jar wls1036_generic.jar -mode=console

出现问题

![image](img/00baa5eed9711ec3104083b832c8cadc.png)

提示空间内存大小不够，清理空间再下一步。

![image](img/07975407bf6720ac74d03d05f75eac69.png)

[root@localhostsrc]# cd /usr/lib/jvm/java-1.7.0-openjdk-1.7.0.51-2.4.5.5.el7.x86_64

![image](img/0a88e1135e90fc017d2e27734fae932a.png)

修改 commEnv.sh 文件

JAVA_HOME="/usr/lib/jvm/java-1.7.0-openjdk-1.7.0.9.x86_64/ jre"

![image](img/51607070781688d44f5e167d6743cbb1.png)

### 2、启动weblogic

[weblogic@localhostroot]$cd /home/weblogic/Oracle/Middleware/user_projects/domains/base_domain/
[weblogic@localhost base_domain]$ ./startWebLogic.sh

![image](img/5669ebea11c8fa5981669b860bb96527.png)

在目录/usr/lib/jvm/java-1.7.0-openjdk-1.7.0.79.x86_64中找不到JRE

编辑setDomainEnv.sh

![image](img/78f228f3589404f1c0c9afa8986b2f0c.png)

重新启动weblogic服务

![image](img/088968c7026fab2283da4167a7b0be0f.png)

## 四、破解weblogic控制台密码

### 第一步 将用户名和密码保存到boot.properties文件中

[root@localhost security]# pwd

/home/weblogic/Oracle/Middleware/user_projects/domains/base_domain/servers/AdminServer/security

在adminserver目录下创建security目录，并创建文件boot.properties

Username=weblogic

Password=weblogic123

### 第二步 重新启动WebLogic服务

[root@localhost bin]# ./startWebLogic.sh&

![image](img/3eb6783948bef079bf1fb6e9998814ab.png)

已经加密

### 第三步 暴力破解

#### 1.java和javac的版本一致

![image](img/4203ef6f18c06c1c67b4f1f7631c432e.png)

#### 2.编译WebLogicPasswordDecryptor.java

![image](img/04979a68f8321a1c08f0c03925fcad30.png)

3.破解密码

![image](img/2fc113f7d96811a325ac7a647e2bfe65.png)