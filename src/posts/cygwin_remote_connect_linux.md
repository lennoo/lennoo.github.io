---
title: Cygwin，以及远程登陆Linux桌面
date: 2008-7-3 21:55:08
---
# 安装Cygwin

在cgywin官方主页下载安装文件setup.exe,这只是一个网络安装包，体积很小。cgywin包含了许多GNU下的应用程序，真正安装时会根据你选择的组件，会自动去网上下载安装的。在国内最好使用镜像服务，这样速度会提高很多，建议去http://www.cygwin.net.cn/ 或 http://www.cygwin.cn/ 下载上述的安装包，并在安装进行到Choose A Download Site这个步骤时，选择合理的镜像。由于中国南北网速的差异，上述两个地址都尝试一下，看看哪个对你而言速度更快一些。

<!-- more -->

在进行到Select Packages这个步骤时，选择你需要包，建议如下：

**Shells -> rxvt-unicode-x**  强大的X终端，可用它替换windows下的cmd.exe
**Net-> openssh**  ssh客户端，可作putty的替换
**Net-> inetutils**  可选，包含一些基本的网络工具，如telnet，否则在cygwin下无法使用windows的telnet
cygwin安装时会自动进行包关联，在安装rxvt时，已自动将X server安装上了。

# 配置调整
启动cygwin,实际上是运行cgywin.bat批处理，它又调用了cmd.exe。我们将安装的rxvt作为默认终端，需要修改cygwin.bat。下面是我机子上的配置修改，请对应修改相应的路径。

```
@echo off
d:
chdir d:\Cygwin\bin
rxvt -e bash --login -i
```

调整rxvt观感，需要修改你用户主目录下的.Xdefaults文件，此文件在你选择的安装目录下的home\usrname下，在我的机子上是D:\Cgywin\home\len。若不存在，可在此目录下新建一个，修改内容如下：

```
Rxvt*background:        black
Rxvt*foreground:        #E2E6C7 
Rxvt*font:              9x16 
Rxvt*boldFont:          9x16 
Rxvt*scrollBar_right:   True
Rxvt*saveLines:         1024
Rxvt*geometry:          80x30
Rxvt*color0:            black
Rxvt*color1:            red
Rxvt*color2:            green   
Rxvt*color3:            yellow
Rxvt*color4:            blue
Rxvt*color5:            magenta
Rxvt*color6:            cyan    
Rxvt*color7:            white   
Rxvt*color8:            burlywood1
Rxvt*color9:            sienna1 
Rxvt*color10:           PaleVioletRed1  
Rxvt*color11:           LightSkyBlue    
Rxvt*color12:           white   
Rxvt*color13:           white
Rxvt*color14:           white   
Rxvt*color15:           white 
```

在cygwin下也是可以访问Windows下其他盘符的,如cd /cygdrive/c/windows，就转到了C盘windows目录下。这样对于在linux下工作的人说有点儿别扭，更希望是以cd /mnt/c/windows的mount方式来访问其他盘符。这需要修改注册表的选项，将HKLM\software\Cygnus Solutions\Cgywin\mounts v2下的子项cygdrive prefix更改为/mnt即可。

# 远程登陆Linux桌面
其实这里介绍的不仅仅适用于Linux，而是针对X Window的。X Widonw的介绍不进行赘述，但需要明确其中的服务器端和客户端的区别，在X Window的概念中服务器端是指你进行显示，输入输出的机器，也是接下来示例中的本机len-computer,IP为10.3.164.70，而客户端指的是进行远程登陆的机器auto-desktop,IP为10.3.164.74。

在局域网内最简单的方法是使用XDMCP连接，这时远程的机器启用xdmcp。那台机器运行着ubuntu-8.04，用gdm进行窗口管理，编辑`/etc/gdm/gdm.conf-custom`如下，其他版本的linux需找到对应的窗口管理的配置文件。

```
[security]
DisallowTCP=false

[xdmcp]
Enale=true
```
修改完后，在远程机器上重启服务，`$sudo /etc/init.d/gdm restart`。接下来本机启动cgywin，转到X目录下，运行Xwin.exe，使用 -query指定远程的linux机器的ip即可。
```
Len@len-computer /usr/X11R6/bin
$ cd /usr/X11R6/bin

Len@len-computer /usr/X11R6/bin
$ Xwin -query 10.3.164.74
```
这里会出现如下面图示的窗口，提示输入用户名和密码。另再附一张在登陆成功后，我在本地执行远程操作的截图。

![](/images/cygwin_remote_connect_linux_1.png)
![](/images/cygwin_remote_connect_linux_2.png)

如果你需要连接的远程机器比较固定，可以修改本地机器d:\cgygin\usrX11R6\bin\startxdmcp.bat中的REMOTE_HOST值为你需要连接机器的IP，这个批处理设置了一些有用的环境变量值。或许你需要创建一个桌面的快键方式，这样每次点击，就直接连接到远程机器了。

# 不引入桌面环境
可能有时候只需要将某个需要X服务的远程应用程序引入到本地桌面显示，而不需要启动像上面的GNOME或者KDE等庞大的桌面环境。这样做比较适合喜欢终端操作的人，我就常常终端敲命令，然后将gvim,openoffice这些从远程导入到本地操作。

找到d:\cgywin\usr\X11R6\bin\startwin.bat，将%RUN% xterm -e /usr/bin/bash -l注释掉，因为我们己经有了rxvt,不需要一个新的xterm终端了，执行该批处理文件，就会在本机运行X server。启动cgywin，用ssh登陆到远程机器上，执行如下命令，导出DISPLAY环境变量和运行你感兴趣的程序。
```
auto@Auto-desktop:~$ export DISPLAY=10.3.164.70:0.0
auto@Auto-desktop:~$ gvim&
[1] 22652
auto@Auto-desktop:~$ oowriter&
```
其中环境变量DISPLAY中的:0.0部分表示X server的display和screen。display指运行着X server实例。如果使用TCP/IP连接，表示的是端口6000+display号做为连接。screen代表X server上的不同输出设备。我在例子中执行gvim和openoffice.org-writer，运行的效果可看下面的截图。在ubuntu上运行着的gvim和openoffice都在我本机10.3.164.70上显示了，并且可操作。

![](/images/cygwin_remote_connect_linux_3.png)

# 有用的链接
[Cgywin/X FAQ](http://x.cygwin.com/docs/faq/cygwin-x-faq.html) 在碰到一些操作问题时，不妨先看看这份FAQ

[使用cygwin X server实现Linux远程桌面](http://blog.csdn.net/easwy/article/details/1807725)easwy介绍了KDE环境下的配置，部分受此启发

[使用rxvt做为cygwin终端](http://blog.csdn.net/easwy/article/details/1812242) 碰到rxvt中文显示问题时，或许有帮助
