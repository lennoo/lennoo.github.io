---
title: 简简单单用py2exe打包python脚本
date: 2008-8-11 19:19:08
---
py2exe是实用的python脚本工具，可以将python脚本程序转换为exe执行文件。这样你的python程序就可以没有安装python运行时环境的电脑里运行了。py2exe方便地提取出python运行时所需要的文件档案，你需要做的就是写一个两三行的安装脚本文件。

py2exe可以从 http://sourceforge.net/projects/py2exe/ 下载，唯一需要注意的是下载与你python版本号对应的版本，简单的英文教程 http://www.py2exe.org/index.cgi/Tutorial 非常容易入门。
<!-- more -->

对早先写的一个代理验证脚本进行exe文件封装作为示例，这测试脚本名为HttpProxyTester.py。

首先，最好测试运行一下待封装的脚本以确定没有问题，然后在HttpProxyTester.py脚本的同级目录新建一setup.py文件。

```
# setup.py

from distutils.core import setup
import py2exe

setup(console=['HttpProxyTester.py'])
```

上面的文件首先引入了distutils模块，这模块随python安装分发的，也就是说内置的。接着导入py3exe模块，它其实对distutils做了一些功能扩展。接下来的语句说明是控制台运行。对于windows的GUI模式运行，而不出控制台窗口，则需要setup(windows=['xxx'])之类指令，这对于pyWidget程序将很有用。

在完成安装脚本后，接下来就是在控制台下运行这脚本。
```
>python setup.py py2exe
```
这时会打印出许多log信息，并在同级目录下出现两个新的文件夹：build和dist。build文件夹下是py2exe生成的一些临时文件，dist就是需要分发的文件内容，可以这文件夹打包，然后在别的机子上运行了。

总之，py2exe非常简单实用，三分钟就可以搞定。
