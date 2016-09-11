---
title: Python类、模块、包
date: 2008-7-24 19:42:08
---
Python在处理功能复用和功能颗粒度划分时采用了类、模块、包的结构。这种处理跟C++中的类和名字空间类似，但更接近于Java所采用的概念。
<!-- more -->

# 类
类的概念在许多语言中出现，很容易理解。它将数据和操作进行封装，以便将来的复用。

# 模块
模块，在Python可理解为对应于一个文件。在创建了一个脚本文件后，定义了某些函数和变量。你在其他需要这些功能的文件中，导入这模块，就可重用这些函数和变量。一般用module_name.fun_name，和module_name.var_name进行使用。这样的语义用法使模块看起来很像类或者名字空间，可将module_name 理解为名字限定符。模块名就是文件名去掉.py后缀。下面演示了一个简单的例子：

```
#moduel1.py
def say(word):
    print word

#caller.py
import module1

print __name__
print module1.__name__
module1.say('hello')
```
```
$ python caller.py
__main__
module1
hello
```
例子中演示了从文件中调用模块的方法。这里还展示了一个有趣的模块属性__name__，它的值由Python解释器设定。如果脚本文件是作为主程序调用，其值就设为__main__，如果是作为模块被其他文件导入，它的值就是其文件名。这个属性非常有用，常可用来进行模块内置测试使用，你会经常在一些地方看到类似于下面的写法，这些语句只在作为主程序调用时才被执行。
```
if __name__ == '__main__':
    app = wxapp(0)
    app.MainLoop()
```
# 模块搜索路径

上面的例子中，当module1被导入后，python解释器就在当前目录下寻找module1.py的文件，然后再从环境变量PYTHONPATH寻找，如果这环境变量没有设定，也不要紧，解释器还会在安装预先设定的的一些目录寻找。这就是在导入下面这些标准模块，一切美好事情能发生的原因。
```
import os
import sys
import threading
...
```

这些搜索目录可在运行时动态改变，比如将module1.py不放在当前目录，而放在一个冷僻的角落里。这里你就需要通过某种途径，如sys.path，来告知Python了。sys.path返回的是模块搜索列表，通过前后的输出对比和代码，应能理悟到如何增加新路径的方法了吧。非常简单，就是使用list的append()或insert()增加新的目录。
```
#module2.py
import sys
import os

print sys.path
workpath = os.path.dirname(os.path.abspath(sys.argv[0]))
sys.path.insert(0, os.path.join(workpath, 'modules'))
print sys.path
$ python module2.py
['e:\\Project\\Python', 'C:\\WINDOWS\\system32\\python25.zip', ...]
['e:\\Project\\Python\\modules', 'e:\\Project\\Python', 'C:\\WINDOWS\\system32\\python25.zip', ...]
```

# 其他的要点

模块能像包含函数定义一样，可包含一些可执行语句。这些可执行语句通常用来进行模块的初始化工作。这些语句只在模块第一次被导入时被执行。这非常重要，有些人以为这些语句会多次导入多次执行，其实不然。

模块在被导入执行时，python解释器为加快程序的启动速度，会在与模块文件同一目录下生成.pyc文件。我们知道python是解释性的脚本语言，而.pyc是经过编译后的字节码，这一工作会自动完成，而无需程序员手动执行。

# 包
在创建许许多多模块后，我们可能希望将某些功能相近的文件组织在同一文件夹下，这里就需要运用包的概念了。包对应于文件夹，使用包的方式跟模块也类似，唯一需要注意的是，当文件夹当作包使用时，文件夹需要包含__init__.py文件，主要是为了避免将文件夹名当作普通的字符串。__init__.py的内容可以为空，一般用来进行包的某些初始化工作或者设置__all__值，__all__是在from package-name import *这语句使用的，全部导出定义过的模块。


