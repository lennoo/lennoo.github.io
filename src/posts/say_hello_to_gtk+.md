---
title: Say Hello to GTK+
date: 2008-3-23 15:18:00
---
Windows下的C++程序员在开发图形用户界面时,首先想到可能就是MFC了.对于[GTK+](http://www.gtk.org/) 这种GNU/Linux上出生出来的东西,就感到陌生了．GTK+是类似于MFC的图形界面库,跟MFC不同的是,它不是用C++,而是用C语言实现了面对对象的机制,但能与许多语言绑定,并具有跨平台的特性.比如与Python的结合,就产生PyGTK,"初识PyGTK"就介结了其在Windows平台的安装.

<!-- more -->

在Windows开发GTK+的应用程序,首先需要其在Windows版本下的库文件,下载gladewin32项目中的gtk+-win32-devel安装包进行安装.我这里使用VS2005进行配置，因为相对于其他的环境，大家对于此IDE更为熟悉．接下来我将一步步介绍我在对GTK/Glande3说Hello时碰到的问题与解决方法．

# 丑陋的Hello World

任何像我一样急切的人，都希望用最少的代码，看一下GTK+的世界是怎么样的．新建一"Win32 项目",然后向其添加gtk.c文件.

```
/*  file gtk.c  */ 
#include  < gtk / gtk.h > 

int  main( int  argc,  char *  argv[])
{
    GtkWidget *  window;
    gtk_init ( & argc,  & argv);

    window  =  gtk_window_new (GTK_WINDOW_TOPLEVEL);
    
    gtk_window_set_title (GTK_WINDOW (window),  " Hello World " );
    gtk_widget_show (window);
    gtk_main();

    return   0 ;
} 
```

现在麻烦的事情到了,设置相关的头文件和库文件.设置头文件,右键点击项目名,然后选择属性.然后在"常规"-"附加包含目录"中设置你安装gtk+-win32-devel时对应的头文件目录.然后再设置库文件,需要gtk-win32-2.0.lib,glib-2.0.lib,gobject-2.0.lib.

![](/images/say_hello_to_gtk+_1.jpg)

![](/images/say_hello_to_gtk+_2.jpg)

![](/images/say_hello_to_gtk+_3.jpg)

设置完好后,你就可以编译运行了,一个什么都不干的丑陋的windows窗体出现你面前了.若编译不通过,缺少某些头文件的话,查看出错信息,然后用windows查找对应头文件路径,再设置.若连接出错,很可能在设置库文件出错了,同理,设置相应的库文件路径.程序代码很简单,定义一个windows窗体指针,然后创建窗体和定义窗体标题,接着就是显示了,和gtk主循环了.

![](/images/say_hello_to_gtk+_4.jpg)

# 让GTK+有点知觉
或许你发现了，点击窗体中的"X"关闭按钮，虽然窗体消失了，但是黑色的命令窗口还在，发现＂任务管理器＂中程序还在运行着．对，它并没有像你想的那样，在点击"X"关闭按钮结束掉程序．这些是需要你告诉程序该怎么做的．我们需要在代码中添加相应的事件处理函数. 新增加了destroy函数，它就是事件处理函数，在gtk也可叫做信号处理函数，因为gtk中用信号来进行事件的通知．destory函数的形参是有约定写法的，这个大家可以参见具体的手册．在主程序中，还需要将这一函数与具体的信号进行关联，见第17行．

在完成这些工作后，你再点击"X"按钮时，会发现"命令行窗口"会出现"按任意键继续"，程序是真正地退出了.

```
/* file gtk.c */
#include 
static void destroy( GtkWidget *widget,
                    gpointer   data )
{
    gtk_main_quit ();            //退出gtk主循环
}

int main(int argc, char* argv[])
{

    GtkWidget* window;
    gtk_init (&argc, &argv);

    window = gtk_window_new (GTK_WINDOW_TOPLEVEL);
    
    gtk_window_set_title (GTK_WINDOW (window), "Hello World");
    g_signal_connect (G_OBJECT (window), "destroy",    G_CALLBACK (destroy), NULL);

    gtk_widget_show (window);
    gtk_main();


    return 0;
}
```

# 让黑黑的命令行窗口消失

有些人会对这个gtk应用程序总会伴随着黑黑的命令行窗口感到不爽，包括我自己在内．不知道gtk程序在gnome下也是这样，我没有试过，不知道．现在我就使用windows平台的特定方法，来让它消失吧，就是使用WinMain做为主函数．这方法或许不是最正规的做法,但我看来却有效．如果要考虑到跨平台的话，可以用条件编译处理下．

```
/* file gtk.c */
#include 
#include 
static void destroy( GtkWidget *widget,
                    gpointer   data )
{
    gtk_main_quit ();            //退出gtk主循环
}

int WinMain(HINSTANCE hInstance,
            HINSTANCE hPrevInstance,
            LPSTR lpCmdLine,
            int nCmdShow
            )
{
    //为应付gtk_init所需要的参数
    int argc=1;    
    char* commandLine={"gtkApplication"};
    char** argv = &commandLine;

    GtkWidget* window;
    gtk_init (&argc, &argv);

    window = gtk_window_new (GTK_WINDOW_TOPLEVEL);
    
    gtk_window_set_title (GTK_WINDOW (window), "Hello World");
    g_signal_connect (G_OBJECT (window), "destroy",    G_CALLBACK (destroy), NULL);

    gtk_widget_show (window);
    gtk_main();


    return 0;
}
```

好啦，现在再编译运行，命令行窗口没有了．你会发现真正的win32窗体了．但是在开发中，我建议用gtk时，还是让命令行窗口显示出来，因为gtk在出错时，会把一些有用的信息打到命令行窗口中，这对你的帮助会很大．
