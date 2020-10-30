# 3.1 判断是否是域环境

> 原文：[https://www.zhihuifly.com/t/topic/3413](https://www.zhihuifly.com/t/topic/3413)

# 3.1 判断是否是域环境

## 命令汇总

```
net view /domain 查询域列表

net time/domain 从域控查询时间，若当前用户是域用户会从域控返回当前时间，亦用来判 断主域，主域一般用做时间服务器

net localgroup administrators 本机管理员【通常含有域用户】

net user /domain 查询域用户(当前域)

net group /domain 查询域工作组

net group “domain computers” /domain 查看加入域的所有计算机名

net group “domain admins” /domain 查询域管理员用户组和域管用户

net localgroup administrators /domain 查看域管理员

net group “domain controllers” /domain 查看域控 `net accounts /domain 查看域密码策略` 
```

## 命令截图

```
net view /domain 查询域列表 
```

![image](img/074122530300312c7a32b4cb204b049d.png)

```
net time/domain 从域控查询时间，若当前用户是域用户会从域控返回当前时间，亦用来判 断主域，主域一般用做时间服务器 
```

![image](img/d731807e7d9ba20392e8d4361fb16c38.png)

```
net localgroup administrators 本机管理员【通常含有域用户】 
```

![image](img/dad297aeea9fadb7cfd10e9bbce92b82.png)

```
net user /domain 查询域用户(当前域) 
```

![image](img/e210bbde750657d8c2878a1db641275d.png)

```
net group /domain 查询域工作组 
```

![image](img/1fd4837206a2256a9e3cfe9ce74fc752.png)

```
net group "domain computers" /domain 查看加入域的所有计算机名 
```

![image](img/711332e8356eda9612240c7d6929a24f.png)

```
net group "domain admins" /domain 查询域管理员用户组和域管用户 
```

![image](img/9d847ffa9b85202074dc9388de2245a3.png)

```
net localgroup administrators /domain 查看域管理员 
```

![image](img/d3289068b668235800f3b39b730a0376.png)

```
net group "domain controllers" /domain 查看域控 
```

![image](img/1fd262360b3bc7c5fc2052c54ded544d.png)

```
net accounts /domain 查看域密码策略 
```

![image](img/bb6867d1aa7c7a6c55112d19e0c9cf0b.png)