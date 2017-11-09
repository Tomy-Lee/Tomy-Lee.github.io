---
layout:     post
title:      "使用Go语言开发selpg命令行程序"
subtitle:   "Go语言编程"
date:       2017-11-09
author:     "tomylee"
header-img: "img/home-bg-o.jpg"
catalog: true
tags:
    go
---


# Selpg

>Selpg is a utility that selects page range from text input. The input can come from the file specified as the last command line parameter, and can also be from standard input when no file name argument is given. selpg first handles all command line arguments. After scanning all the option parameters (that is, those with a hyphen), if selpg finds a parameter, it will accept the parameter as the name of the input file and try to open it for reading. If there are no other parameters, selpg assumes that the input comes from standard input.

>selpg 是从文本输入选择页范围的实用程序。该输入可以来自作为最后一个命令行参数指定的文件，在没有给出文件名参数时也可以来自标准输入。selpg首先处理所有的命令行参数。在扫描了所有的选项参数（也就是那些以连字符为前缀的参数）后，如果selpg发现还有一个参数，则它会接受该参数为输入文件的名称并尝试打开它以进行读取。如果没有其它参数，则 selpg 假定输入来自标准输入。
>selpg命令开发C语言版本：[selpg.c](https://www.ibm.com/developerworks/cn/linux/shell/clutil/selpg.c)

---

## Reference
1.[开发 Linux 命令行实用程序](https://www.ibm.com/developerworks/cn/linux/shell/clutil/index.html)
2.[Linux命令行程序设计](https://wenku.baidu.com/view/c7cf91ee5ef7ba0d4a733b58.html)
3.[Using Python to create UNIX command line tools](https://www.ibm.com/developerworks/aix/library/au-pythocli/index.html)

## 命令行准则
通用 Linux 实用程序的编写者应该在代码中遵守某些准则。这些准则经过了长期发展，它们有助于确保用户以更灵活的方式使用实用程序，特别是在与其它命令（内置的或用户编写的）以及 shell 的协作方面 ― 这种协作是利用 Linux 作为开发环境的能力的手段之一。selpg 实用程序用实例说明了下面列出的所有准则和特性。（注：在接下来的那些示例中，“$”符号代表 shell 提示符，不必输入它。）
#### 准则 1. 输入
应该允许输入来自以下两种方式：

-  在命令行上指定的文件名。例如：
 > $ command input_file
 >  
>在这个例子中，command 应该读取文件 input_file。


-  标准输入（stdin），缺省情况下为终端（也就是用户的键盘）。例如：
>$ command
>
这里，用户输入 Control-D（文件结束指示符）前输入的所有内容都成为 command 的输入。

-  但是，使用 shell 操作符“<”（重定向标准输入），也可将标准输入重定向为来自文件，如下所示：
> $ command < input_file
这里，command 会读它的标准输入，不过 shell／内核已将其重定向，所以标准输入来自 input_file。

-  使用 shell 操作符“|”（pipe）也可以使标准输入来自另一个程序的标准输出，如下所示：
>$ other_command | command
这里，other_command 的标准输出（stdout）被 shell／内核透明地传递至 command 的标准输入。

#### 准则 2. 输出

-  输出应该被写至标准输出，缺省情况下标准输出同样也是终端（也就是用户的屏幕）：
>$ command
在这个例子中，command 的输出出现在屏幕上。

-  同样，使用 shell 操作符“>”（重定向标准输出）可以将标准输出重定向至文件。
>$ command > output_file
这里，command 仍然写至它的标准输出，不过 shell／内核将其重定向，所以输出写至 output_file。

-  或者，还是使用“|”操作符，command 的输出可以成为另一个程序的标准输入，如下所示：
>$ command | other_command
在这个例子中，shell／内核安排 command 的输出成为 other_command 的输入。

#### 准则 3. 错误输出
-  错误输出应该被写至标准错误（stderr），缺省情况下标准错误同样也是终端（也就是用户的屏幕）：
>$ command
这里，运行 command 时出现的任何错误消息都将被写至屏幕。

-  但是使用标准错误重定向，也可以将错误重定向至文件。例如：
>$ command 2>error_file
在这个例子中，command 的正常输出在屏幕显示，而任何错误消息都被写至 error_file。

-  可以将标准输出和标准错误都重定向至不同的文件，如下所示：
>$ command >output_file 2>error_file
这里，将标准输出写至 output_file，而将所有写至标准错误的内容都写至 error_file。

-  如果已将标准输出重定向至某一位置，也可以将标准错误重定向至同一位置。例如：
>$ command 2>&1
>
>在这个例子中，符号“2>&1”表示“将标准错误发送至标准输出被重定向的任何位置”，因此错误和正常的消息都将在屏幕上显示。当然，这是多余的，因为下面简单的调用:
>$ command
>
将做同样的事。在标准输出已被重定向至其它源，而您希望在同一命令行上将标准错误也写至同一目的地时，该特性就非常有用。例如：
$ command >output_file 2>&1
>
>在这个例子中，已首先将标准输出重定向至 output_file；因此“2>&1”将使标准错误也被重定向至 output_file。

#### 准则 4. 执行
程序应该有可能既独立运行，也可以作为管道的一部分运行，如上面的示例所示。该特性可以重新叙述如下：不管程序的输入源（文件、管道或终端）和输出目的地是什么，程序都应该以同样的方式工作。这使得在如何使用它方面有最大的灵活性。
#### 准则 5. 命令行参数
如果程序可以根据其输入或用户的首选参数有不同的行为，则应将它编写为接受名为 选项的命令行参数，这些参数允许用户指定什么行为将用于这个调用。
作为选项的命令行参数由前缀“-”（连字符）标识。另一类参数是那些不是选项的参数，也就是说，它们并不真正更改程序的行为，而更象是数据名称。通常，这类参数代表程序要处理的文件名，但也并非一定如此；参数也可以代表其它东西，如打印目的地或作业标识（有关的示例，请参阅“man cancel”）。
可能代表文件名或其它任何东西的非选项参数（那些没有连字符作为前缀的）如果出现的话，应该在命令的最后出现。
通常，如果指定了文件名参数，则程序把它作为输入。否则程序从标准输入进行读取。
所有选项都应以“-”（连字符）开头。选项可以附加参数。
Linux 实用程序语法图看起来如下：
>$ command mandatory_opts [ optional_opts ] [ other_args ]

其中：

- command 是命令本身的名称。
- mandatory_opts 是为使命令正常工作必须出现的选项列表。
- optional_opts 是可指定也可不指定的选项列表，这由用户来选择；但是，其中一些参数可能是互斥的，如同 selpg 的“-f”和“-l”选项的情况（详情见下文）。
- other_args 是命令要处理的其它参数的列表；这可以是任何东西，而不仅仅是文件名。
在以上定义中，术语“选项列表”是指由空格、跳格或二者的结合所分隔的一系列选项。
以上在方括号中显示的语法部分可以省去（在此情况下，必须将括号也省去）。
各个选项看起来可能与下面相似：
-f （单个选项）
-s20 （带附加参数的选项）
-e30 （带附加参数的选项）
-l66 （带附加参数的选项）
有些实用程序对带参数的选项采取略微不同的格式，其中参数与选项由空格分隔 ― 例如，“-s 20” ― 但我没有选择这么做，因为它会使编码复杂化；这样做的唯一好处是使命令易读一些。
以上是 selpg 支持的实际选项。

## selpg 程序逻辑
如前面所说的那样，selpg 是从文本输入选择页范围的实用程序。该输入可以来自作为最后一个命令行参数指定的文件，在没有给出文件名参数时也可以来自标准输入。
selpg 首先处理所有的命令行参数。在扫描了所有的选项参数（也就是那些以连字符为前缀的参数）后，如果 selpg 发现还有一个参数，则它会接受该参数为输入文件的名称并尝试打开它以进行读取。如果没有其它参数，则 selpg 假定输入来自标准输入。
#### 参数处理
**“-sNumber”和“-eNumber”强制选项：**
selpg 要求用户用两个命令行参数“-sNumber”（例如，“-s10”表示从第 10 页开始）和“-eNumber”（例如，“-e20”表示在第 20 页结束）指定要抽取的页面范围的起始页和结束页。selpg 对所给的页号进行合理性检查；换句话说，它会检查两个数字是否为有效的正整数以及结束页是否不小于起始页。这两个选项，“-sNumber”和“-eNumber”是强制性的，而且必须是命令行上在命令名 selpg 之后的头两个参数：
>$ selpg -s10 -e20 ...

（... 是命令的余下部分，下面对它们做了描述）。
**“-lNumber”和“-f”可选选项：**
selpg 可以处理两种输入文本：
***类型 1***：该类文本的页行数固定。这是缺省类型，因此不必给出选项进行说明。也就是说，如果既没有给出“-lNumber”也没有给出“-f”选项，则 selpg 会理解为页有固定的长度（每页 72 行）。
选择 72 作为缺省值是因为在行打印机上这是很常见的页长度。这样做的意图是将最常见的命令用法作为缺省值，这样用户就不必输入多余的选项。该缺省值可以用“-lNumber”选项覆盖，如下所示：
>$ selpg -s10 -e20 -l66 ...

这表明页有固定长度，每页为 66 行。

***类型 2***：该类型文本的页由 ASCII 换页字符（十进制数值为 12，在 C 中用“\f”表示）定界。该格式与“每页行数固定”格式相比的好处在于，当每页的行数有很大不同而且文件有很多页时，该格式可以节省磁盘空间。在含有文本的行后面，类型 2 的页只需要一个字符 ― 换页 ― 就可以表示该页的结束。打印机会识别换页符并自动根据在新的页开始新行所需的行数移动打印头。
    将这一点与类型 1 比较：在类型 1 中，文件必须包含 PAGELEN - CURRENTPAGELEN 个新的行以将文本移至下一页，在这里 PAGELEN 是固定的页大小而 CURRENTPAGELEN 是当前页上实际文本行的数目。在此情况下，为了使打印头移至下一页的页首，打印机实际上必须打印许多新行。这在磁盘空间利用和打印机速度方面效率都很低（尽管实际的区别可能不太大）。
类型 2 格式由“-f”选项表示，如下所示：
> $ selpg -s10 -e20 -f ...

该命令告诉 selpg 在输入中寻找换页符，并将其作为页定界符处理。
注：“-lNumber”和“-f”选项是互斥的。

**“-dDestination”可选选项：**
selpg 还允许用户使用“-dDestination”选项将选定的页直接发送至打印机。这里，“Destination”应该是 lp 命令“-d”选项（请参阅“man lp”）可接受的打印目的地名称。该目的地应该存在 ― selpg 不检查这一点。在运行了带“-d”选项的 selpg 命令后，若要验证该选项是否已生效，请运行命令“lpstat -t”。该命令应该显示添加到“Destination”打印队列的一项打印作业。如果当前有打印机连接至该目的地并且是启用的，则打印机应打印该输出。这一特性是用 popen() 系统调用实现的，该系统调用允许一个进程打开到另一个进程的管道，将管道用于输出或输入。在下面的示例中，我们打开到命令
>$ lp -dDestination

的管道以便输出，并写至该管道而不是标准输出：
>selpg -s10 -e20 -dlp1

该命令将选定的页作为打印作业发送至 lp1 打印目的地。您应该可以看到类似“request id is lp1-6”的消息。该消息来自 lp 命令；它显示打印作业标识。如果在运行 selpg 命令之后立即运行命令 lpstat -t | grep lp1 ，您应该看见 lp1 队列中的作业。如果在运行 lpstat 命令前耽搁了一些时间，那么您可能看不到该作业，因为它一旦被打印就从队列中消失了。
#### 输入处理
一旦处理了所有的命令行参数，就使用这些指定的选项以及输入、输出源和目标来开始输入的实际处理。
selpg 通过以下方法记住当前页号：如果输入是每页行数固定的，则 selpg 统计新行数，直到达到页长度后增加页计数器。如果输入是换页定界的，则 selpg 改为统计换页符。这两种情况下，只要页计数器的值在起始页和结束页之间这一条件保持为真，selpg 就会输出文本（逐行或逐字）。当那个条件为假（也就是说，页计数器的值小于起始页或大于结束页）时，则 selpg 不再写任何输出。瞧！您得到了想输出的那些页。

## design process
导入所需包
```go
packge main

import (
	"bufio"
	"flag"
	"fmt"
	"io"
	"os"
	"os/exec"
)
```
定义 selpgargs 结构体，存储用户从命令行输入的各项参数等信息：
```go
type selpgargs struct {
	start_page int      //起始页面
	end_page   int      //终止页面
	input_file string   //读取的文件名
	page_len   int      //文件中每页行数，默认值72
	form_deli  bool    //
}
```
全局的 string 变量 progname ，在错误消息中显示，即使将 selpg 命令重命名为别的名称，新的名称也将在消息中显示。
```go
var progname string
```

函数接口
```go
func Usage() {}   // 向用户显示 selpg 指令的用法
func FlagInit(args *selpgargs) {} //初始定义一些标记变量
func ProcessArgs(args *selpgargs) {}
// 把用户输入的各个参数分割成 selpgArgs 结构体实例中的每个部分，并存储在结构体实例中
func ProcessInput(args *selpgargs) {}
// 根据用户输入的各个参数进行相应的操作
func printOrWrite(args *selpgargs, line string, stdin io.WriteCloser) {}
//写行
```


## Selpg usage

 >   selpg -s=Number -e=Number [options] [filename]
>
* `-s=Number` Specify the start number. Must be the first argument.
* `-e=Number` Specify the end number. Must be the second argument.
* `-l=number` [option] Set the number of line per page. Default is 72.
* `-f` [option] Pagers are separated by /f is true. Default is false.
* `filename` [option] Input from this file. Default is input from standard input.


## Test examples

>安装Go语言环境，然后将bin目录添加到路径下面，通过go get安装selpg。如果环境变量中已经设置过GOBIN，可以在系统其他位置执行selpg。


### 1.help
```
./selpg -h
```

```
Usage of ./selpg:

./selpg is a tool to select pages from what you want.

Usage:

        selpg -s=Number -e=Number [options] [filename]

The arguments are:

        -s=Number       Start from Page <number>.
        -e=Number       End to Page <number>.
        -l=Number       [options]Specify the number of line per page.Default is 72.
        -d=lp number    [options]Using cat to test.
        -f              [options]Specify that the pages are sperated by \f.
        [filename]      [options]Read input from the file.

If no file specified, ./selpg will read input from stdin. Control-D to end.

```

### 2.some tests

There is a text file named `test` and the content is 135 lines tests:
```
 test1
 test2
 test3
 test4
 ...
 test131
 test132
 test133
 test134
 test135
```
Then here are some commands to test the selpg.
1.

    $ selpg -s 1 -e 1  -l 15 test
    test16
    test17
    test18
    test19
    test20
    test21
    test22
    test23
    test24
    test25
    test26
    test27
    test28
    test29
    test30

2.

    $ selpg -s=2 -e=3 -l 6 test
    test13
    test14
    test15
    test16
    test17
    test18
    test19
    test20
    test21
    test22
    test23
    test24
3.

    $ selpg -s=1 -e=1 -l 10 < test
    test11
    test12
    test13
    test14
    test15
    test16
    test17
    test18
    test19
    test20

4.

    $ cat test | selpg -s 2 -e 3 -l 10
    test21
    test22
    test23
    test24
    test25
    test26
    test27
    test28
    test29
    test30
    test31
    test32
    test33
    test34
    test35
    test36
    test37
    test38
    test39
    test40

5.

    $ selpg -s 2 -e 3 -l 10 test >output
    test21
    test22
    test23
    test24
    test25
    test26
    test27
    test28
    test29
    test30
    test31
    test32
    test33
    test34
    test35
    test36
    test37
    test38
    test39
    test40

6.

    $ selpg -s 2 -e 0 -l 5 test
    Invalid arguments
    Usage of selpg:
    
    selpg is a tool to select pages from what you want.

    Usage:

            selpg -s=Number -e=Number [options] [filename]

    The arguments are:

            -s=Number       Start from Page <number>.
            -e=Number       End to Page <number>.
            -l=Number       [options]Specify the number of line per page.Default is 72.
            -f              [options]Specify that the pages are sperated by \f.
            [filename]      [options]Read input from the file.

    If no file specified, selpg will read input from stdin. Control-D to end.
  

7.

    $ selpg -s 0 -e 0 test (测试默认行数输出)
    test1
    test2
    test3
    test4
    test5
    test6
    test7
    test8
    test9
    test10
    test11
    test12
    test13
    ...
    test68
    test69
    test70
    test71
    test72
8.

    selpg -s 0 -e 1 -l 5 -d=lp1 test
    1  test1
    2  test2
    3  test3
    4  test4
    5  test5
    6  test6
    7  test7
    8  test8
    9  test9 
    10 test10
    
---

---

---


## 代码：



>
package main
>
import (
	"bufio"
	"flag"
	"fmt"
	"io"
	"os"
	"os/exec"
)

>



##  [我的Github](https://github.com/Tomy-Lee/selpg)
