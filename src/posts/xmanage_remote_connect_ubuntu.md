---
title: Xmanager远程连接ubuntu
date: 2008-6-1 21:10:08
---
涉及到软件：Xmanager 1.3.9 / Windows xp, ubuntu hardy

1. 在ubuntu机器上配置好gdm,修改/etc/gdm/gdm.conf-custom,对照添加如下内容：
'''
[security]
DisallowTCP=false
[xdmcp]
Enable=true
'''
2. 性能调优。这步非常关键，不然使用Xmanager登陆速度非常慢，且会报错，主要原因是gnome使用Esound进行声音数据的传送，需要使用TCP 16001端口。所以我建设在ubuntu关掉混音选项。
系统-首选项-音效-音效,将“允许软件混音”不要勾选上，
系统-首选项-字体-字体渲染,选择"单色",在“细节”的“字体渲染细节”中的平滑和微调选项，都选择"无"。

有用的参考：(http://www.netsarang.com/products/xmg_faq.html)
