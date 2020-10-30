# 建立..文件/文件夹

> 原文：[https://www.zhihuifly.com/t/topic/3495](https://www.zhihuifly.com/t/topic/3495)

# 建立..文件/文件夹

```
echo 'whoami'  > ..\ \ \ \ //注意最后一个转义符后有一个空格 
```

![image](img/882a172ad61d2b13f8d024c6daf916e3.png)

```
bash ..\ \ \ \ \ //这里也是多一个空格。 
```

![image](img/5fb95176b75304e18c37842d7626cdbc.png)

这样建立的文件/文件夹管理员不知道是使用了几个转义符建立的，所以通过rm+tab是无法删除的。而且我测试使用rm -rf ./*也无法删除。