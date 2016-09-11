---
title: 可爱的Vim
date: 2008-5-25 20:19:00
---
Vim是功能强大的文本编辑器,但是每个工具都有其针对的适用群体。如果你只是偶尔做些文本编辑工作的话，那灵活而又显得繁琐的设置，以及特别的操作方式可能不适合你。但是你是跟我一样，是个平平凡凡的程序员，每天要花费大量时间在写代码,把弄着各式各样的程序语言:C\C++,Python,Tcl,Html,Xml,...，那么你可能需要像Vim这样的工具，即使你要在它上面花费些时间去熟悉和适应它。

先讲述一下，我跟Vim相处的过程，这是个从认识，到抛弃，到再认识，到再学习，到喜欢的过程。最早接触到Vim是在Solaris上，需要修改编辑一些配置文件，看着其他工程师们手指随意地在键盘上敲击，就完成内容的修改，根本没有动用到什么鼠标，那是好生羡慕。严格意义来说，那时候碰到还不是Vim，只是VI而已。在终端上工作，没有什么Notepad之类的程序，只好把指令抄在纸上，查查网上的资料，学会了h,j,k,l,w,q,e,这几个简单指类来进行简单的文本查看工作，仅此而己。后来在Windows上安装了VIM，但是挣腾了几下，没有适应过来，也就只好使用UltraEdit了。UltraEdit对一般的纯文本，按Windows习惯来说是蛮好使的。再后来，玩了会儿ruby，又装起了Vim，但是那时候的对Vim的使用也只是限于上面的简单的指令，再加上Vim的插件，来完成语法高亮，ruby中的MVC文件的方便跳转而已，还是没有习惯Vim，有时候还是不经意用UltraEdit来打开查看编辑文件。直到最近，需要编写Docbook，以及用Python，才真正花费了大量时间来学习使用Vim，才真正认识到到它的可爱。

<!-- more -->

接着说说，我为什么使用Vim，觉得值得学习它，喜欢它的理由吧，纯粹以自己的观点来叙述。

**跨平台性**，无论在Windows，Linux,Solaris,FreeBSD等等操作系统上，以及一些名都没有听过的系统上，你都可以找到它。这样就保证了你的学习投资的保值性，就拿UltraEdit做对比吧，即使你在UltraEdit上学会灵活运用许多功能，到了Linux上，你在这部分学习投资就没有价值了，你可能需要找其他称手的编辑器，然后再进行学习一些功能。特别在一些古老的大型机上的系统上，即使没有Vim，一般来说，还有Vi的，这样一般简单的操作命令还是可复用的。如果你确定你一直只呆在Windows上可忽略这一点。

**开源免费**,Vim是开源软件，意味着你可以自由使用，修改，查看它的代码。我对FreeSoftware,Open Source,Copyright，这些都是持中间立场的。对于自由查看，修改程序代的保证，有总比没有好。对于盗版软件，你有能力还是不要使用的好。正是这一特性，也是促使我放弃UE,投向Vim的重要原因。如果你对于使用盗版软件蛮不在乎，或你有财力购买正版软件，也可忽视这一条。

**支持多种编程语言**，Vim是程序员的编辑器，当然对程序员是非常友好的。它对C,C++,Python,Perl,Tcl,Ruby,PHP等等，以及一大堆我没有听过见过的语言，以语法着色，代码缩进等基本支持，还有一些其他特性。比如，我在编辑XML时，它能提供自动封闭标记的支持。因此如果你有对多种格式的文本编辑需要，那么你就有了一个编辑的大平台，不需用再装一大堆针对某个格式特定的编辑器了。正如跨平台性一样，你只要一次投资，多次回报。如果你专注于某一格式文件的工作，那这一点同样对于你来说是没有用的。

**高效地编辑**，Vim的操作方式相对于Windows上呆久了的人来说，是蛮奇特的，这一点我深有体会。但是正如很多人讲的那样，你掌握了其操作后，发现它会大大增进你的编辑速度。你的双手根本不用离开键盘，就完成了许多事情，可以让鼠标歇会儿了。如果你特别钟爱鼠标，或只偶尔打打字，那么我说的这点，同样对你没有用。

**灵活的设置**，Vvim可自定义的地方太多了，你可以自定义键盘映射，语法着色，缩进，格式等等。所以你在网上可以看到许多人贴着自己的vimrc配置文件，配置着自己喜欢的作业环境。如果你需要开盒即用的工具，那么这点对你的吸引力就不大了。

安装可直接到[VIM官网](http://www.vim.org/download.php#pc)，选择Self-install executable形式的安装包下载安装。

# 帮助

帮助非常重要，VIM带有我认为非常好的帮助系统，可以获取你需要的任何有关VIM的详细信息。使用帮助非常简单,只需要:help安装即可。安装后程序带的是英文帮助，如果你对英文不是特别适应的话，可以去 http://vimcdoc.sourceforge.net/ 下载安装中文帮助，或像我一样直接使用在线的中文帮助 
http://vimcdoc.sourceforge.net/doc/help.html 。强烈推荐你好好阅读下这一手册，然后完成其中的30分钟教程，这些内容在"初步知识"中。这比你在网上狂搜相关的教程要好得多。再次啰嗦一次，Vim的帮助非常强大，教程也非常好,是你碰到问题时的第一选择。

# 操作方法

对于基本操作方法，通过Vim的教程，你应该能很好的掌握了。一些常见的设置，关于特定类型的配置，因人而异，不想多述。我会列出一些认为比较好的参考文章，置于文尾供参考。但在下面，我还是在Windows下的Vim的使用做点说明，或许你现在用不上。

# Vim文件夹结构

安装完Vim后，你在其安装目录下应有vim$ver($ver是版本号)和vimfiles两个文件夹。其中vim$ver是vim的程序运行时目录,在里面会看到gvim.exe(vim的GUI),vim.exe,xxd.exe等程序，一大堆的dll动态链接库,还有就是color(语法着色),doc(帮助说明),indent(缩进)等文件夹。在vimfiles内，也会看到color,doc,indent等类似的文件夹，但它们里面没有文件。vim$ver和vimfiles两者有什么区别呢，vim$ver是运行时文件目录，vimfiles相当于个人配置目录，常常有文章说在linux下将什么插件放进.vim下的plugin等等之类的，其.vim在windows下就相当于vimfiles。

# 标签页

Tabpage是Vim后增的功能，类似于UltraEdit的标签页。也想在Windows下使用Untraledit一样，在同一个VIM实例中打开多个文件的话，需要做些小修改。在注册表中删除"HKCR\*\shellex\ContextMenuHandlers\gvim\"主键，然后在Shell下新项"Vim 编辑",再在其下新建command项,然后修改其值为$vimruntime\gvim.exe -p --remote-tab-silent "%1"，其中$vimruntime修改为你系统中VIM实际运行目录。如果你不知道$vimruntime的值，可以打开gvim，输入:echo $vimruntime。你想双击关联文件，也在同一实例打开的话，查找注册表中gvim相关项，将$vimruntime\gvim.exe改为上述的值即可,主要是`HKLM\software\classes\application\gvim.exe\shell\edit\command`下的值。

# 文件编码

具体可参见"Vim实用技术:实用技巧"。我推荐内部编码使用utf-8，以支持国际化，即encoding=utf-8。这需要在_vimrc中进行设置，网上常有人启用这一选项后Vim菜单和消息出现乱码。据我的经验，需要将这encoding=utf-8写在_vimrc最开头，然后设置language message,可参见我的_vimrc文件。

# vimrc文件

Vim使用中，配置文件vimrc是非常重要的，用:echo $myvimrc，来查看你的vimrc在哪里。
如果这为空的话，你可以在$vim目录，建一新的_vimrc文件。

## 我的vimrc文件

```
set encoding=utf-8
set termencoding=gbk
set nocompatible          " We're running Vim
set nobackup		"We don't need the backup file
set showmatch		"Show where the bracket match
set showcmd
set ruler		"Show the line and column number 
set hlsearch		"Highlight the search key
set backspace=indent,eol,start
set fileencodings=ucs-bom,utf-8,chinese
set guifont=courier_new:h10
set autoindent
syntax on           " Enable syntax highlighting
filetype plugin indent on " Enable filetype-specific indenting and plugins
language message zh_CN.utf-8 " Use chinese message
color zellner		" Color theme
```

其中termencoding=gbk是因为windows中的“命令提示符”窗口只能使用gbk编码，不能像Gnome中的Console那样用utf-8。不设置的情况下，使用“命令提示符”下的vim，而不是gvim时，会出乱码。在设置文件中的色彩和字体，可以先在gvim菜单中设置，然后将你所喜好的，添加到_vimrc文件中。看到我的vimrc文件，你是不是感觉特别短。因为我把许多跟文件类型的相关设置放在其对应的脚本里，扔在vimfiles文件夹了。在vimrc里，例如常见的空格，制表符，缩进都没有在这配置。

# 杂项

Vim中一些内置的变量，你都可以通过:echo varname来查看值，比如：:echo $myvimrc
这些变量，注意大小写，常用的有
`$VIM` Vim的安装目录
`$vimruntime` Vim运行时目录
`$myvimrc` 用户的_vimrc文件

$home 用户的主目录
我常常使用`:e $myvimrc`来编辑我的vimrc文件，非常方便。

对一些带值的配置选项，你可以用`:set optionname`来查看其当前值，或用`:set optionname=val`来更改其值.比如:set fileformat查看文件格式，因为dos,unix,mac对于换行是不一样的。:set filemat=unix的话，换行将用LF，而不是dox\windows下的CR,LF。

# 参考链接

IBM开发中心非常实在的Vim实用技术系列：
[Vim实用技术(1)-实用技巧](http://www.ibm.com/developerworks/cn/linux/l-tip-vim1/)
[Vim实用技术(2)-常用插件](http://www.ibm.com/developerworks/cn/linux/l-tip-vim2/)
[Vim实用技术(3)-定制Vim](http://www.ibm.com/developerworks/cn/linux/l-tip-vim3/)

Easwy的博客，里面有用的信息，更多的Vim资源链接
[Vim专栏](http://blog.csdn.net/easwy/article/category/234641)


