.. include:: common.txt

基础流程控制
******************
在前面的章节，我们学习了如何去定义变量和常量。
我们也看到了 GO 不同的基础的内置变量。
在这一章节中，我们将会看到基础的控制结构, or how to *reason about* this data. 
但在这之前，让我们先聊聊注释和分号。

注释
========
你可能已经注意到在前面章节的示例代码中的一些注释，没看到吗？
Go 的注释方法和 C++  是一样的:

* 单行注释：所有从``//``开始直到结束的那一行内容都是注释。
* 块注释：所有在``/*`` 到 ``*/`` 之间的内容都是注释。

.. code-block:: go
    :linenos:

    //Single line comment, this is ignored by the compiler.
    /* Everything from the beginning of this line until the keyword 'var' on
    line 4 is a comment and ignored by the compiler. */
    var(
        integer int
        pi = float32
        prefix string
    )

分号
==========
如果你是用 C、C++或者 Pascal 写程序的，那么你就会很想直到为什么前面的示例中没有看到分号。
实际在 Go 里面步需要分号。Go 会自动的在所有看起来像是行结束的地方放置分号。

这会让代码看起来清晰很多，也更容易阅读。

唯一你能看到分号的地方就是在循环逻辑中的分句中，他们不是在所有的声明中都需要的。

注意，你可以在同一行中使用分号来分隔不同的声明语句。

有一个会让你感到惊讶的是，
和if 一样在一个和 if 声明一样的结构中放置一个左大括号在同一 行是非常重要的。
如果你不这么做的话，就可能编译失败或者得到错误的结果 [#f1]
The one surprise is that it's important to put the opening brace of a construct
such as an if statement on the same line as the if; if you don't, there are
situations that may not compile or may give the wrong result [#f1]_

好了，我们现在开始了。

if 声明
================
也许这是在命令式编程中大家最熟悉的类型声明了。它可以被概括为：
 if 条件，do 做什么，else 做什么
Perhaps the most well-known statement type in imperative programming. And it can
be summarized like this: if condition met, do this, else do that.

在 Go 里面，有不必使用小括号来扩住条件语句。

.. code-block:: go
    :linenos:

    if x > 10 {
        fmt.Println("x is greater than 10")
    } else {
        fmt.Println("x is less than 10")
    }

你也可以在条件之前有一个简短的初始化什么。

.. code-block:: go
    :linenos:

    // Compute the value of x, and then compare it to 10.
    if x := computed_value(); x > 10 {
        fmt.Println("x is greater than 10")
    } else {
        fmt.Println("x is less than 10")
    }

你也可以联合使用多个if/else 声明。

.. code-block:: go
    :linenos:

    if integer == 3 {
        fmt.Println("The integer is equal to 3")
    } else if integer < 3 {
        fmt.Println("The integer is less than 3")
    } else {
        fmt.Println("The integer is greater than 3")
    }

for 声明
=================
``for``声明是用于迭代和循环。 它的大致语法如下：

.. code-block:: go
    :linenos:

    for expression1; expression2; expression3{
        ...
    }

总的来说，``expression1``, ``expression2``, 和 ``expression3`` 是表达式，
``espression1``和``expression3``是用于指令或者函数调用（``expression``在循环之前调用，
``expression3``在每次迭代之后执行），``expression2`` 用于判断迭代是否继续或停止。

来个例子会比前面的描述更清晰对吧？好那就来一个！

.. code-block:: go
    :linenos:

    package main
    import "fmt"

    func main(){
        sum := 0;
        for index:=0; index < 10 ; index++ {
            sum += index
        }
        fmt.Println("sum is equal to ", sum)
    }

在上面的代码中，我们初始化了一个变量``sum`` 的值为``0``。
``for``循环 开始的时候把变量``index``赋值为``0``(``index := 0``)。

然后当条件``index < 10``为真，``for``的循环体开始执行(``sum += i``)。

在每次迭代结束时``for``循环都会执行``index++``这个表达式(例如，自增``index``)。

 你可以猜测到上面程序的输出会是这样：

.. container:: output

    | sum is equal to  45

你的答案是正确的！
因为0+1+2+3+4+5+6+7+8+9 的结果就是45。

**问题:**

  在上面的程序中是否可以把第5和第6行合并成一行？如何实现？

实际上``expression1``, ``expression2``, 和 ``expression3``都是可选的。
这就是说在``for``循环里你可以忽略一个、两个或者全部都忽略掉。
如果你忽略表达式``expression1``或者``expression3``，就代表循环不会执行。
如果你忽略表达式``expression2``就代表条件一直为真，循环会一直执行下去永不结束 -- 除非遇到``break``声明。

因为表达式是可以忽略的，因此你可以像下面这些例子一样写代码：

.. code-block:: go
    :linenos:

    //expression1 and expression3 are omitted here
    sum := 1
    for ; sum < 1000;  {
        sum += sum
    }

也可以简单的把分号去掉：

.. code-block:: go
    :linenos:

    //expression1 and expression3 are omitted here, and semicolons gone.
    sum := 1
    for sum < 1000 {
        sum += sum
    }

甚至连 expression2 也可以去掉，那么就像下面这样：

.. code-block:: go
    :linenos:

    //infinite loop, no semicolons at all.
    for {
        fmt.Println("I loop for ever!")
    }

中断和继续
==================
使用``break``你可以简单的中断循环。 例如：在``expression2``仍然为真的情况下中断循环。

.. code-block:: go
    :linenos:

    for index := 10; index>0; index-- {
        if index < 5{
            break
        }
        fmt.Println(index)
    }

上面的代码片段是说：从10到0打印数字（第6行）；但是当 i<5 的时候终止（停止）循环！
因此程序将会打印：10，9，8，7，6，5

``continue``则是中断当前的迭代直接跳到一下此次的循环。

.. code-block:: go
    :linenos:

    for index := 10; index > 0; index-- {
        if index == 5 {
            continue
        }
        fmt.Println(index)
    }

这个程序将会打印除了5之外从10到1的所有数字，因为在第三行里面，条件``if index == 5``为真，
因此``continue``将会被执行, ``fmt.Println(index) (for index == 5)`` 将不会被执行。

switch 声明
====================
有时候，你需要写些复杂的``if``/``else``测试。你的代码将变得很难看，并且难以阅读和维护。
``switch``声明能使它变得更好并且很容易的去阅读。

Go 的 switch 比 C 中的更加松散，因为 case 表达式步需要是常量甚至也步需要是整形。

switch 声明的大致形式如下：

.. code-block:: go
    :linenos:

    // General form of a switch statement:
    switch sExpr {
        case expr1:
            some instructions
        case expr2:
            some other instructions
        case expr3:
            some other instructions
        default:
            other code
    }

类型``sExpr``和表达式``expr1``, ``expr2``, ``expr3``... 的类型应该要匹配。
例如： 如果``var``是``int``类型，那么 case  表达式应该同样是``int``类型。

当然，你可以使用 case 的数量是不限的。

可以使用多个匹配表达式，每个都在 case 声明中使用逗号分开。

如果不存在``sExpr``表达式，那么默认就是*布尔*类型，case 条件也应该是*布尔*类型。

如果超过一个 case 声明被匹配，那么字面上的第一个将会被执行。
If more than one case statements match, then the first in lexical order is
executed.

*default*这个 case 条件是可选的，并且可以放在 switch 块的任何地方，而不是只能放最后。

 和其他的一些编程语言不一样，每隔一个 case 快都是独立的，代码不会*落空*（每一个 case 代码块都
像独立的 if-else-if 代码块一样。 如果你希望落空的匹配，你可以使用 fallthrough 声明来明确地 指定。
Unlike certain other languages, each case block is independent and code does not
"fall through" (each of the case code blocks are like independent if-else-if
code blocks. There is a fallthrough statement that you can explicitly use to
obtain fall through behavior if desired.)

一些 switch 的例子：
---------------------

.. code-block:: go
    :linenos:

    // Simple switch statement example:
    i := 10
    switch i {
        case 1:
            fmt.Println("i is equal to 1")
        case 2, 3, 4:
            fmt.Println("i is equal to 2, 3 or 4")
        case 10:
            fmt.Println("i is equal to 10")
        default:
            fmt.Println("All I know is that i is an integer")
    }

 在这个片段中，我们初始化``index``为10，但实际上，你应该把``index``看成是一个计算型的值（
switch 声明前的某种计算或者是函数返回值）

 注意在第6行中，我们把一些表达式（2，3，4）组合成一个单独的 case 声明。

.. code-block:: go
    :linenos:

    // Switch example without sExpr
    index := 10
    switch {
        case index < 10:
            fmt.Println("The index is less than 10")
        case index > 10, index < 0:
            fmt.Println("The index is either bigger than 10 or less than 0")
        case index == 10:
            fmt.Println("The index is equal to 10")
        default:
            fmt.Println("This won't be printed anyway")
    }

现在在这个例子中，我们忽略``sExpr``。因此 case 表达式应该是*布尔*类型。它们就是布尔类型！（
它们对比的结果要么是``true``要么是``false``）
Now, in this example, we omitted ``sExpr`` of the general form. So the cases
expressions should be of type *bool*. And so they are! (They're comparisons that
return either ``true`` or ``false``)

**问题**:

  你可以说说为什么第10行的``default``这个条件永远不会被执行吗？


.. code-block:: go
    :linenos:

    //switch example with fallthrough
    integer := 6
    switch integer {
    case 4:
        fmt.Println("The integer was <= 4")
        fallthrough
    case 5:
        fmt.Println("The integer was <= 5")
        fallthrough
    case 6:
        fmt.Println("The integer was <= 6")
        fallthrough
    case 7:
        fmt.Println("The integer was <= 7")
        fallthrough
    case 8:
        fmt.Println("The integer was <= 8")
        fallthrough
    default:
        fmt.Println("default case")
    }

在这个例子中，第10行的 case 条件被匹配了，但是因为这里有一个``fallthrough``，
因此``case 7:``以及后面的代码都会被执行（和 C 的 switch 不是用``break``关键字一样）


.. [#f1] http://golang.org/doc/go_tutorial.html#tmp_33
