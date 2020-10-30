# 域操作命令

> 原文：[https://www.zhihuifly.com/t/topic/3627](https://www.zhihuifly.com/t/topic/3627)

# 域操作基础命令

```
net group /domain                                 获得所有域用户组列表

net group xxxxx /domain                           显示域中xxxxx组的成员

net group xxxxx /del /domain                      删除域中xxxxx组

net group xxxxx xy /del /domain                   删除域内xxxxx 群组中的xy成员

net group xxxxx /add /domain                      增加域中的群组

net group “domain admins” /domain                 获得域管理员列表

net group “enterprise admins” /domain             获得企业管理员列表

net localgroup administrators /domain             获取域内置administrators组用（enterprise admins、domain admins）

net group “domain controllers” /domain            获得域控制器列表

net group “domain computers” /domain              获得所有域成员计算机列表

net user /domain                                  获得所有域用户列表

net user xxxx /domain                             获得指定账户xxxx的详细信息

net accounts /domain                              获得域密码策略设置，密码长短，错误锁定等信息

net view /domain                                  查询有几个域, 查询域列表

net view /domain:xxxx                             查看 xxxx域中的计算机列表

nltest /domain_trusts                             获取域信任信息

net user domain-admin /domain                     查看管理员登陆时间，密码过期时间，是否有登陆脚本，组分配等信息

net config Workstation                            查询机器属于哪个域

net time /domian                                  查询主域服务器的时间

echo %logonserver%                                查看登陆到这台服务器的计算机名

gpupdate/force                                    更新域策略 `tasklist /S ip /U domain\username /P /V           查看远程计算机进程列` 
```