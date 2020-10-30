# 上路由常用命令

> 原文：[https://www.zhihuifly.com/t/topic/3626](https://www.zhihuifly.com/t/topic/3626)

# 路由查询常用命令

```
tracert IP                                        路由跟踪

route print                                       打印路由表

arp -a                                            列出本网段内所有活跃的IP地址

arp -s （ip + mac）                               绑定mac与ip地址

arp -d （ip + mac）                               解绑mac与ip地址

netsh firewall show config                        查看防火墙策略 `netsh firewall show state                         查看防火墙策略` 
```