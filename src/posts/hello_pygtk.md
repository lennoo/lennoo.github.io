---
title: 初识PyGTK
date: 2008-3-12 19:43:00
---
PyGTK让你用Python轻松创建具有图形用户界面的程序.底层的GTK+提供了各式的可视元素和功能,如果需要,你能开发在GNOME桌面系统运行的功能完整的软件.

PyGTK真正具有跨平台性,它能不加修改地,稳定运行各种操作系统之上,如Linux,Windows,MacOS等.除了简单易用和快速的原型开发能力外,PyGTK还有一流的处理本地化语言的独特功能.

<!-- more -->

PyGTK是自由软件,所以你能几乎没有任何限制的使用，修改，分发，研究它，它是基于LGPL协议发布的．

如果你对上面提到的GTK+,也不了解的话，那允许再对它也进行一番介绍．GTK+，用C语言开发的，具有跨平台的GUI库，它是GNOME桌面系统(如果你在用Linux，一定不陌生)和GIMP图象编辑器的开发工具箱．它是世界上许多程序员的选择，对他们来说，国际化的支持是必要的，而且性能也总是他们考虑的因素．与GTK同一领域的还有Qt库，它是由商业公司开发的C++图形库，虽然它也有免费的．

# 在windows平台的安装和开发

安装PyGTK只需执行下列步骤：

* 安装Python2.4或以上的windows版本[www.python.org](http://www.python.org)

* 从GTK+/Glade的windows版本的GTK+ 2.10开发运行时环境[gladewin32.sourceforge.net](http://gladewin32.sourceforge.net)

* 从PyGTK网站下载安装PyCairo,PyGobject和PyGTK安装包，注意这些需全部安装才能使PyGTK工作[pygtk.org]
或许你对这些步骤还感到麻烦，或者对Python不熟悉的话，那也没有关系，直接下载一键安装包all-in-one installer,为你配置好全部运行时环境．

看看开发环境是否配置正确，将下列代码作为Python脚本或者在Python交互控制台下输入．如果正确的话，应该有一个标题为 **"Hello World"** 的windows的空窗口呈现在你面前．

如果不能运行的话，有可能会出现一个不能成功加载dll的错误提示，这是因为缺少iconv.dll．这时需要只需从网上下载过来，拷贝至windows/system32目录下即可了.

```
import gtk
window = gtk.Window()
window.set_title("Hello World")
window.show_all()

gtk.main()
```
