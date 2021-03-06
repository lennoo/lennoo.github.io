---
title: Boost.Lambda是什么? 
date: 2008-5-18 16:03:00
---
# Boost.Lambda是什么?

Boost Lambda库是C++模板库,以C++语言实现了lambda抽象.Lambda这个术语来自函数编程语言和lambda闭包理论,lambda抽象实际上定义了匿名函数.了解过C#新引入的匿数函数特性或Lisp编程的人，对这些概念理解会有很大帮助.Lambda库设计的主要动机是为STL算法提供灵活方便的定义匿名函数对象的机制.这个Lambda库究竟是有什么用呢?代码胜千言!看下面将STL容器中的元素打印到标准输出上的代码.

<!-- more -->

```
for_each(a.begin(), a.end(), std::cout << _1 << ' ');
```

表达式std::cout << _1 << ' '定义了一元函数对象.变量_1是函数的形参,是实参的占位符.每次for_each的迭代中,函数带着实际的参数被调用,实际参数取代了占位符,然后函数体里的内容被执行.Lambda库的核心就是让你能像上面所展示的那样,在STL算法的调用点,定义小的匿名函数对象.

# Lambda库的安装

Lambda库只由头文件组成,这就意味着你不需要进行任何编译,连接,生成二进制库的动作,只需要boost库头文件路径包含进你的工程中即可使用.

与现代的C++语言一样,在使用时你需要声明用到的名字空间,把下列的代码包含在你的源文件头:

```
using namespace boost::lambda;
```

# Boost Lambda库的动机

在标准模板库STL成为标准C++的一部分后,典型的STL算法对容器中元素的操作大都是通过函数对象(function objects)完成的.这些函数作为实参传入STL算法.

任何C++中以函数调用语法被调用的对象都是函数对象.STL对某些常见情况预置了些函数对象.比如:plus,less,not1下面就是标准plus模板的一种可能实现:

```
template <class T> 
struct plus : public binary_function <T, T, T> {
  T operator()(const T& i, const T& j) const {
    return i + j; 
  }
};
```

基类binary_function<T, T, T>包含了参数和函数对象返回类型的类型定义,这样可使得函数对象可配接.

除了上面提到的基本的函数对象外,STL还包含了binder模板,将可配接的二元函数中的某个实参固定为常量值,来创建一个一元函数对象.比如:

```
class plus_1 {
  int _i;
public:
  plus_1(const int& i) : _i(i) {}
  int operator()(const int& j) { return _i + j; }
};
```

上面的代码显性地创建了一个函数对象,将其参数加1.这样的功能可用plus模板与binder模板(bind1st来等效地实现.举例来说,下面的两行表达式创建了一个函数对象,当它被调用时,将返回1与调用参数的和.

```
plus_1(1)
bind1st(plus<int>(), 1)
plus<int>
```

就是计算两个数之和的函数对象.bind1st使被调用的函数对象的第一个参数绑定到常量1.作为上面函数对象的使用示例,下面的代码就是将容器a中的元素加1后,输出到标准输出设备:

```
transform(a.begin(), a.end(), ostream_iterator<int>(cout),
          bind1st(plus<int>(), 1));
```

为了使binder更加通用,STL包含了适配器(adaptors)用于函数引用与指针,以及成员函数的配接.
所有这些工具都有一个目标,就是为了能在STL算法的调用点有可能指定一个匿名的函数,换句说,就是能够使部分代码片断作为参数传给调用算法函数.但是,标准库在这方面只做了部分工作.上面的例子说明用标准库工具进行匿名函数的定义还是很麻烦的.复杂的函数调用表达式,适配器,函数组合符都使理解变得困难.另外,在运用标准库这些方法时还有明显的限束.比如,标准C++98中的binder只允许二元函数的一个参数被绑定,而没有对3参数,4参数的绑定.这种情况在TR1实施后,引进了通用的binder后可能改善,对于使用MSVC的程序员,有兴趣还可以查看下微软针对VS2008发布的TR1增强包.

但是不管怎样,Lambda库提供了针对这些问题比较优雅的解决方法:

* 对匿名函数以直观的语义进行创建,上面的例子可改写成:

```
transform(a.begin(), a.end(), ostream_iterator<int>(cout), 
          1 + _1);
```

更直观点:

```
for_each(a.begin(), a.end(), cout << (1 + _1));
```

* 绝大部分对函数参数绑定的限制被去除,在实际C++代码中可以绑定任意的参数

* 分离的函数组合操作不再需要了,函数组合被隐性地支持.

# Lambda表达式介绍

Lambda表达在函数式编程语言中很常见.在不同语言中,它们的语法有着很大不同,但是lambda表达式的基本形式是:

```
lambda x1...xn.e
```

lambda表达式定义了匿名函数,并由下列的元素组成

* 函数的参数:x1...xn
* 表达式e,以参数x1...xn的形式计算函数的值

一个简单的lambda表达式的例子是:

```
(lambda x y.x+y) 2 3 = 2 + 3 = 5 
```

在lambda表达式的C++版本中,表达式中x1...xn不需要,已预定义形式化的参数.在现在Boost.Lambda库中,存在三个这样的预定义的参数,叫做占位符:_1,_2,和_3.它们分别指代在lambda表达式中的第一,二,三个参数.比如,下面这样的lambda表达式:

```
lambda x y.x+y
```

C++定义的形式将会是这样:

```
_1 + _2
```

因此在C++中的lambda表达式没有语义上所谓的关键字.占位符作为运算符使用时就隐性地意味着运算符调用是个lambda表达式.但是只有在作为运算符调用才是这样.当Lambda表达式包含函数调用,控制结构,转换时就需要特殊的语法调用了.更为重要的是,作为函数调用是需封装成binder函数的形式.比如,下面这个lambda表达式:

```
lambda x y.foo(x,y)
```

不应写成foo(_1,_2),对应的C++结构应如下:
```
bind(foo, _1, _2)
```
对于这种表达式,更倾向于作为绑定表达式bind expressions

lambda表达式定义了C++的函数对象,因此,对于函数调用的形式跟其他的函数对象一样,比如:(_1 + _2)(i, j).

# 性能
性能,运行效率,总是C++程序员关心的话题.理论上,相对于手写循环代码,使用STL算法和Lambda函数对象的所有运行开销,可以通过编译优化消除掉.这种优化取决于编译器,实际中的编译器大都能做到.测试表明,性能会有下降,但是影响不大,对于代码的效率和简洁之间的权衡,只能由程序员自己做出判断了.

Lambda库的设计与实现中大量运用了模板技术,造成对于同一模板需要大量的递归实例化．这一因素可能使构建复杂逻辑的lambda表达式，不是一个非常理想的做法．因为编译这些表达式需要大量的内存，从而使编译时间变得非常慢，这在一些大型项目中会更加突出．还有在发生编误错误时，引发的大量错误信息，不能有效地指出真正错误之处．最后点，C++标准建议模板的嵌套层次不要超过17层来防止导致无限递归,而复杂的Lambda表达式模板会很容易超过这一限制．虽然大多数编译器允许更深层次的模板嵌套，但是通常需要显性地传入一个命令行参数才能做到．

# 参考

大多数内容是从[Boost.Lambday](http://www.boost.org/doc/libs/1_35_0/doc/html/lambda.html)库在线文档参考翻译而成
