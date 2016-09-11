---
title: PC-Lint简介
date: 2008-1-30 15:18:08
---
PC-lint for C/C++是由Gimpel软件公司于1985年开发的代码静态分析工具,它能有效地发现程序语法错误、潜在的错误隐患、不合理的编程习惯等。

FlexeLint for C/C++是在PC_lint在windows平台获得成功后，同样由Gimpel公司开发的，以源代码形式发布的，在Unix/Linux平台上的静态代码分析工具。

本文主要介绍PC-lint的安装与配置，因此是在windows平台上进行讨论。

<!-- more -->

PC-lint支持几乎所有流行的编译器和IDE环境，可能因为其发展历史和面对专业程序员群体的原因，它是以命令行加配置文件的形式进行使用，所以其使用习惯跟现在常见的windows软件不同。

现以PC-lint与VS2005进行集成来说明：

将PC-lint释放到某一目录下,如: `D:\Program Files\pclint` .将新建一std.lnt文件在主目录中，并将添加上以下的内容

```
au-sm.lnt
co-msc80.lnt
lib-mfc.lnt
lib-stl.lnt
lib-w32.lnt
lib-wnt.lnt
lib-atl.lnt
options.lnt  -si4 -sp4

-i "C:\Program Files\Microsoft Visual Studio 8\VC\include"
-i "C:\Program Files\Microsoft Visual Studio 8\VC\atlmfc\include"
-i "C:\Program Files\Microsoft Visual Studio 8\VC\PlatformSDK\include"
-i "C:\Program Files\Microsoft Visual Studio 8\SDK\v2.0\include"
```

注意：-i后面为相应的vc头文件目录路径,而一系列xxx.lnt是语法配置规则，决定了按什么规则进行检查，以后可以根据需要进行增减. 写在配置文件中相应的的xxx.lnt从lnt子目录中，拷贝到主目录中．（这一步很重要) 在vs2005中的工具->外部工具中，点击＂添加",新建一个外部工具.标题可以任意，可取(pc_lint)；命令为：`D:\Program Files\pclint\LINT-NT.EXE；参数为:-i"D:\Program Files\pclint" std.lnt "$(ItemFileName)$(ItemExt)"` ;初始目录为：$(ItemDir)，并将下面的＂使用输出窗口＂勾选上. 接下来，就可以写一段程序，在工具菜单中选择pc_lint 来进行检查了．如果你编写的程序有不符合定义的规范，则会在输出窗口中出现相关的信息.
