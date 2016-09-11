---
title: 简单的QQ代理验证
date: 2008-7-2 22:13:08
---
QQ在许多公司内部被禁止使用,为了能使用QQ,稍微懂点儿计算机的人都知道用代理。QQ提供了socket和http代理这两种功能，socket代理功能强大，但一般公司对外允许连接的端口号比较有限，难以利用。大多数公司是允许连接外部的80端口的，这样使用QQ的http代理是可行的。但是找到能用的QQ代理有点儿麻烦，因此下面的Python代码提供了自动进行QQ代理验证的功能。

<!-- more -->

```
import urllib2
import socket
import re

f = urllib2.urlopen('http://www.proxycn.com/html_proxy/http-1.html')
content = f.read()
f.close()
ipPattern = re.compile(r'(\d+\.\d+\.\d+\.\d+):80')
ipList = ipPattern.findall(content)
print ipList
requestData = "CONNECT http.tencent.com:443 HTTP/1.1\x0d\x0a"
requestData += "Accept: */*\x0d\x0aContent-Type: text/html\x0d\x0a"
requestData += "Proxy-Connection: Keep-Alive\x0d\x0a"
requestData += "Content-length: 0\x0d\x0a\x0d\x0a"
for ip in ipList:

    s = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
    try:
        s.connect((ip,80))
        s.send(requestData)
        data = s.recv(64)
        if data.find("200 Connection established")!= -1:
            print ip, 'good'
            # A vailable proxy is found once, then exit the program
            s.close
            exit(0)
        else:
            print ip, 'bad'
    except socket.error:
        print ip, 'dead'
    finally:
        s.close
```
程序在找到一个可用的QQ代理后退出，用good标识。另两种代理服务器的状态是dead,说明本地无法连接到代理服务器，或是bad，能与代理服务器建立连接，但是代理不能与QQ服务器通讯。

# 代码思路

通过代理中国获取到80端口的代理服务器列表，使用了urllib2模块获取页面数据，然后正则表达式解析出80端口的IP地址存入list中。接下来的几行代码简单，但是很重要，使用较为底层的socket对象，构造合适的数据包通过代理，请求与QQ服务器连接，通过读取的返回数据包来验证连接是否能建立。

这里主要涉及到了HTTP协议的CONNECT的概念，很多人可能认为http代理只是为web浏览提供服务，其实CONNECT方法允许允许用户建立TCP连接到任何端口，这意味着代理不仅可用于HTTP，还可用于FTP，QQ等其他协议。只是网上提供CONNECT方法的代理服务器比较少，我有时候扫了一大堆，也没有找到一个可用的代理。反过来说，有时候你找到的能浏览网页的http服务器，未必能用在QQ上，QQ需要的是能CONNECT的代理。网页浏览一般只使用HTTP协议的GET或POST方法，提供这两种方法的服务器就多了。

了解了代码的原理，稍做改动，就可以用于其他类型的代理的验证了，需要的是一些基本网络知识和数据报的发送和接收。
