.. include:: common.txt

如果你不是一个程序员
**************************
这一章是为那些从未试用过命令式编程语言写过程序的人员准备的。
如果你试用过诸如 C，C++，Python，Perl，Ruby 之类的编程语言，
或者你知道变量、常量、类型、指令这些特定的含义，那么你应该（不，你要）完整的跳过这个章节。


程序是怎么运作的?
=====================
在程序的世界里有多重熟悉的语言：函数式，逻辑式，命令式。

Go 和 C，C++，Python，Perl 以及其他的一些被称作命令式语言，因为程序按你的写法去执行。
它用命令的方式去描述一个任务去让计算机执行。

一个命令式程序就是处理可执行的一系列的 *指令* ，当程序被以处理器可理解的方式编译及 *指令* 成二进制程序。
An imperative program is a sequence of *instructions* that the processor
executes when the program is compiled and *transformed* to a binary form that
the processor understands.

可以想象成构建某物的一系列的步骤集合。
我们这里说的 *一系列* 是因为这里步骤的 **顺序** 一般来说非常重要。
就像你在建好一面墙之前是无法刷墙面一样，明白不？顺序很重要
Think of it as a list of steps to assemble or build something. We said
*"sequence"* because the **order** of these steps generally matters a LOT! You
can't paint the walls of a house when you haven't built the walls yet. See?
Order matters!

什么是变量?
===================
当你解数据题的时候（注意现在不会真的在讨论数学题），例如你说设``x``的值为``1``。

有时候你解方程式：``2x + 1 = 5``。解这个方程式就是要求``x``的值。

因此可以说``x``是一个容纳不同特定类型数值的容器。

可以把他想象成一个贴有*identifiers*标签的存贮这个值得箱子。

什么是赋值？
====================
我们把一个值赋给一个变量，就是说这个变量和这个值相等，直到另一次赋值把这个值给改变。

.. code-block:: go
    :linenos:

    //assign the value 5 to the variable x
    x = 5
    // Now x is equal to 5. We write this like this: x == 5
    x = x + 3 //now, we assign to x its value plus 3
    //so now, x == 8

!!!zzz!!!
在上面的代码片段中，我们把数值``5``赋值给那个定义为``x`` 的变量（简称为变量``x``），
接着我们又把变量``x``本身的值（这里就是上面赋值的``5``了）加``3`` 后赋值给``x``。
现在 x 就就是``5 + 3`` 也就是``8``。
Assignation is the act of *storing* values inside variables. In the previous
snippet, we assigned the value ``5`` to the variable whose identifier is ``x``
(we say simply the variable ``x``) and just after, we assigned to the variable
``x`` its own value (``5`` is what we assigned to it in the previous
instruction) plus ``3``. So now, x contains the value ``5+3`` which is ``8``.

.. graphviz::

    digraph variable_use {
        //rankdir=LR;
        graph [bgcolor=transparent, resolution=96, fontsize="10"];
        edge [arrowsize=.5, arrowhead="vee", color="#ff6600", penwidth=.4];
        node [shape=box, fontsize=8, height=.2, penwidth=.4]
        value[shape="circle", label="5", fixedsize="false"]
        value2[shape="circle", label="x+3", fixedsize="false"]
        variable[label="x"];
        variable2[label="x"];
        value->variable
        value2->variable2
        {rank=sink; variable2 value2}
    }

什么是变量类型？
===============
每个被定义的变量都有给定的数据类型。变量类型就是这个变量可以存贮的一组特定值。
 换句话说：变量类型就是一组可以被赋值给一个变量的值。

例如：一个``int``类型变量可以存贮整形值：0, 1, 2, ...

*""*这个短语可以赋值给一个``string``类型的变量。

因此我们实际上定义一个给定的类型的变量就是为了去使用这个类型的值。

什么事常量？
===================
常量就是一个给定特定数值的标识，但这个数值是不可以被你的程序所修改的。

我们给常量定义一个给定的数值。

.. code-block:: go
    :linenos:

    // Pi is a constant whose value is 3.14
    const Pi = 3.14

    // Let's use the constant Pi
    var x float32 //we declare a variable x of type float32
    x = 3 * Pi //when compiled, x == 3 * 3.14

    var y float32
    y = 4 * Pi //when compiled, y == 4 * 3.14

例如，我们可以定义一个常量``Pi``的值为``3.14``。在程序运行期间，``Pi``这个值不会被修改。
这里没有修改常量数值的 办法。

常量是的程序更容易阅读和维护。实际上 在你的程序中写``Pi``而不是写``3.14``会让你的程序更容易阅读。

 同样，如果你决定让我们程序中得``Pi``更明确，我们可以修改常量定义中值为``3.14159``而不是之前的``3.14``，
这样当程序被重新编译的时候，新的值就会在整个程序中生效。

结尾
==========
 命令式程序师很简单的.简单到就想把一个变量放入箱子中一样。
一旦你明白这个最基础的东西，复杂的（ 注意这里不是说困难的）的东西就可以看做是简单东西的组合。

简单好玩。像搭建乐高一样开心玩耍:)
Easy and fun. Fun as playing with Lego sets :)
