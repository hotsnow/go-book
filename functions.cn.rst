.. include:: common.txt

函数，为什么及如何做
Functions, why and how
**********************
在前面的两个章节中，我们看到基础的数据类型和基础的控制结构。
感觉上他们在一些基础的需求中是有用的，
当需求或者问题变得越来越复杂的时候，你就会感觉到需要复杂的数据和控制结构。

问题
===========
例如我们有一个需求要写一个程序返回两个整形数值中的最大者。
这个是非常简单的，我们只需要写一个*if*语句就可以了，对吧？

.. code-block:: go
    :linenos:

    //compare A and B and say wich is bigger
    if A > B {
        fmt.Print("The max is A")
    } else {
        fmt.Print("The max is B")
    }

现在设想一下我们的程序中需要多次使用上面的比较大小的代码。
每次都重复写一次上面的代码是非常讨厌的，并且很容易出错。
这就是为什么我们需要*概括*一下上面的的代码块，给他一个名字，然后每次我们需要知道
两个数值大小的时候调用这个代码块。这个代码块就是我们说的函数。

.. graphviz::

    digraph function {
        rankdir=LR;
        graph [bgcolor=transparent, resolution=96, fontsize="10"];
        node [shape=circle, fontsize=8, fixedsize=true, penwidth=.4];
        edge [arrowsize=.5, arrowtail="dot", color="#555555", penwidth=.4];
        block[label = "Function", shape="box", color=antiquewhite3, style=filled, peripheries=2];
        input1->block [color="#ff6600"];
        input2->block [color="#ff6600"];
        input3->block [color="#ff6600"];
        block->output1;
        block->output2;
        { rank=same; input1 input2 input3}
    }


一个*函数*就是一个输入一些元素并返回一些结果的代码块。

如果你做过电工，你就会看到函数和电子电路或者元件非常类似。
需要放大一个信号？那就用一个放大器元件组。
需要一个滤波器？那就用一个滤波器元件组。如此类推...

作为一个软件开发者，你的工作会比这个简单很多，
当你把这些复杂的工程看成一些列的模块，这些模块的输入来之另外一个模块的输出，这将会变得非常有趣。
这些复杂的解决方式将被简化并且调试也变得更加简单。

让我们看看应用这些设计去解决下面的问题：
假设我们有一个函数 MAX(A, B)，函数接受两个整形参数{A, B}并返回一个整型 M 为 A 和 B 的较大者。

.. graphviz::

    digraph MAX {
        rankdir=LR;
        graph [bgcolor=transparent, resolution=96, fontsize="10" ];
        node [shape=circle, fontsize=8, fixedsize=true, penwidth=.4];
        edge [arrowsize=.5, arrowtail="dot", color="#555555", penwidth=.4];
        block[label = "MAX", shape="box", color=antiquewhite3, style=filled, peripheries=2];
        A->block [color="#ff6600"];
        B->block [color="#ff6600"];
        block->M;
    }

通过重用``MAX``函数，我们像下面一样算出三个值中的最大者：

.. graphviz::

    digraph MAX {
        rankdir=LR;
        graph [bgcolor=transparent, resolution=96, fontsize="10" ];
        node [shape=circle, fontsize=8, fixedsize=true, penwidth=.4];
        edge [arrowsize=.5, arrowtail="dot", color="#555555", penwidth=.4];
        max1[label = "MAX", shape="box", color=antiquewhite3, style=filled, peripheries=2];
        max2[label = "MAX", shape="box", color=antiquewhite3, style=filled, peripheries=2];
        A->max1 [color="#ff6600"];
        B->max1 [color="#ff6600"];
        max1->max2;
        C->max2 [color="#ff6600"];
        max2->X;
        { rank=same; A B C}
    }

实际上这是个非常简单的例子。作为练习，你可以写一个``MAX3``函数，他接收3个整数参数并返回一个整数，
而不是重复使用``MAX``函数。

当然了， 如果数据类型相同的话，你也可以吧一个函数的输出*插入*到另外一个函数。

在 Go 里面如何实现这个？
=====================
函数的大致语法如下：

.. code-block:: go
    :linenos:

    func funcname(input1 type1, input2 type2) (output1 type1, output2 type2) {
        //some code and processing here
        ...
        //return the results of the function
        return value1, value2
    }

细节：

* 关键字``func``用于定义一个函数的名字*funcname*。
* 函数也许会需要几个输入参数。每一个参数都是跟随着一个对应的类型定义，并且所有参数都是使用逗号分隔。
* 函数可以有多个返回值。
* 在上面的代码片段中，``result1`` 和 ``result2`` 被称作*命名返回值*，如果你不需要命名返回参数，你
  只指定返回值的类型，同样也是使用逗号分隔。
* 如果你的函数只有一个返回值，你可以忽略掉返回参数两边的小括号。
* 如果你的函数不返回任何值，你就可以完全忽略掉这一项。

举些例子会更容易理解这些规则:

一个简单的求最大值函数
---------------------
.. code-block:: go
    :linenos:

    package main
    import "fmt"

    //return the maximum between two int a, and b.
    func max(a, b int) int {
        if a > b {
            return a
        }
        return b
    }

    func main() {
        x := 3
        y := 4
        z := 5

        max_xy := max(x, y) //calling max(x, y)
        max_xz := max(x, z) //calling max(x, z)

        fmt.Printf("max(%d, %d) = %d\n", x, y, max_xy)
        fmt.Printf("max(%d, %d) = %d\n", x, z, max_xz)
        fmt.Printf("max(%d, %d) = %d\n", y, z, max(y,z)) //just call it here
    }

Output:

.. container:: output

    | max(3, 4) = 4
    | max(3, 5) = 5
    | max(4, 5) = 5


我们的函数 *MAX*  有两个输入参数 A 和 B，并且返回一个 ``int`` 类型的值。
注意我们是如何把 A 和 B 的类型组合在一起的。我们也可以写成：MAX(A int, B int)，然而这样写
是个更简短的方法。

注意的是我们更倾向于仅定义返回值的类型 (``int``) 而不是返回命名的类型。

一个返回两个值得函数
--------------------------------

.. code-block:: go
    :linenos:

    package main
    import "fmt"

    //return A+B and A*B in a single shot
    func SumAndProduct(A, B int) (int, int) {
        return A+B, A*B
    }

    func main() {
        x := 3
        y := 4

        xPLUSy, xTIMESy := SumAndProduct(x, y)

        fmt.Printf("%d + %d = %d\n", x, y, xPLUSy)
        fmt.Printf("%d * %d = %d\n", x, y, xTIMESy)
    }

Output:

.. container:: output

    | 3 + 4 = 7
    | 3 * 4 = 12

一个具有两个返回值的函数
---------------------------------
.. code-block:: go
    :linenos:

    package main

    //look how we grouped the import of packages fmt and math
    import(
        "fmt"
        "math"
    )

    //A function that returns a bool that is set to true of Sqrt is possible
    //and false when not. And the actual square root of a float64
    func MySqrt(f float64) (squareroot float64, ok bool){
        if f > 0 {
            squareroot, ok = math.Sqrt(f), true
        } else {
            squareroot, ok = 0, false
        }
        return squareroot, ok
    }

    func main() {
        for index := -2.0; index <= 10; index++ {
            squareroot, possible := MySqrt(index)
            if possible {
                fmt.Printf("The square root of %f is %f\n", index, squareroot)
            } else {
                fmt.Printf("Sorry, no square root for %f\n", index)
            }
        }
    }

Outputs:

.. container:: output

    | Sorry, no square root for -2.000000
    | Sorry, no square root for -1.000000
    | Sorry, no square root for 0.000000
    | The square root of 1.000000 is 1.000000
    | The square root of 2.000000 is 1.414214
    | The square root of 3.000000 is 1.732051
    | The square root of 4.000000 is 2.000000
    | The square root of 5.000000 is 2.236068
    | The square root of 6.000000 is 2.449490
    | The square root of 7.000000 is 2.645751
    | The square root of 8.000000 is 2.828427
    | The square root of 9.000000 is 3.000000
    | The square root of 10.000000 is 3.162278


这里，我们 *import* 包 "``match``" 使为了使用它提供的 ``Sqrt`` (Square root) 函数。
然后我们自己写了一个叫做 ``MySqrt`` 的函数，这个函数返回两个值： 第一个是输入参数
``f`` 的平方根，第二个是 布尔型返回值（表示取平方根成功或失败）

注意我们在函数体中是如何使用参数 ``s`` 和 ``ok`` 来作为一个实际变量的。

因为结果变量会根据他的类型来初始化为"零"(0, 0.00, false...)，我们可以把上面的例子重写: 

.. code-block:: go
    :linenos:

    import "math"

    //return A+B and A*B in a single shot
    func MySqrt(floater float64) (squareroot float64, ok bool){
        if floater > 0 {
            squareroot, ok = math.Sqrt(f), true
        }
        return squareroot, ok
    }

空返回值
----------------
当我们使用命名返回参数时，如果函数的 return 指令没有加参数，那么返回参数的当前值将
被默认用作返回值。

因此，我们可以像下面一样重写上面的例子：

.. code-block:: go
    :linenos:

    import "math"

    //return A+B and A*B in a single shot
    func MySqrt(floater float64) (squareroot float64, ok bool) {
        if floater > 0 {
            squareroot, ok = math.Sqrt(f), true
        }
        return // Omitting the output named variables, but keeping the "return".
    }

.. _value-reference:

传值参数和传引用参数
=====================================
当传递一个参数给函数的时候，这实际上是接收了一份这个参数的 *复制* 。
因此，如果函数修改了这个参数的值，那么最开始的这个参数（译者注：函数外的这个变量）的值
是不会被修改的，因为函数内部只是对这个变量的复制做了操作，而不是这个变量本身。

举个例子验证上面描述：

.. code-block:: go
    :linenos:

    package main
    import "fmt"

    //simple function that returns 1 + its input parameter
    func add1(a int) int {
        a = a+1 // we change the value of a, by adding 1 to it
        return a //return the new value
    }

    func main() {
        x := 3

        fmt.Println("x = ", x) // Should print "x = 3"

        x1 := add1(x) //calling add1(x)

        fmt.Println("x+1 = ", x1) // Should print "x+1 = 4"
        fmt.Println("x = ", x) // Will print "x = 3"
    }

.. container:: output

    | x =  3
    | x+1 =  4
    | x =  3

看到了吗？变量 ``x`` 的值在函数 ``add1`` 调用之后并没有被改变，尽管我们已经在代码的第6行中
执行了 ``a = a+1`` 的操作

原因是很简单的：当我们调用函数 ``add1``, 它接收了变量 ``x`` 的一个复制，而不是 ``x`` 本身，
因此它修改的是那个复制的值，而不是 ``x`` 本身。

我知道你们心中会有个疑问：
*"如果我想要在函数中修改 x 的值，那么我应该怎么做?"*

下面就是指针的用法：这次我们把一个指针变量传递给函数，而不是传递一个 ``int`` 型的普通变量！
这样函数就可以 * 使用*  这个函数外的变量，并直接修改它。

让我们试试看。

.. code-block:: go
    :linenos:

    package main
    import "fmt"

    //simple function that returns 1 + its input parameter
    func add1(a *int) int { // Notice that we give it a pointer to an int!
        *a = *a+1 // We dereference and change the value pointed by a
        return *a // Return the new value
    }

    func main() {
        x := 3

        fmt.Println("x = ", x) // Will print "x = 3"

        x1 := add1(&x) // Calling add1(&x) by passing the address of x to it

        fmt.Println("x+1 = ", x1) // Will print "x+1 = 4"
        fmt.Println("x = ", x) // Will print "x = 4"
    }

.. container:: output

    | x =  3
    | x+1 =  4
    | x =  4

 现在，我们已经修改了 ``x`` 的值！

你也许会问，传递一个引用到函数里面有什么用？

* 第一个原因是传递一个引用可以使函数 * 联合操作*  同一个变量，换句话说，就是你可以使用多个
  函数去操作同一个变量，所有的函数都可以去修改这个变量。

* 指针是非常廉价的。在内存使用上非常廉价。 例如我们使用函数操作一个非常大的数组， 但是每次
  调用这个函数去处理这个数组的时候都需要复制一份这个数组。但是用指针的话，这样能少用很多内存。
  记住了吗？这仅仅是一个内存地址! :)


.. _functions-signatures:

函数的签名
Function's signatures
=====================
和人一样，名字的意义其实不是非常大的。是吗？是的，让我们看看，名字有用，但是更加有意义的是
他们做什么，他们需要什么，他们产出什么（说的就是你！回去工作！）
Like with people, names don't matter that much. Do they? Well, yes, let's face
it, they do matter, but what matters most is what they do, what they need, what
they produce (Yeah you! Get back to work!).

在我们签名的例子中，我们可以把我们的函数 **add1** 命名为 **add_one**, 他同样是和之前一样工作。
真正影响我们函数的是：
1. 它的输入参数：多少个？什么类型？
2. 它的输出参数：多少个？什么类型？
3. 它的函数体：这个函数做什么处理？

我们可以重写这个函数体使它更高效率，主程序会重新被编译并且毫无问题地被执行。但我们不能在函数定
义的地方修改它的输入和输出参数，需要在主程序中继续使用它原有的参数。

换句话说，在上面列出的三点中，影响更大的是：
函数期待什么样的输入参数，函数返回什么样的输出参数。

这两个元素就是我们说的 *函数签名*, 我们会像下面的这个形式使用：

``func (input1 type1 [, input2 type2 [, ...]]) (output1 OutputType1 [, output2 OutputType2 [,...]])``

或者以可选的形式使用函数的名称：

``func function_name (input1 type1 [, input2 type2 [, ...]]) (output1 OutputType1 [, output2 OutputType2 [,...]])``

签名的例子
----------------------

.. code-block:: go
    :linenos:

    //The signature of a function that takes an int and returns and int
    func (int x) x int

    //takes two float and returns a bool
    func (float32, float32) bool

    // Takes a string returns nothing
    func (string)

    // Takes nothing returns nothing
    func()

