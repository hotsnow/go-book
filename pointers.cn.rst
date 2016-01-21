.. include:: common.txt

指针和内存
*******************
很多人认为指针是个*高级*的主题。
说实话，你越觉得他们高级，你就越害怕他们并且理解得越少。
你不是一个笨蛋，让我来证明给你看。

当你在程序中定义一个变量的时候，你知道这个变量在内存（RAM）中占有一段位置，对吧？
因为当你使用、修改这个变量的值的时候，它肯定是在某个地方的。

这里的*某个地方*就是变量在内存中的*地址*。

.. graphviz::

    digraph ram {
        rankdir=LR;
        graph [bgcolor=transparent, resolution=96, fontsize="10"];
        edge [arrowsize=.5, arrowhead="vee", color="#ff6600", penwidth=.4];
        node [shape=record, fontsize=8, height=.2, penwidth=.4]
        ram[label="<fs>...|<f0>i|<f1>ok|<f2>hello|<f3>...|<f4>f|<fe>..."];
        node[shape="oval", fixedsize="false"]
        exnode0 [label="var i int"];
        exnode1 [label="var ok bool"];
        exnode2 [label="var hello string"];
        exnode4 [label="var f float32"];
        exnode0->ram:f0;
        exnode1->ram:f1;
        exnode2->ram:f2;
        exnode4->ram:f4;
        RAM[label="This is your RAM", shape=plaintext, fontcolor=blue];
        RAM->ram:n [color=blue, style=dotted];
    }

现在，你买的内存都是以 GB（Giga Bytes）来计算的，这就是你的程序及其变量可以用的*bytes*数。
你猜猜看，字符串"hello, world"和一个简单整形占用的内存数还不一样呢。

例如，变量``i``占用了4 个字节的内存，从第100个字节到103个字节。
字符串``"Hello world"``会占用更多地内存，从第105个字节到116个字节。

变量所占空间的第一个字节的数值就叫做这个变量的地址。

我们使用``&``操作符来找出给定变量的地址。

.. code-block:: go
    :linenos:

    package main

    import "fmt"

    var (
        i int = 9
        hello string = "Hello world"
        pi float32 = 3.14
        c complex64 = 3+5i
    )

    func main() {
        fmt.Println("Hexadecimal address of i is: ", &i)
        fmt.Println("Hexadecimal address of hello is: ", &hello)
        fmt.Println("Hexadecimal address of pi is: ", &pi)
        fmt.Println("Hexadecimal address of c is: ", &c)
    }

Output:

.. container:: output

    | Hexadecimal address of i is:  0x4b8018
    | Hexadecimal address of hello is:  0x4b8108
    | Hexadecimal address of pi is:  0x4b801c
    | Hexadecimal address of c is:  0x4b80b8

看到吗？每个变量都由自己的地址，你可以猜测到变量的地址是和其他变量所占的内存有依赖关系的。

那些*存储* 其他变量地址的变量我们就叫做*指针*变量.

如何定义指针？
=========================
对于给定的类型``T``，这里有相关联的``*T``指针指向它的变量类型``T``。

例如:

.. code-block:: go
    :linenos:

    var i int
    var hello string
    var p *int //p is of type *int, so p is a pointer to variables of type int

    //we can assign to p the address of i like this:
    p = &i //now p points to i (i.e. p stores the address of i
    hello_ptr := &hello //hello_ptr is a pointer variable of type *string and it points hello


如何获取指针所指向的变量的值？
=================================================

如果``Ptr``是指向``Var``的指针，那么``Ptr == &Var``, ``*Ptr == Var``.

换句话说：
你在变量前放置``&``操作去获取变量的地址（一个指针指向它），
你在变量前放置``*``操作去获取变量所指向值。这也叫做：*解引用*

.. admonition:: Remember

    * ``&`` is called the address operator.
    * ``*`` is called the dereferencing operator.

.. code-block:: go
    :linenos:

    package main
    import "fmt"

    func main() {
        hello := "Hello, mina-san!"
        //declare a hello_ptr pointer variable to strings
        var hello_ptr *string
        // make it point our hello variable. i.e. assign its address to it
        hello_ptr = &hello
        // and int variable and a pointer to it
        i := 6
        i_ptr := &i //i_ptr is of type *int

        fmt.Println("The string hello is: ", hello)
        fmt.Println("The string pointed to by hello_ptr is: ", *hello_ptr)
        fmt.Println("The value of i is: ", i)
        fmt.Println("The value pointed to by i_ptr is: ", *i_ptr)
    }

Output:

.. container:: output

    | The string hello is:  Hello, mina-san!
    | The string pointed to by hello_ptr is:  Hello, mina-san!
    | The value of i is:  6
    | The value pointed to by i_ptr is:  6

.. graphviz::

    digraph ram {
        rankdir=LR;
        graph [bgcolor=transparent, resolution=96, fontsize="10" ];
        edge [arrowsize=.5, arrowtail="dot", color="#ff6600", penwidth=.4];
        node [shape=box, fontsize=8, height=.1, penwidth=.4]
        i [label="var i int"];
        i_ptr [label="var i_ptr *int", shape="oval"];
        hello [label="var hello string"];
        hello_ptr [label="var hello_ptr *string", shape="oval"];
        i_ptr->i;
        hello_ptr->hello;
    }


这就是所有了！指针就是存储其他变量地址的变量，如果一个变量的类型是``int``，那么它的指针就是``*int``,
如果变量类型是``float32``，那么它的指针就是``*float32``如此类推...

为什么我们需要指针？
========================
因为我们可以使用变量，只用变量名字就可以赋值，有人也许会想知道为什么指针很有用。
这是一个非常合理地问题，你将看到一个非常简明的回答。

假设你的程序的运行需要把一些结果存储到一些没有定义的变量里。如何来实现呢？
你也许会说：我可以先预先定义一些变量。
但是如果变量的数量在每次程序运行过程中都不一样呢？

答案就是*运行时分配*：你的程序可以在运行过程中*分配*一些内存去存储数据。

.. _function-new:

Go 有一个内置的分配函数叫做``new``,它分配确切的内存空间给需要存储的给定类型的变量值，
当内存创建完成后，它将返回一个指针。

用法如下：``new(Type)``, 这里``Type``就是你需要使用的变量类型。

 这里有一个解释``new``函数的例子。

.. code-block:: go
    :linenos:

    package main
    import "fmt"

    func main() {
        sum := 0
        var  double_sum *int //a pointer to int
        for i:=0; i<10; i++{
            sum += i
        }
        double_sum = new(int) //allocate memory for an int and make double_sum point to it
        *double_sum = sum*2 //use the allocated memory, by dereferencing double_sum
        fmt.Println("The sum of numbers from 0 to 10 is: ", sum)
        fmt.Println("The double of this sum is: ", *double_sum)
    }

.. container:: output

    | The sum of numbers from 0 to 10 is:  45
    | The double of this sum is:  90

另外一个好的理由是，我们将会在学习函数的时候看到，往函数传递参数的引用在很多时候都是非常有用的。
但这时另外的课题了。到时候，我希望你能理解指针的概念，但不要担心，我们将会在下一章中逐渐使用指针。
那时你将看到那些人害怕指针是真得没必要的。
