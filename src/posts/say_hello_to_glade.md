---
title: Say Hello to Glade
date: 2008-3-27 20:49:00
---
Glade是针对GTK+工具箱与GNOME桌面开发环境的快速图形界面开发工具.用Glade设计的用户接口以XML的文件形式保存,然后根据需要由程序通过libglade库文件来动态加载.因为使用了libglade库,Glade XML文件能够被C,C++,Java,Perl,Python,C#等等语言所支持.针对其他未涉及的语言的支持也是方便的.

在网上可以见到某些关于Glade的教程,大都是关于Linux平台和Glade 2的,因为原先Glade作为快速开发工具,集成代码生成功能,生成C文件.所以常常有初学者对网上某些教程所提及的"generate"(生成代码)功能表示迷惑,在新版本的Glade-3上找不到对应的功能.

<!-- more -->

新版本的Glade-3是对原先Glade代码的完全重写.一个显著的变化就是去除了代码生成功能.这样做是有原因的,即然代码生成功能不被提倡使用,而是更鼓励使用libglade功能.但是如果你真需要代码生成功能的话,它还是可以做为插件来提供的.另一个显著的不同是glade-3设计用来最大化使用GObject的自省机制(GObject introspection),来使外部工具箱和部件的控制,信号和属性的集成更加容易.

如果看过Say Hello to GTK+的话，可能感觉那样的窗体程序太简单了．那么现在让我们借助Glade弄点儿复杂一点儿的界面吧．首先来瞧瞧Glade长什么样,下图就是Glade在windows下的界面.左边的窗体的小部件选择器，相当于调色板．中间是主菜单，右边的是属性窗体．

![](/images/say_hello_to_glade_1.jpg)

现在开始创建一个类似于文本编辑器的图形界面．按照上图标注的顺序，依次添加window部件，vertical box部件,menu bar部件,text view部件和Status部件.vertical box设置三行，它是用来进行界面布局，分割空间用，这是gtk+设计与传统的windows UI设计很不同的地方．后三个部件是放置vertical box中的，最后设计完成图形如下．保存取名为win.glade.如果你感兴趣的话，可以用文件编辑器打开这个文件看看，正如所说的那样，它是一个xml格式的文本文件．

![](/images/say_hello_to_glade_2.jpg)

现在我们设置相关的头文件和库文件，编辑一个glade.c文件，添加进以下的代码，运行看看，会出现如上图的对话框．虽然这个对话框什么都不干，但是通过Glade，我们能较为容易地设计界面，而不用通过gtk函数，一个一个将控件实现.

```
#include <gtk/gtk.h>
#include <glade/glade.h>
int main(int argc, char* argv[])
{
	GladeXML        *gxml;
	GtkWidget       *window;

	gtk_init (&argc, &argv);
	gxml = glade_xml_new ("win.glade", NULL, NULL);
	window = glade_xml_get_widget (gxml, "hello");
	g_object_unref (G_OBJECT (gxml));
	gtk_widget_show (window);                
	gtk_main ();

	return 0;
}
```
