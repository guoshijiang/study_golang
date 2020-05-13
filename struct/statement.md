# go 语言声明方式

声明语句定义了程序的各种实体对象以及部分或全部的属性。Go语言声明包含包声明，变量声明，常量声明，类型和函数实体对象声明。分别对应的关键字是pacKage, var、const、type和func,接下来，我们详细说一下 Go 语言的声明。

### 1.包声明


go语言中包（package）与java中的包（package）非常类似，都是组织代码的方式，而且都和磁盘上的目录结构存在对应关系。go语言中，包名一般为go代码所在的目录名，但是与java不同的是，go语言中包名只有一级，而在java中包名是以点分割的多级目录组合的。

声明语句：package 包名


#### 1.1 包声明的基本规则

同目录下的源码文件的代码包声明语句要一致。也就是说，它们要同属于一个代码包。如果目录中有命令源码文件，那么其他种类的源码文件也应该属于main包。这也是我们能够成功构建和运行它们的前提; 如果不声明为 main, 代码将无法执行。

如下面的函数是无法执行的


	package test

	import (
		"fmt"
	)

	func main(){
		values := []int{4, 93, 84, 85, 80, 37, 81, 93, 27,12}
		fmt.Println(values)
	}


源码文件声明的代码包的名称可以与其所在的目录的名称不同（最好相同）。在针对代码包进行构建时，生成的结果文件的主名称与其父目录的名称一致。对于命令源码文件而言，构建生成的可执行文件的主名称会与其父目录的名称相同。

	C：\gingernet\go_work_space\stady\pkg\test.go


	package pkg

	import "fmt"

	func Hello(name string) {
		fmt.Printf("Hello, %s!\n", name)
	}


Go 源码文件所在的目录相对于 src 目录的相对路径就是它的代码包导入路径，而实际使用其程序实体时给定的限定符要与它声明所属的代码包名称对应；

提示：若是 GOPATH 项目第三方包建议放在项目下面的 vendor 目录。若是 go mod 项目，go mod 会去组织包结构。

### 2.变量声明

Go语言是静态类型语言，因此变量（variable）是有明确类型的，编译器也会检查变量类型的正确性。在数学概念中，变量表示没有固定值且可改变的数。但从计算机系统实现角度来看，变量是一段或多段用来存储数据的内存。

声明语句形式：

`var name type`

其中，var 是声明变量的关键字，name 是变量名，type 是变量的类型。

需要注意的是，Go语言和许多编程语言不同，它在声明变量时将变量的类型放在变量的名称之后。这样做的好处就是可以避免像C语言中那样含糊不清的声明形式，例如：int* a, b; 。其中只有 a 是指针而 b 不是。如果你想要这两个变量都是指针，则需要将它们分开书写。而在 Go 中，则可以和轻松地将它们都声明为指针类型：

`var a, b *int`

#### 2.1 变量声明的几种方式

1.常用格式

`var 变量名 变量类型`

如：

`var (
    a int
    b string
    c []float32
    d func() bool
    e struct {
        x int
    }
)`

2.简短格式 
`名字 := 表达式`;
如
`a := 10` 

需要注意的是，简短模式（short variable declaration）有以下限制：
定义变量，同时显式初始化。
不能提供数据类型。
只能用在函数内部。

和 var 形式声明语句一样，简短变量声明语句也可以用来声明和初始化一组变量：
i, j := 0, 1

下面通过一段代码来演示简短格式变量声明的基本样式。
func main() {
   x:=100
   a,s:=1, "abc"
}

因为简洁和灵活的特点，简短变量声明被广泛用于大部分的局部变量的声明和初始化。var 形式的声明语句往往是用于需要显式指定变量类型地方，或者因为变量稍后会被重新赋值而初始值无关紧要的地方。


### 3.常量声明

常量的声明使用关键字 const, 声明的时候可以带类型，也可以不带类型；常用格式如下：

`const 名字 类型 `

如：

声明一个常量

`const MAX = 4096`

声明一个指定类型的常量

`const LIMIT int16 = 1024
const LIMIT2 = int16(1024)`

声明一组常量

`const (
    start  = 0x1 
    resume = 0x2 
    stop   = 0x4 
)
`

声明一组指定类型的常量

`const (
    start  int8 = 0x1 
    resume int8 = 0x2 
    stop   int8 = 0x4 
)`

用iota简化上面的写法

`const (
    start  int8 = 1 << iota
    resume
    stop
)`


### 4.类型声明

类型声明使用 type 关键字，类型声明即绑定一个标识符（新类型名称）到一个类型。有两种形式：类型别名声明、新类型定义。

类型别名声明
很简单：在类型别名和类型之间使用等号（=）

`type (
	nodeList = []*Node  
	Polar    = polar    
)`

新类型定义

创建一个新的类型，这个类型和给定的已存在的（旧）类型 有相同的底层类型和操作，并用一个新的标识符来表示这个新类型。这样的新类型也叫做自定义类型

`
type (
	Point struct{ x, y float64 }  
	polar Point                  
)

type TreeNode struct {
	left, right *TreeNode
	value *Comparable
}

type Block interface {
	BlockSize() int
	Encrypt(src, dst []byte)
	Decrypt(src, dst []byte)
}`


### 5.函数对象声明

函数声明使用 func 关键字，函数的基本组成为以下 6 部分

关键字 func(必须写)、
函数名、
参数列表、
返回值、
函数体
返回语句

形式如下：

`func 函数名(形式参数列表)(返回值列表){
        函数体
}`

例如：

`func hypot(x, y float64) float64 {
    return math.Sqrt(x*x + y*y)
}
fmt.Println(hypot(3,4)) // "5"`

这里我们对包，变量，常量，类型，函数等都是简单的介绍，说明他们的声明方式，关于他们在后面的课程中将会有详细的介绍
   
 
