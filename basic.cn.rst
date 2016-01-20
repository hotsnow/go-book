.. include:: common.txt

最基础的东西
****************
在之前的章节，我们知道 Go 程序是由包构成的，Go 原生支持 UTF-8字符串和标识符。
在这个章节里，我们看看如何定义和使用变量、常量，看看 Go 内置类型的区别。

如何定义一个变量？
==========================
Go 有几种定义变量的方法。

基本形式如下:

.. code-block:: go

    // declare a variable named "variable_name" of type "type"
    var variable_name type

你可以在一行里面同时定义几个同一个类型的变量，只需要用逗号分隔开。

.. code-block:: go

    // declare variables var1, var2, and var3 all of type type
    var var1, var2, var3 type

你也可以在定义变量的同时做变量初始化。

.. code-block:: go

    /* declare a variable named "variable_name" of type "type" and initialize it
        to value*/
        var variable_name type = value

你甚至可以在同一个声明中同时初始化多个被定义的变量

.. code-block:: go

    /* declare a var1, var2, var3 of type "type" and initialize them to value1,
    value2, and value3 respectively*/
    var var1, var2, var3 type = value1, value2, value3

猜猜会什么情况？你可以在不指定类型，因为他们会在初始化的时候被推断出正确地类型

.. code-block:: go

    /* declare and initialize var1, var2 and var3 and initialize them
    respectively to value1, value2, and value3. /
    var var1, var2, var3 = value1, value2, value3

甚至可以更短，在函数内部（我们再重复一次：仅仅在函数内部）你甚至可以把``var``去掉，
这时要使用``:=``而不是``=``

.. code-block:: go

    // omit var and type, and use ':=' instead of '=' inside the function body
    func test(){
        var1, var2, var3 := value1, value2, value3
    }

不用担心。这实际上非常简单。下面的包含内置类型的例子，会举例子说明所有的这些情况。
只要记住，和 C 的方法不一样， Go 的类型是放在定义的最后的，
需要再重复一遍吗？** := 操作符仅仅适用了函数内部**。

.. _primitive-types:

内置类型
=================

布尔
-------
对于布尔类型的值，Go 有用``bool``类型表示（就像 C++ 一样），他的值可以是:``true``或者``false``。
For boolean truth values, Go has the type ``bool`` (like the C++ one) that takes
one of the values: ``true`` or ``false``.

.. code-block:: go
    :linenos:

    //Example snippet
    var active bool //basic form
    var enabled, disabled = true, false //type omitted, variables initialized
    func test(){
        var available bool //general form
        valid := false //type and var omitted, and variable initialized
        available = true //normal assignation
    }


数值型
-------------
对于整形数值，可分为有符号和无符号数，Go 使用``int``和``uint``表示，他们都根据你的机器（是32位
还是64位） 来表示，但你也可以明确指定数值长度：``int8``, ``int16``, ``int32``, ``int64``
以及 ``byte``, ``uint8``, ``uint16``, ``uint32``, ``uint64``。 其中``byte``和``uint8``是一样的。

对于浮点型，可分为``float32``和``float64``.

这还不是全部类型，Go 原生支持复数！
实际上，你可以用``complex64``表示一个具有 32 bits 的实数部分和 32 bits 的虚数部分的数，
同样我们还可以用``complex128``表示一个具有 64 bits 的实数部分和 64 bits 的虚数部分的数，

数值类型的对应表格
^^^^^^^^^^^^^^^^^^^^^^

From the `Go Programming Language Specification`_

+----------+----------------------------------------------------------------------------------------+
|Type      | Values                                                                                 |
+==========+========================================================================================+
|uint8     | the set of all unsigned  8-bit integers (0 to 255)                                     |
+----------+----------------------------------------------------------------------------------------+
|uint16    | the set of all unsigned 16-bit integers (0 to 65535)                                   |
+----------+----------------------------------------------------------------------------------------+
|uint32    | the set of all unsigned 32-bit integers (0 to 4294967295)                              |
+----------+----------------------------------------------------------------------------------------+
|uint64    | the set of all unsigned 64-bit integers (0 to 18446744073709551615)                    |
+----------+----------------------------------------------------------------------------------------+
|                                                                                                   |
+----------+----------------------------------------------------------------------------------------+
|int8      | the set of all signed  8-bit integers (-128 to 127)                                    |
+----------+----------------------------------------------------------------------------------------+
|int16     | the set of all signed 16-bit integers (-32768 to 32767)                                |
+----------+----------------------------------------------------------------------------------------+
|int32     | the set of all signed 32-bit integers (-2147483648 to 2147483647)                      |
+----------+----------------------------------------------------------------------------------------+
|int64     | the set of all signed 64-bit integers (-9223372036854775808 to 9223372036854775807)    |
+----------+----------------------------------------------------------------------------------------+
|                                                                                                   |
+----------+----------------------------------------------------------------------------------------+
|float32   | the set of all IEEE-754 32-bit floating-point numbers                                  |
+----------+----------------------------------------------------------------------------------------+
|float64   | the set of all IEEE-754 64-bit floating-point numbers                                  |
+----------+----------------------------------------------------------------------------------------+
|                                                                                                   |
+----------+----------------------------------------------------------------------------------------+
|complex64 | the set of all complex numbers with float32 real and imaginary parts                   |
+----------+----------------------------------------------------------------------------------------+
|complex128| the set of all complex numbers with float64 real and imaginary parts                   |
+----------+----------------------------------------------------------------------------------------+
|                                                                                                   |
+----------+----------------------------------------------------------------------------------------+
| byte        familiar alias for uint8                                                              |
+----------+----------------------------------------------------------------------------------------+


.. code-block:: go
    :linenos:

    //Example snippet
    var i int32 //basic form with a int32
    var x, y, z = 1, 2, 3 //type omitted, variables initialized
    func test(){
        var pi float32 //basic form
        one, two, three := 1, 2, 3 //type and var omitted, variables initialized
        c := 10+3i // a complex number, type infered and keyword 'var' omitted.
        pi = 3.14 // normal assignation
    }

字符串
-------
和前面章节看到的一样，字符串是 UTF-8 编码的，并且被双引号（"） 引住的，他的类型用``string``表示

.. code-block:: go
    :linenos:

    //Example snippet
    var french_hello string //basic form
    var empty_string string = "" // here empty_string (like french_hello) equals ""
    func test(){
        no, yes, maybe := "no", "yes", "maybe" //var and type omitted
        japanese_hello := "Ohaiou"  //type inferred, var keyword omitted
        french_hello = "Bonjour" //normal assignation
    }

常量
=========
在 Go 里面，常量类型的值实在编译阶段创建的，常量可以是：数值，布尔或者是字符串。

定义常量的语法如下：

.. code-block:: go

    const constant_name = value

一些列子:

.. code-block:: go
    :linenos:

    //example snippet
    const i = 100
    const pi = 3.14
    const prefix = "go_"

Facilities
==========

分组定义
---------------------
多个的``var``, ``const`` 和 ``import``定义可以使用圆括号合在一起。

一些有趣的写法:

.. code-block:: go
    :linenos:

    //example snippet
    import "fmt"
    import "os"

    const i = 100
    const pi = 3.14
    const prefix = "go_"

    var i int
    var pi = float32
    var prefix string

我们可以这样写:

.. code-block:: go
    :linenos:

    //example snippet with grouping
    import(
        "fmt"
        "os"
    )

    const(
        i = 100
        pi = 3.14
        prefix = "go_"
    )

    var(
        i int
        pi = float32
        prefix string
    )


*当然*，你使用 consts 来把 consts 分组，使用 vars 来把 vars 分组，使用 imports 来把 imports 分组。
你不能把他们合并到一起来分组！

iota and enumerations
---------------------
Go 提供一个关键字``iota``用来定义枚举常量，
这个关键字同时也是从0开始，然后每次使用后都递增1。

例子:

.. code-block:: go
    :linenos:

    //example snippet
    const(
        x = iota //x == 0
        y = iota //y == 1
        z = iota //z == 2
        w // implicitely w == iota, therefore: w == 3
    )


好了，这就是这章的内容。我和你说过，这不会很难。实际上，Go 变量的定义是非常简单。
你几乎可以像写 Python 脚本一样编写代码 -- 甚至做得更好。

.. external links and footnotes:

.. _Go Programming Language Specification: http://golang.com/doc/go_spec.html#Numeric_types
