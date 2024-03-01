# 1. 第一个Go语言程序

```go
package main

import "fmt"

func main() {
    fmt.Println("Hello, world")
}
```

Go语言是编译性语言（静态编译）。

Go语言提供的工具通过go命令调用。

example:（$表示命令提示符）

```cmd
$ go run test.go
```

```cmd
Hello,world
```

编译一个或多个以.go结尾的源文件，链接库文件，并运行最终生成的可执行文件。

Go语言原生支持Unicode编码。

Go语言代码通过package（包）进行组织，类似于其他语言中的libraries，或者modules。一个包由位于单个目录下一个或多个go语言源文件组成，目录定义包的作用，每个源文件都以package声明语句开始，表示文件属于某一个包，以及一系列import包，之后是存储在当前文件下的程序。

Go标准库定义了100多个包，例如fmt包，含有格式化输出，接受输入的函数。Println函数可以打印通过空格间隔的一个或多个值，在最后添加换行符。

example：

```go
fmt.Println("The first program about golang")
```

main包定义了一个独立可执行程序，而不是一个库。main里的main函数是整个程序的执行入口，main函数一般调用其他包里的函数。

example：

```go
func main() {
	fmt.Println("Hello world")
}
```

编写Go程序时必须告诉编译器使用了哪些包，这些包通过import引入 。

example：

```go
import "fmt"
```

大多数程序需要引入多个包。

如果**缺少了必须的包或者引入了不必要的包**，在程序编译过程中都会出错，严格要求避免在程序中引入未使用的包（**go语言的编译过程没有警告信息，这也是go语言的争议特性之一**）。

import声明必须在package声明之后，随后则是程序的函数，变量，常量，类型的声明语句（func，var，const，type）。没有严格的声明顺序，但最好遵顼定义规范。有些示例程序会省略package，import声明为了节省代码篇幅，但在源码中这些声明肯定有，否则会编译错误。

一个函数的声明由func关键字，函数名，参数列表，返回值列表，以及包含在{}中的函数体组成。

example：

```utf-8
func functionName(list1 return) {
	function body
}
```

**Go语言不需要在声明和语句末尾添加分号，除非上一行有多条语句。（编译器会自动将特定符号后面的换行符转化成分号，因此换行符的添加位置会影响go语言编译器的正确解析）**比如行末是标识符、整数、浮点数、虚数、字符或字符串文字、关键字`break`、`continue`、`fallthrough`或`return`中的一个、运算符和分隔符`++`、`--`、`)`、`]`或`}`中的一个。

example：错误写法

```go
func functionName() 
{
	function body
}
```

example：正确写法

```
func functionName() {
	function body
}
```

func后面的大括号 "{" 必须和func声明在同一行，位于行末，不能独占一行。

example：x + y表达式

```go
func calculate() {
	return x + y
}
```

```go
func calculate() {
	return x +
	y
}
```

以上两段代码都正确，在 + 前不可换行，在后面可以换行，以加号结尾不会添加分号分隔符，但是在 x 后面会插入分号分隔符，会导致编译错误。

gofmt工具将代码格式化为标准格式（不可修改），并且go工具中的 fmt 子命令会对指定包，否则默认为当前目录中所有.go源文件应用gofmt命令。

优点：方便做多种自动源码转换。

单行注释：//注释内容

多行注释：/* 注释内容 */

# 2.程序结构

## 2.1命名

Go语言函数名，变量名，常量名，类型名，语句标号，包名命名规范：必须以字母或者是下划线开头，后面可以跟任意数量的字母、数字、下划线，区分大小写。推荐使用驼峰命名法（名字首字母小写之后的每个单词首字母大写）

关键字：

```go
break      default       func     interface   select
case       defer         go       map         struct
chan       else          goto     package     switch
const      fallthrough   if       range       type
continue   for           import   return      var
```

预定义：

```go
内建常量: true false iota nil

内建类型: int int8 int16 int32 int64
          uint uint8 uint16 uint32 uint64 uintptr
          float32 float64 complex128 complex64
          bool byte rune string error

内建函数: make len cap new append copy close delete
          complex real imag
          panic recover
```

**注：关键字不能用作命名，只能在特定的语法结构中使用，预定义不是关键字，可以在一些特殊的场景中重新定义，但是要注意避免过度而引起语义混乱。**

名字在函数内部定义，那么它的作用范围就是函数内部，如果是在函数外部定义，那么在当前包的所有文件中都可以访问。

名字开头字母的大小写决定了名字在包外的可见性，如果名字是大写字母开头，那么它将是导出的，可以被外部的包访问。

**注：必须是在函数外部定义的包级名字；包级函数名本身也是包级名字，包本身的名字一般用小写**

名字的长度逻辑上没有限制，但是编码风格尽量采用短小的语义化命名，尤其是局部变量，例如函数中的 i 。

如果一个名字的作用域较大，并且生命周期较长，那么一般会采用稍长的名字。

缩略词避免使用混合写法，例如：HTML等等

## 2.2声明

声明语句定义了程序的各种实体对象以及部分或全部的属性。

四种类型的声明语句：var	const	type	func

​								   变量   常量     类型    函数

一个Go语言编写的程序对应一个或多个.go作为结尾的源文件。每个源文件中以包的声明开始，说明该源文件属于哪个包。

import语句导入依赖的其他包，然后是包一级的类型，变量，常量和函数声明语句（声明顺序无关紧要，但是函数内部的名字必须先声明后使用）。

```go
package main

import "fmt"

const num1 = 1
const num2 = 2
func main() {
	var sum = num1 + num2
	fmt.Println("sum = ",sum)	
}
```

函数的声明由函数名字，参数列表（函数调用者提供的参数变量的具体值），返回值列表，函数体组成。如果函数没有返回值，那么返回值省略。函数执行时，从函数的第一条语句开始执行，直到遇到return语句。如果没有函数返回语句，则一直执行到函数末尾，然后返回至函数的调用者。

函数的调用不同于C语言，在Go语言中函数可以先调用再声明，但在C语言中函数先使用再声明，则需在调用前进行说明。

example：

```go
package main

import "fmt"

func main() {
    const freezingF, boilingF = 32.0, 212.0
    fmt.Printf("%g°F = %g°C\n", freezingF, fToC(freezingF)) // "32°F = 0°C"
    fmt.Printf("%g°F = %g°C\n", boilingF, fToC(boilingF))   // "212°F = 100°C"
}

func fToC(f float64) float64 {
    return (f - 32) * 5 / 9
}
```

## 2.3变量

声明变量关键字：var

example：

```go
var 变量名字 类型 = 表达式 
```

类型和表达式可省略其一，如果省略类型则根据表达式来推导变量的类型信息，如果省略表达式则初始化值会被赋予0。

数值类型对应的零值：0

布尔类型对应的零值：false

字符串类型对应的零值：空字符串

接口或引用类型（slice，指针，map，chan，函数）对应的零值：nil

数组或结构体等聚合类型对应的零值：每个元素或字段对应该类型的零值。

零值初始化机制的优点：不存在未初始化的变量，确保每个声明的变量都有一个良好定义的值，简化代码。在无额外条件的情况下确保边界值的合理性。

example：

```go
var s string
fmt.Println(s)	//""
```

连续声明：

example：

```go
var a,b,c int	//int int int初始值都为0
```

连续声明并且赋值：

example：

```
var num,str,empty = 1,"hello",false
```

