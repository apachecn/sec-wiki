# 基础网络命令

> 原文：[https://www.zhihuifly.com/t/topic/3625](https://www.zhihuifly.com/t/topic/3625)

# 基本网络命令

```
ipconfig/all                                      查看IP地址

whoami                                            查询账号所属权限

whoami/all                                        查看sid值

systeminfo                                        查询系统以及补丁信息

tasklist /svc                                     查看进程

taskkill /im 进程名称(cmd)                         结束进程

taskkill /pid [进程码] -t(结束该进程) -f(强制结束该进程和子进程)

wmic qfe get hotfixid                             查看已安装过得补丁

wmic qfe list full /format:htable > hotfixid.html 详细的补丁安装

ping hostname(主机名）                             显示该机器名的IP

net start                                         查看当前运行的服务

net user                                          查看本地组的用户

net session                                       查看当前会话

net share                                         查看SMB指向的路径

net use \IP\ipc$ password /user:username         建立IPC会话

wmic share get name,path                          查看SMB指向的路径

wmic nteventlog get path,filename,writeable       查询系统日志文件存储位置

net use \IP\ipc$ password /user:username         建立IPC会话

netstat -ano                                      查看开放的端口

netstat -an | find “3389”                         找到3389端口

net accounts                                      查看本地密码策略

nbtstat –A ip                                     netbiso查询

net view                                          查看机器注释

echo %PROCESSOR_ARCHITECTURE%                     查看系统是32还是64位

set                                               查看系统环境设置变量

net start                                         查看当前运行的服务

wmic service list brief                           查看进程服务

wmic process list brief                           查看进程

wmic startup list brief                           查看启动程序信息

wmic product list brief                           查看安装程序和版本信息

wmic startup list full                            识别开机启动的程序

wmic product > pro.txt                            查看安装软件以及版本路径等信息，输出到pro.txt

if defined PSModulePath (echo 支持powershell) else (echo 不支持powershell)                      查看是否支持posershell

qwinsta                                            查看登录情况

REG query HKCU /v “pwd” /s                         查看保存在注册表中的密码 `for /l %i in (1,1,255) do @ping 192.168.1.%i -w 1 -n 1 | find /i"ttl"      批量扫描内网存活主机（192.168.1.内网IP段）` 
```