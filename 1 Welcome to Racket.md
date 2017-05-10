### Racket

---

#### 1 欢迎使用Racket

取决于你怎么看，**Racket** 可以是

- 一种*编程语言*——起源自Scheme的Lisp方言；

- 一*系列*的编程语言——Racket的各种变体；或者

- 一组*工具*——为了使用一系列的编程语言。

为了不引起混乱，我们单纯地使用Racket这个叫法。

Racket的主要工具包括
- `racket`，核心编译器、解释器和运行时系统；
- **DrRacket**，编程环境；以及
- `raco`，一个命令行工具，用于执行Racket一些命令（**Ra**cket **Co**mmands），例如安装包、构建库等等。

一开始，你可能想要用DrRacket来探索Racket语言。不过如果你愿意的话，你也可以使用命令行的`racket`解释器与你最喜欢的文本编辑器，参考：[Command-Line Tools and Your Editor of Choice](https://docs.racket-lang.org/guide/other-editors.html)。这个指南的剩余部分大多与你使用的编辑器无关。

如果你在使用DrRacket，你需要选择正确的语言，因为DrRacket支持许多Racket变种和其他语言。假设你从没用过DrRacket，就从这一行开始吧

```
#lang racket
```

在DrRacket上部的编辑区域顶点击`Run`按钮，随后DrRacket便明白你想使用原始的Racket（你也可以使用更小巧的[racket/base](https://docs.racket-lang.org/reference/index.html)或是其他的变体）。

如果你曾经在DrRacket中使用过别的带[#lang](https://docs.racket-lang.org/guide/Module_Syntax.html#%28part._hash-lang%29)的程序，DrRacket会记忆你上一次使用的语言而非从[#lang](https://docs.racket-lang.org/guide/Module_Syntax.html#%28part._hash-lang%29)行推断。这种情形下，使用`Language|Choose Language...`菜单项。从弹出的对话框中选择第一个选项，这个选项告诉DrRacket使用源代码中[#lang](https://docs.racket-lang.org/guide/Module_Syntax.html#%28part._hash-lang%29)所声明的语言。随后把[#lang](https://docs.racket-lang.org/guide/Module_Syntax.html#%28part._hash-lang%29)行写在上部的编辑区域内，就像之前说的一样。

---

#### 1.1 与Racket交互

DrRacket的下部编辑区域和`racket`命令行程序（不加参数启动）都可以作为演算器使用。输入一个Racket表达式，点击Return/Enter键，然后结果就被打印出来。用Racket术语来说，这类演算器被称为*输入-执行-打印循环* 或者*REPL*。

数字自身便是一个表达式，执行结果就是这个数字自身

```
> 5
5
```

一个字符串是执行自身的表达式。字符串被包括在双引号当中。

```
> "Hello, world!"
"Hello, world!"
```

Racket使用圆括号用来包裹更大更复杂的表达式——除了简单的常量之外的所有表达式。例如，函数调用是这样的：开圆括号，函数名，参数表达式，闭圆括号。下面的表达式调用了内建函数[substring](https://docs.racket-lang.org/reference/strings.html#%28def._%28%28quote._~23~25kernel%29._substring%29%29)，参数是`"the boy out of the country", 4`，与`7`。

```
> (substring "the boy out of the country" 4 7)
"boy"
```

---

#### 1.2 定义与交互

你可以使用[define](https://docs.racket-lang.org/reference/define.html#%28form._%28%28lib._racket%2Fprivate%2Fbase..rkt%29._define%29%29)来定义像[substring](https://docs.racket-lang.org/reference/strings.html#%28def._%28%28quote._~23~25kernel%29._substring%29%29)一样工作的自己的函数，就像这样：

```
(define (extract str)
	substring str 4 7))
	
> (extract "the boy out of the country")
"boy"
> (extract "the country out of the boy")
"cou"
```

尽管你可以在 [REPL](https://docs.racket-lang.org/guide/intro.html#%28tech._repl%29)中使用[define](https://docs.racket-lang.org/reference/define.html#%28form._%28%28lib._racket%2Fprivate%2Fbase..rkt%29._define%29%29)，定义一般是你想长期使用或者稍后使用的程序的一部分。所以，在DrRacket中，你可以将定义放在上部编辑器中——被称作定义区——带上[#lang](https://docs.racket-lang.org/guide/Module_Syntax.html#%28part._hash-lang%29)前缀：

```
#lang racket

(define (extract str)
	(substring str 4 7))
```

如果对`(extract "the boy")`的调用是你的程序的主行为，它也可以放在[定义区](https://docs.racket-lang.org/guide/intro.html#%28tech._definitions._area%29)内。不过如果它只是一个用来探索`extract`的范例表达式，你可以离开[定义区](https://docs.racket-lang.org/guide/intro.html#%28tech._definitions._area%29)，点击`Run`，随后在[REPL](https://docs.racket-lang.org/guide/intro.html#%28tech._repl%29)中运行`(extract "the boy")`。

在使用命令行的`racket`而非DrRacket时，你可以保存上面的代码到一个文件中。例如你把它保存成了`"extract.rkt"`，那么在同一个目录运行`racket`，你可以直接运行下面的代码：

```
> (enter! "extract.rkt")
> (extract "the gal out of the city")
"gal"
```

[enter!](https://docs.racket-lang.org/reference/interactive.html#%28form._%28%28lib._racket%2Fenter..rkt%29._enter%21%29%29)载入文件中的代码并且将运行的上下文切换到这个模块中，就像DrRacket的`Run`按钮一样。

---

#### 1.3 生成可执行文件

如果你的文件（或者DrRacket的[定义区]()）包括了：

```
#lang racket

(define (extract str)
	(substring str  4 7))
	
(extract "the cat out of the bag")
```

那么这就是一个运行后打印”cat“的完整程序。你可以在DrRacket中或使用`racket`的[enter!]()运行它。如果它被保存在 <src-filename> 中，你也可以通过 `racket <src-filename>`来运行它。

想要将你的程序打包成一个可执行程序，你有如下选择：

- 在DrRacket中，选择`Racket|Create Executable...`菜单项；
- 在命令行中运行`raco exe <src-filename>`，其中 <src-filename> 保存了这个程序。参考[raco exe: Creating Stand-Alone Executables]()来获得更多信息。
- 在Unix或者Mac OS中，你也可以将源文件转化成一个可执行脚本，只需要在文件的开头加上这一行`#! /usr/bin/env racket`。使用`chmod +x <filename>`来获得执行权限。

只要`racket`在用户的path下，这个脚本都能正常工作。或者，在`#!`后使用完整的`racket`路径，这样用户的path便无所谓了。

---

#### 1.4 给Lisp/Scheme使用者的备注

如果你已经对Scheme或Lisp有了一点了解，你也许会把

```
(define (extract str)
  (substring str 4 7))
```

保存在`extract.rktl`中，并且使用`racket`运行下面的代码：

```
> (load "extract.rktl")
> (extract "the dog out")
"dog"
```

这可以工作，因为`racket`愿意模拟传统Lisp的环境，但是我们强烈建议不要使用`load`或者在一个模块外写程序。

在模块外写程序会导致混乱的报错信息、糟糕的性能以及难以组合、运行程序。这个问题不是`racket`独有的；这是传统顶层环境的一个基本限制。Scheme和Lisp的实现与特别指定的命令行标志、编译器指示和构建工具斗争了多年。模块系统便是用于避免这些问题的，长远来看，用[#lang]()开头会使你更加高兴。