---
title: Amazon EC2注册及应用 
date: 2012-5-25 23:03:08
---

Amazon EC2是Amazon云计算的一部分，相当于一台VPS，现提供有限额的一年的免费使用时间。注册Amazon EC2需要有Visa或Master的信用卡进行支付验证，会扣除一美元，然后再把这钱重新返还。（我出现2次扣款短信，实际登陆只发现可用信用额少，但没有出应付款帐单）。

注册完Amazon EC2后，需要创建实例可部分参考 [Amazon EC2 Ubuntu折腾笔记](http://lanhl.com/2011-amazon-ec2-ubuntu-lamp-wordpress.html)，有几点需要注意：一是区域测试过日本机房和新加坡机房，对我所处的位置来说，新加坡的ping时间在200ms水平，而日本在300ms水平，新加坡的更适合。二是登陆用的是密钥形式，如用putty需要格式转换，另不同的ami的登陆名不同，ubuntu官方的ami的id为099720109477，用户名为ubuntu，而不是Amazon实例的ec2-user。

<!-- more -->

创建完EC2实例后，就是一般的虚拟主机使用方法，我现有两个用途：ssh转发和反向代理。ssh转发用的组合是 *firefox+foxyproxy+putty* ，其中putty在Tunnels页配置隧道，端口可随意，一般选7070，目标为Dynamic。在foxyproxy的代理配置页，需要Socks proxy?勾选上，我在这步开始没有注意，发现端口创建连接都失败，来回排查putty,foxyproxy，后来将装了autoproxy插件工作正常，才兜回来发现这步错误。配置完毕，就可自由地浏览网络了。

反向代理用的是nginx，在ubuntu 10.04.2下的操作:
1. 安装nginx，`$sudo aptitude install nginx`
2. 修改配置文件,`$vim /etc/nginx/nginx.conf` ，在http段落下添加
```
server {
listen 80;
server_name www.lengoo.com lengoo.com;
location ／ {
proxy_redirect off;
proxy_pass http://len-home.appspot.com;
proxy_set_header  X-Real-IP  $remote_addr;
proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
}
}
```

上面其中 **len-home.appspot.com** 为appengine的应用程序名，对于Amazon Linux AMI(应是CentOS)可参见使用免费的 amazon EC2做google app engine反向代理，但文中提到用转发到 http://ghs.google.com ，我照其方法会跳转到错误的google首页。

