# （CVE-2020-11107）XAMPP任意命令执行漏洞

> 原文：[https://www.zhihuifly.com/t/topic/3271](https://www.zhihuifly.com/t/topic/3271)

# （CVE-2020-11107）XAMPP任意命令执行漏洞

## 一、漏洞简介

在windows下，XAMPP允许非管理员账号访问和修改其编辑器和浏览器的配置，编辑器的默认配置为notepad.exe，一旦修改配置后，则对应的每个可以访问XAMPP控制面板的用户都更改了配置。当攻击者将编辑器的值设置为恶意的.exe文件或.bat文件，与此同时如果有管理员账号通过XAMPP控制面板查看apache的日志文件，便会执行恶意的.exe文件或.bat文件，以此达到任意命令执行。

**利用条件**

*   1：windows系统、
*   2：装有XAMPP、
*   3：拥有系统普通账户、
*   4：管理员账户通过XAMPP面板查看logs文件。

## 二、漏洞影响

Apache Friends XAMPP <7.2.29

Apache Friends XAMPP 7.3.*，<7.3.16

Apache Friends XAMPP 7.4.*，<7.4.4

## 三、复现过程

### 环境安装

**第一部分：首先以管理员身份登录到****windows10****：**

> 运行cmd查看当前用户，这里的Scarlett即登录的有管理员权限的账户：

![image](img/51afa593fb2b018a0f8d4e7383fd2926.png)

> 安装XAMPP，注意要使用管理员权限进行安装，根据提示不要装在C盘，本次复现安装在D盘，其他按照默认安装即可，最后安装完成如下图所示：

![image](img/4d68f8d5adb684f0fb388e6c60d55eeb.png)

> 运行cmd，输入如下命令：powershell start-process cmd -verb runas （用powershell启动管理员权限的cmd进程），在管理员权限的cmd上，输入：net user lowuser /add，创建一个普通账号lowuser，通过net user lowuser 可知为普通权限账号：

![image](img/0d84930e0b666ac1faf1b3d30009f06a.png)

> 输入命令net user lowuser * 为lowuser设置密码：

![image](img/6fa1bdcde656f16c85b5a49bfbe06551.png)

> 关闭cmd命令窗口，注销管理员权限的Scarlett账户。

![image](img/66557d9d32d4cc40301a8e813ecc8400.png)

### 漏洞复现

**以普通账号lowuser登录到windows10：**

> 输入上面第4步设置的密码，登录lowuser账户：

![image](img/ead1953826a93a60aec06c96d8daccc2.png)

> 设置显示文件扩展名和隐藏项目：

![image](img/2d94aa894d89df1763bda19fd4e2e5de.png)

> 创建command.bat文件，输入命令如下，其作用是将lowuser账号加入管理员权限：

```
@echo off 
net localgroup administrators lowuser /add 
```

![image](img/8fcfa732877c87021fadc49fd5e9195f.png)

> 运行xampp，并在控制面板上找到config配置。

![image](img/1ae4de47ae58d028cd9a23ae3b6261d8.png)

> 修改编辑器的默认配置，更改为刚才创建的command.bat文件，添加并应用，如下图所示：

![image](img/56feb82b2cd8e0452f239959058928a5.png)

> 查看lowuser的用户组，还是普通权限，注销lowuser账户。

![image](img/a29a0bd8ff1286b3ce78141b965a529b.png)

**再次以管理员（Scarlett）登录到windows10：**

> 打开XAMPP控制面板，点击查看logs文件：

![image](img/ddb8a20cf3cfc33e9ecd9631c0caa6fe.png)

**切换到lowuser账户：**

> 运行cmd，查看lowuser用户组，发现已经提升为administators组：

![image](img/bdd829e09b32d77bacef1c096d4127fe.png)

## 参考链接

> https://mp.weixin.qq.com/s?__biz=MzU1ODg3NTMyMQ==&mid=2247490782&idx=1&sn=3c5843a34f4415e25bc0c21388c0e03a&chksm=fc1e8b08cb69021e7da183a3c9e0750cf51f9856fb647eea1f103f41bea609c64f10c9553364&mpshare=1&scene=1&srcid=&sharer_sharetime=1587143054964&sharer_shareid=346bf064ccfaeb680ec3e1af3a4fc9a8&key=64daf1adc09d7c6e5cfb777986ec80c9619f47ef9565c6f0252270416251ea412f230e87b1ec8f3435bc4fb2a9a7b5cfaef4e5e24f07f4ca67b17c37a019205e77237996ec31a74ff713a0436cb2a3a9&ascene=1&uin=MTU0OTU5NDkzMA==&devicetype=Windows+10+x64&version=6209005d&lang=zh_CN&exportkey=AWykz1/C4yy1gXuSSIh8VsI=&pass_ticket=CsmWLKOnF5M0Q2QFomZUfTPJMWXGf5jEVxKfIZtVeWb8bpfTluZvRRNsuVmT2X9Z