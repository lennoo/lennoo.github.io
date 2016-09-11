---
title: 强大的网络传输工具cURL和libcurl
date: 2008-5-26 11:40:08
---
cURL是一个利用URL语法的文件传输工具,是基于libcurl的前端命令行工具。它支持很多协议：FTP, FTPS, HTTP, HTTPS, GOPHER, TELNET, DICT, FILE 以及 LDAP。 它同样支持HTTPS认证，HTTP POST方法, HTTP PUT方法, FTP上传, kerberos认证, HTTP上传, 代理服务器, cookies, 用户名/密码认证, 下载文件断点续传, 上载文件断点续传, http代理服务器管道（ proxy tunneling）, 甚至它还支持IPv6, socks5代理服务器,通过http代理服务器上传文件到FTP服务器等等，功能十分强大。

<!-- more -->

除了使用curl命令行直接进行相关的网络操作，你也可以自由地使用libcurl,它是用C语言编写的，可以绑定到众多的编程语言中，如C,C++,PHP,Python,Perl,Java等等。你可以很方便地利用libcurl，在程序中进行一些网络传输工作，来代替一些语言的内置，使你的知识可重用。在Unix工作环境下，你可以用curl代替wget和ftp等工具，并能将这种学习经验迁移到将来使用libcurl来完成一些自动化任务。

curl是瑞典curl组织开发的，可以通过(http://curl.haxx.se/)来获取更详细的信息和下载文件。

# curl命令行工具使用

curl太强大了，只能对其HTTP的部分作一简单的介绍，其他选项可以参见其附带的手册。它的后端库的使用也非常方便，主要也是在选项设置上，跟命令行基本无异。

## 用法

`curl [选项] [URL...]`

## URL 语法

URL语法是跟协议相关的，具体细节可参见RFC 3986 
可以指定多个URLs或者部分URL地址，通过花括号{}进行分割： 
http://site.{one,two,three}.com 
或者用[]使用字母序： 
ftp://ftp.numericals.com/file[1-100].txt 
ftp://ftp.numericals.com/file[001-100].txt (有前导零) 
ftp://ftp.letters.com/file[a-z]].txt

当前序列嵌套不被支持，但是还是可以使用下列的样式： 
http://any.org/archive[1996-1999]/vol[1-4]/part{a,b,c}.html 
可以在命令行指定任意数量的URLs,它们将以指定的顺序被取回。

从curl7.15.1开始指定可以范围步长，所以可以得到第n个数或字母： 
http://www.numericals.com/file[1-100:10].txt 
http://www.letters.com/file[a-z:2].txt

如果使用了protocal://前缀，curl会将尝试你想使用的协议。它默认使用HTTP，但是其他一些协议也常被用作主机名。比如说，以"ftp"打头的主机名，curl会假定你想使用ftp协议。

Curl会尝试对多文件传输使用重连接，可以使从同一服务器获取很多文件时，不会进行多次的连接。这种做法改进了速度，当然这只会在同一命令行中指定的文件启用，而不会在独立的Curl调用时使用。

## 进度指示器


curl通常在操作时会显示一个进度指示器，来指明当前的传输量，传输速度和预计的剩余时间等等。

但是，即然curl默认将数据显示在终端，如果你调用curl进行操作，它会将数据打印到终端上，这时它会禁用掉进度指示器，否则这些会将输出信息搞乱掉。

如果在进行HTTP的POST或PUT请求时，你想将输出重定向到文件中，可以使用shell的重定向符(>)，或者类似的-o[file]选项。

但是在FTP上传并不会这样，这些操作不会将数据插入到终端中。

如果想使用进度栏，而不是常规的指示器，那么-#会非常有帮助。

## 常用的HTTP选项

**-A/--user-agent<agent string>**
(HTTP)指定用户代理字符串发送给HTTP服务器。如果这个字段没有被设为"Mozilla/4.0"，某些CGIs将不会正常工作。如果在字符串中存在空白字符，需要用单引号标识。这个字段值也可用-H/--header选项进行设置。 
如果这选项被多次设置，最后的设置将起作用。

**-b/--cookie<name=data>**
(HTTP)将data作为cookie传给HTTP服务器，这数据当然是在使用了"Set-Cookie:"后，先前从服务端接收到的。这数据应是"NAME1=VALUE1;NAME2=VALUE2"的格式。 
如果没有"="字符，它将会当作先前存储cookie行的文件名，如果它能被匹配的话。使用这选项，也能激活"cookie parser"，它使curl记录传入的cookies数据。将它与-L/--locaion选项组合将会更加便利。被读取cookie的文件格式应当是文本HTTP头或者Netscape/Mozilla cookie文件格式。 
注意：被-b/--cookie指定的文件只能作为输入使用。没有cookie会存储在这文件中。为了存储cookie，应使用-c/--cookie-jar选项或者直接将HTTP头输出到文件中，用-D/--dump-header选项。这选项可以设置多次，但是只有最后一个起作用。

**-connect-timeout<seconds>**
以秒计的最大超时，用于进行服务器连接时。这只在连接阶段起作用，一旦curl连接建立，这选项将不再起作用。

**-c/--cookie-jar<file name>**
指定在完成一系列操作后，需要将全部的cookie信息保存到哪个文件中。Curl会将先前读取的cookie的信息和从服务器返回的信息一起保存。如果没有cookie信息，则不会进行写文件。cookie信息的文件将与Netscape cookie文件格式保存。如果文件名被设置为'-'，则将cookie打印至终端。 
注意：如果cookie-jar不能被创建写入，整个curl操作也不会失改，甚至不会向你报告错误.使用-v将会得到警告显示，但也只能在可能导致发生致命错误的情况才会显示。

**--create-dirs**
这与-o选项配合使用，curl会在需要时建立本地文件夹结构。这选项会创建在-o选项中涉及到的文件夹。如果-o选项中的文件名没有使用到文件夹，或者所需的文件夹已经存在，则不会有文件夹创建。

**-D/--dump-header<file>**
将协议头写到指定的文件中。当你想存储HTTP站点发给你的数据时，这选项非常有用。在协议头中的cookie将来可以用curl的另外调用来进行读取，那就是-b/--cookie选项。但是-c/--cookie-jar选项将会是更好的存储cookie信息的方法。 
当使用FTP协议时，ftp服务器的应答信息将相应地当作成协议头，然后被存储。

**-p/-proxytunnel**
当HTTP代理被设置后(-x/--proxy)，选项会使不是HTTP协议的传输试图通过代理隧道，而不是表现得HTTP类似的操作形为。代理隧道的方法是通过HTTP服务器直接使用CONNECT请求，让代理直接连接到curl隧道所请求的远程端口号的方式来实现的。

**-o/--output<file>** 
将输出信息打印到文件中，而不是终端。可使用{}或者[]取回多个文档，可在file指定格式中的'#'跟一数字，这样这变量将会由取回的URL字符串所取代。如下： 
curl http://{one,two}.site.com -o "file_#1.txt" 
如果有多个变量，可以写成下面的样子： 
curl http://{site.host}.host[1-5].com -o "#1_#2" 
你可对任意数量的URL使用同样多的这个选项

**-x/--proxy<proxyhost[:port]>**
使用指定的HTTP代理，如果端口号没有被指定，默认为1080. 
这选项会覆盖环境变量中代理服务器的设置。如果环境变量中设置了代理，可将proxy设置为空字符串，来覆盖环境变量中的设置。 
注意：所有通过HTTP代理的操作都会自动转化为HTTP协议。这意味着一些特定协议的操作将会变得无效。这不会有问题，如果在设置了-p/--proxytunnel选项来通过代理隧道进行操作。

# 简单示例

获取cppblog首页，打印至终端

`>curl http://www.cppblog.com`
重定向，保存到文件cppblog.html
`>curl http://www.cppblog.com`
作用同上，使用选项
`>curl -o baidu.html http://www.baidu.com`
使用http代理，可指定IP和端口
`>curl -x 202.127.98.43:80 -o baidu.html http:www.baidu.com`
在访问一些论坛时，常常要求启用cookie，因为这些网站需要启用cookie来记录sessioin信息,这时需要选项-D，将cookie信息保存起来
`>curl -o cpp.html -c len@cppblog.com[1].txt http://www.cppblog.com`

先前保存的cookie信息返回给网站,这通常会传回你的一些用户信息。
`>curl -o cpp.html -c len@cppblog.com[2].txt -b len@cppblog.com[1].txt http://www.cppblog.com`
