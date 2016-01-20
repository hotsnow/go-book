.. include:: common.txt

Konnichiwa, World!
******************
在开始使用 Go 写一个和 wized 一样的应用之前，我们先从一些非常基础的材料开始。
如果你连翅膀是什么都不知道的时候你是不可能建一架飞机的！
因此在这章节中我们会从一些最简单基础的语法开始试试水。

程序
===========
按照惯例。我们也推崇一些惯例[#f1]_.
一个人学习编程语言后写的第一个程序就是简单的打印出一句：``Hello World``。

Ready? Go!

.. code-block:: go
    :linenos:

    package main

    import "fmt"

    func main() {
        fmt.Printf("Hello, world; καλημ ́ρα κóσμ or こんにちは世界\n")
    }

Output:

.. container:: output

    | Hello, world; καλημ ́ρα κóσμ or こんにちは世界

细节
===========

包的概念
----------------------
Go 程序师由"包"构成的，这些包也可能引用其他的包。
A Go program is constructed as a "package", which may in turn use facilities
from other packages.

声明``package <something>``（在我们的例子中：``<something>`` 就是 main）就是说这个文件是属于哪个包。

为了在屏幕中输出"Hello, World..."，我们使用了``fmt``这个包中的``Printf``这个函数，
其中``fmt``这个包就是在第三行的声明中引入的``import "fmt"``。

.. 注意::
    每一个独立的 Go 程序都包括一个叫做 main 的包
    其中它的 main 函数就是程序初始化后开始执行的地方。
    main.main 函数没有参数也没有返回值。

这实际上和 perl 及 python 模块的做法是一样的。和你看到的一样，这里最大的优势就是：
模块化和可重用性

这里说的*模块化*，我的意思是你可以按照你的实际需求把程序分开成几个片段，每段实现一部分特定功能。
这里说的*可重用性*，我意思是你可以重复多次使用你所写的或者 Go 本身自带的包，而不必要每次都重写。

从现在开始，你可以只把包想象成一个对用户透明的用法或者语法优点。
后面我们会再来看如何创建我们自己的包。
For now, you can just think of packages from the perspective of their *rasion
d'être* and advantages. Later we will see how to create our own packages.

main 函数
-----------------
在第5行，我们定义了我们的 man 函数，这个函数式使用关键字``func``以及使用``{``和``}``闭合的主体
构成，这个和 C，C++，Java 以及其他编程语言是一样的。

注意我们的``main()``函数是没有参数的。
但后面我们会学习到带有一个参数，返回零个、一个或者多个值的函数。

在第6行中，我们调用了一个叫``Printf``的函数，这个函数是在第3行导入的``fmt``包中定义的。

这里的``点引用`` 在 Go 中是表示数据引用或者指定包的某个函数。
再次说明，这个并不是 Go 的发明，这个技术已经被用在其他编程语言中，例如 Python：
``<module_name>.<data_or_function>``
The ``dot notation`` is what is used in Go to refer to data, or functions
located in imported packages. Again, this is not a Go's invention, since this
technique is used in several other languages such as Python:
``<module_name>.<data_or_function>``

UTF-8 Everywhere!
-----------------
注意我们使用``Printf``输出了非 ASCII 码的字符（ 希腊语和日语）。
实际上，Go 原生支持 UTF-8 字符串和标识符。

总结
==========
Go 使用包（和 python 模块一样）去组织代码。
函数``main.main``（main 包中的 main 函数）是所有独立 Go 程序的入口。
Go 使用 UTF-8 编码的字符串和标识符而不仅仅是老旧的 ASCII 字符集。


.. external links and footnotes:

.. [#f1] It is said that the first "hello, world" program was written by Brian
    Kernighan in his tutorial "Programming in C: A Tutorial" at Bell Labs in
    1974. And there is a "hello, world" example in the seminal book "The C
    programming Language" much often refered to as "K&R" by Brian Kernighan and
    Dennis Ritchie.
