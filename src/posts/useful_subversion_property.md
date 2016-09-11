---
title: 好用的Subversion属性功能
date: 2008-6-30 12:06:08
---
Subversion的属性是非常好用的功能,它将一些工作自动化，实现为受版本控制的源文件添加元信息的作用。属性是外部不可见的，可以简单认为是附加上在文件上的信息，和文件大小之类的信息是一样的，只不过他是通过subversion来管理的。属性的名称和值可以是你希望的任何值，限制就是名称必须是可读的文本，并且最好的一点是这些属性也是版本化的，就像你的文本文件内容，你可以像提交文本修改一样修改、提交和恢复属性修改，当你更新时也会接收到别人的属性修改—你不必为适应属性改变你的工作流程。

<!-- more -->

Subversion保留了一组名称以svn:开头的属性，来预定义一些有用的功能。比如你常会看到一些人的源代码底部有像下面之类标识的文字：

`$Id: main_window.py 68 2008-06-30 02:05:05Z Len $`

这就使用了Subversion 中的 svn:keywords的自动属性，它让将发生在源代码中的一些属性的变化自动地更新到源代码中。这行字的意思是表示，main_windows.py 这个源代码文件最后被用户 len 更新于 2008-6-30 02:05:05Z，修订版本号为 68。要实现这样的自动更新，你只要对需要这样属性的文件上使用下面这行指令。

```
> svn propset svn:keywords "Id" main_window.py
```
或者使用TortoiseSVN中的Properties的操作按钮，方便地增加新的属性。接着需要在源代码文件中需要 Subversion 进行自动更新的地方插入 $Id$ 这样的 Keyword，那么在你下次进行提交更新时，该$Id$ 就会被 Subversion 自动替换为$Id: main_window.py 68 2008-06-30 02:05:05Z Len $ 这样的格式。 
Subversion 中可以使用的Keyword 包括下面这些：

* Id 
上面介绍过的综合的格式
* LastChangedDate 
最后被修改的时间，缩写为 Date。
* LastChangedBy 
最后修改该源代码文件的用户名，缩写为 Author。
* LastChangedRevision 

最后修订的版本号，缩写为 Revision。
如果想每次向Subversion服务器提交文件修改时，都要设置文件的属性，则需要进行Subversion配置的修改。配置文件在你用户的主目录下，在Windows下应类似于C:\Documents and Settings\Len\Application Data\Subversion\config文件，Len是Windows用户名，注意Application Data是隐藏文件夹，需要显示全部文件才能看到。接着如下相应的修改，对你想要处理的文件做配置。
```
enable-auto-props = yes   
[auto-props]   
*.c = svn:keywords=Id   
*.py = svn:keywords=Id 
```
对于开源项目，常见其源文件头部有着版权声明的文本，这些操作大多也是通Subversion的属性功能来完成的，有关更详细的介绍和操作指南，可参见Subversion中文手册中的属性章节。
