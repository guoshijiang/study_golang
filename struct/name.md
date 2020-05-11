# Go 命名规范


### 1 常规情况

Go语言中的包名、函数名、变量名、常量名、类型名、语句标号和包名等所有的命名，都遵循一个简单的命名规则：一个名字必须以一个字母（Unicode字母）或下划线开头，后面可以跟任意数量的字母、数字或下划线。大写字母和小写字母是不同的：heapSort和Heapsort是两个不同的名字。

Go语言中类似if和switch的关键字有25个；关键字不能用于自定义名字，只能在特定语法结构中使用。

    break      default       func     interface   select
    case       defer         go       map         struct
    chan       else          goto     package     switch
    const      fallthrough   if       range       type
    continue   for           import   return      var
    
此外，还有大约30多个预定义的名字，比如int和true等，主要对应内建的常量、类型和函数。

内建常量: `true false iota nil`

内建类型: `int int8 int16 int32 int64
          uint uint8 uint16 uint32 uint64 uintptr
          float32 float64 complex128 complex64
          bool byte rune string error`

内建函数: `make len cap new append copy close delete
          complex real imag
          panic recover`
          
这些内部预先定义的名字并不是关键字，你可以在定义中重新使用它们。在一些特殊的场景中重新定义它们也是有意义的，但是也要注意避免过度而引起语义混乱。

### 2.公有和私有性

实际上，Go 语言中没有 public 和 private 等这样的关键字，它是通过字母大小写来控制可见性的，如果定义的常量、变量、类型、接口、结构、函数等的名称是大写字母开头表示能被其它包访问或调用（相当于public），非大写开头就只能在包内使用（相当于private，变量或常量也可以下划线开头）

如：

可访问

    package main

    import (
      "fmt"
      "stady/pkg"
    )

    func main(){
      values := []int{4, 93, 84, 85, 80, 37, 81, 93, 27,12}
      fmt.Println(values)
      fmt.Println(pkg.BubbleSort(values))
    }

    package calc

    func BubbleSort(values []int) []int {
      for i := 0; i < len(values)-1; i++ {
        for j := i+1; j < len(values); j++ {
          if  values[i]>values[j]{
            values[i],values[j] = values[j],values[i]
          }
        }
      }
      return values
    }

私有的, 私有的只能在当前包内访问，在其他包里面访问将会报错

     package calc

     func bubble_sort(values []int) []int {
        for i := 0; i < len(values)-1; i++ {
          for j := i+1; j < len(values); j++ {
            if  values[i]>values[j]{
              values[i],values[j] = values[j],values[i]
            }
          }
        }
        return values
      }
      
以上两段代码中，第一个使用了驼峰命令，第二个使用了下划线命名，这也是目前编程里面比较常规的规则，一般情况下，变量，函数，包名等我在命名中尽量使用驼峰和下滑线命令的方式。


### 3.常用的命名方式

#### 3.1 文件命名

文件命名一律采用小写，不用驼峰式，尽量见名思义，看见文件名就可以知道这个文件下的大概内容。其中测试文件以test.go结尾，除测试文件外，命名不出现。

例子：

`stringutil.go， stringutil_test.go`

#### 3.2 包名package
包名用小写,使用短命名,尽量和标准库不要冲突。包名统一使用单数形式。

#### 3.3变量

变量命名一般采用驼峰式，当遇到特有名词（缩写或简称，如DNS）的时候，特有名词根据是否私有全部大写或小写。

例子：

`apiClient、URLString`

#### 3.4 常量
同变量规则，力求语义表达完整清楚，不要嫌名字长。如果模块复杂，为避免混淆，可按功能统一定义在package下的一个文件中。

#### 3.5 接口
单个函数的接口名以 er 为后缀

`
type Reader interface {
    Read(p []byte) (n int, err error)
}
`

两个函数的接口名综合两个函数名，如:

`
type WriteFlusher interface {
    Write([]byte) (int, error)
    Flush() error
}
`
三个以上函数的接口名类似于结构体名，如:

`
type Car interface {
    Start() 
    Stop()
    Drive()
}
`

#### 3.6 结构体
结构体名应该是名词或名词短语，如Account,Book，避免使用Manager这样的。如果该数据结构需要序列化，如json， 则首字母大写， 包括里面的字段。

#### 3.7 方法
方法名应该是动词或动词短语，采用驼峰式。将功能及必要的参数体现在名字中， 不要嫌长， 如updateById，getUserInfo.如果是结构体方法，那么 Receiver 的名称应该缩写，一般使用一个或者两个字符作为 Receiver 的名称。如果 Receiver 是指针， 那么统一使用p。 如：

`
func (f foo) method() {
    ...
}

func (p *foo) method() {
    ...
}
`
对于Receiver命名应该统一， 要么都使用值， 要么都用指针。

#### 3.8 注释
每个包都应该有一个包注释，位于 package 之前。如果同一个包有多个文件，只需要在一个文件中编写即可；如果你想在每个文件中的头部加上注释，需要在版权注释和 Package前面加一个空行，否则版权注释会作为Package的注释。如：

`
// Copyright 2009 The Go Authors. All rights reserved.
// Use of this source code is governed by a BSD-style
// license that can be found in the LICENSE file.
package net
`
每个以大写字母开头（即可以导出）的方法应该有注释，且以该函数名开头。如：

`
// Get 会响应对应路由转发过来的 get 请求
func (c *Controller) Get() {
    ...
}
`

大写字母开头的方法以为着是可供调用的公共方法，如果你的方法想只在本包内掉用，请以小写字母开发。如:

`
func (c *Controller) curl() {
    ...
}
`

注释应该用一个完整的句子，注释的第一个单词应该是要注释的指示符，以便在 godoc 中容易查找。

注释应该以一个句点 . 结束。

