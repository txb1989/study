[学习文档http://www.topgoer.com/](http://www.topgoer.com/)

#### Go语言特点

- 继承了C语言的很多理念
- 引入包的概念
- 垃圾回收机制
- **天然并发**
  - 语言层面支持还，实现简单
  - goroutine，轻量级线程可实现大并发处理，高效利用多喝
  - 基于CPS并发模型实现
- 支持管道通讯机制
- 函数可以返回多个值
- 加入了一些新特性如切片、defer等

#### GO开发工具

- VS Code ： 安装插件
- Sublime Text
- Vim
- Emacs
- Ecipse：安装插件
- LiteIDE：国人开发，免费
- JetBrains：付费+安装插件

##### 安装VSCODE插件失败

​	步骤原理就是自己从Github上把源码下载下来，仿佛对应的目录安装。

​	第一步：现在自己的`GOPATH`的`src`目录下创建`golang.org/x`目录

​	第二步：在终端`/cmd中cd`到`GOPATH/src/golang.org/x`目录下

​	第三步：执行`git clone https://github.com/golang/tools.git tools`命令

​	第四步：执行`git clone https://github.com/golang/lint.git`命令

​	第五步：按下`Ctrl/Command+Shift+P`再次执行`Go:Install/Update Tools`命令，在弹出的窗口全选并点击确定，这一次的安装都会SUCCESSED了。

#### 第一个Go程序

```go
package main    //包名
import "fmt"    //引入其他包
func  main()  {  //main函数
	fmt.Println("Hello world") //打印语句
}
```

#### GO包引用

1. Golang Package

     在 go1.12 之前，安装 golang 之后，需要配置两个环境变量----*GOROOT* 和*GOPATH*。前者是 go 安装后的所在的路径，后者是开发中自己配置的，用于存放go 源代码的地方。在 GOPATH 路径内，有三个文件夹，分别是
   
    - bin：go编译后的可执行文件所在目录
    - pkg：编译非main包的中间连接文件
    - src：go项目源代码
    
2. Golang Module

#### Go关键字

1. 25个关键字

   ```go
   break        default      func         interface    select
   case         defer        go           map          struct
   chan         else         goto         package      switch
   const        fallthrough  if           range        type
   continue     for          import       return       var
   ```

   

2. 37个保留字

   ```go
   Constants:    true  false  iota  nil
   
   Types:    int  int8  int16  int32  int64  
             uint  uint8  uint16  uint32  uint64  uintptr
             float32  float64  complex128  complex64
             bool  byte  rune  string  error
   
   Functions:   make  len  cap  new  append  copy  close                    delete
                complex  real  imag
                panic  recover
   ```

   

3. 可见性

   1. 声明在函数内部，是函数的本地值，类似private
   2. 声明在函数外部，是对当前包可见(包内所有.go文件都可见)的全局值，类似protect
   3. 声明在函数外部且首字母大写是所有包可见的全局值,类似public

4. 声明

   - var :声明变量
   - const：声明常量
   - type：声明类型
   - func：声明函数

5. 内置类型

   ```go
   bool
   int(32 or 64), int8, int16, int32, int64
   uint(32 or 64), uint8(byte), uint16, uint32, uint64
   float32, float64
   string
   complex64, complex128
   array    -- 固定长度的数组
   ```

6. 引用类型

   ```go
   slice   -- 序列数组(最常用)
   map     -- 映射
   chan    -- 管道
   ```

   

7. 内置函数

   ```go
   append          -- 用来追加元素到数组、slice中,返回修改后的数组、slice
   close           -- 主要用来关闭channel
   delete            -- 从map中删除key对应的value
   panic            -- 停止常规的goroutine  （panic和recover：用来做错误处理）
   recover         -- 允许程序定义goroutine的panic动作
   real            -- 返回complex的实部   （complex、real imag：用于创建和操作复数）
   imag            -- 返回complex的虚部
   make            -- 用来分配内存，返回Type本身(只能应用于slice, map, channel)
   new                -- 用来分配内存，主要用来分配值类型，比如int、struct。返回指向Type的指针
   cap                -- capacity是容量的意思，用于返回某个类型的最大容量（只能用于切片和 map）
   copy            -- 用于复制和连接slice，返回复制的数目
   len                -- 来求长度，比如string、array、slice、map、channel ，返回长度
   print、println     -- 底层打印函数，在部署环境中建议使用 fmt 包
   ```

   #### init函数和main函数

   - init函数

     - init函数是用于程序执行前做包的初始化的函数
     - 每个包可以拥有多个init函数
     - 包的每个源文件也可以拥有多个init函数
     - 同一个包中多个init函数的执行顺序go语言没有明确的定义(说明)，同一个包的源文件的init函数按照从上到下的顺序执行
     - 不同包的init函数按照包导入的依赖关系决定该初始化函数的执行顺序
     - init函数不能被其他函数调用，而是在main函数执行之前，自动被调用

   - main函数

     Go语言程序的默认入口函数(主函数)：func main()

     ```go
     func main() {
         //函数体
     }
     ```

   - init函数和main函数的异同

     - 相同

        两个函数在定义时不能有任何的参数和返回值，且Go程序自动调用。

     - 不同

       1.  init可以应用于任意包中，且可以重复定义多个。
       2.  main函数只能用于main包中，且只能定义一个。

#### GO语言命令

- go env：用于打印GO语言的环境信息
- go run：编译并运行源码文件
- go get：可以根据要求和实际情况从互联网上下载或更新指定的代码包及其依赖包，并对它们进行编译和安装。
-  go build：编译我们指定的源码文件或代码包以及它们的依赖包。
- go install：编译并安装指定的代码包及它们的依赖包。
- go clean：删除掉执行其它命令时产生的一些文件和目录。
- go doc：打印附于Go语言程序实体上的文档。我们可以通过把程序实体的标识符作为该命令的参数来达到查看其文档的目的。
- go test：对Go语言编写的程序进行测试。
- go list：列出指定的代码包的信息。
- go fix：把指定代码包的所有Go语言源码文件中的旧版本代码修正为新版本的代码。
- go vet：用于检查Go语言源码中静态错误的简单工具。
- o tool pprof：命令来交互式的访问概要文件的内容。

#### 运算符

1. 算术运算符

   | 运算符 | 描述 |
   | ------ | ---- |
   | +      | 相加 |
   | -      | 相减 |
   | *      | 相乘 |
   | /      | 相除 |
   | %      | 求余 |

2. 关系运算符

   | 运算符 | 描述                                                         |
   | :----: | :----------------------------------------------------------- |
   |   ==   | 检查两个值是否相等，如果相等返回 True 否则返回 False。       |
   |   !=   | 检查两个值是否不相等，如果不相等返回 True 否则返回 False。   |
   |   >    | 检查左边值是否大于右边值，如果是返回 True 否则返回 False。   |
   |   >=   | 检查左边值是否大于等于右边值，如果是返回 True 否则返回 False。 |
   |   <    | 检查左边值是否小于右边值，如果是返回 True 否则返回 False。   |
   |   <=   | 检查左边值是否小于等于右边值，如果是返回 True 否则返回 False。 |

3. 逻辑运算符

   | 运算符 | 描述                                                         |
   | :----: | ------------------------------------------------------------ |
   |   &&   | 逻辑 AND 运算符。 如果两边的操作数都是 True，则为 True，否则为 False。 |
   |   ll   | 逻辑 OR 运算符。 如果两边的操作数有一个 True，则为 True，否则为 False。 |
   |   !    | 逻辑 NOT 运算符。 如果条件为 True，则为 False，否则为 True。 |

4. 位运算符

   | 运算符 | 描述                                                         |
   | :----: | ------------------------------------------------------------ |
   |   &    | 参与运算的两数各对应的二进位相与。（两位均为1才为1）         |
   |   l    | 参与运算的两数各对应的二进位相或。（两位有一个为1就为1）     |
   |   ^    | 参与运算的两数各对应的二进位相异或，当两对应的二进位相异时，结果为1。（两位不一样则为1） |
   |   <<   | 左移n位就是乘以2的n次方。“a<<b”是把a的各二进位全部左移b位，高位丢弃，低位补0。 |
   |   >>   | 右移n位就是除以2的n次方。“a>>b”是把a的各二进位全部右移b位。  |

5. 赋值运算符

   | 运算符 | 描述                                           |
   | :----: | ---------------------------------------------- |
   |   =    | 简单的赋值运算符，将一个表达式的值赋给一个左值 |
   |   +=   | 相加后再赋值                                   |
   |   -=   | 相减后再赋值                                   |
   |   *=   | 相乘后再赋值                                   |
   |   /=   | 相除后再赋值                                   |
   |   %=   | 求余后再赋值                                   |
   |  <<=   | 左移后赋值                                     |
   |  >>=   | 右移后赋值                                     |
   |   &=   | 按位与后赋值                                   |
   |   l=   | 按位或后赋值                                   |
   |   ^=   | 按位异或后赋值                                 |

#### 下划线(_)

“_”是特殊标识符，用来忽略结果。

- import中使用"_"

  `import _ "./包名"`表示只会执行其中的init方法，并不会导入包里面的方法。所以无法使用包名来调用其他的函数

- 代码中使用"_"

  ```go
  package main
  
  import (
      "os"
  )
  
  func main() {
      buf := make([]byte, 1024)
      f, _ := os.Open("/Users/***/Desktop/text.txt")
      defer f.Close()
      for {
          n, _ := f.Read(buf)
          if n == 0 {
              break    
  
          }
          os.Stdout.Write(buf[:n])
      }
  }
  ```

  下划线就是忽略变量，比如os.open的返回值未*os.File,error。普通的写法是`f,err:=os.Open("xxxx")`此时如果不需要返回的错误值就可以使用`f,_:=os.Open("xxxx")`忽略error变量

#### 变量

  1. 变量声明

    Go语言中的每一个变量都有自己的类型，并且变量必须经过声明才能开始使用。
  - 变量标准声明
  ```go
    var 变量名 变量类型
  ```
  - 批量声明

    ```go
       var (
            a string
            b int
            c bool
            d float32
        )
    ```
  2. 变量初始化
  
     - 整型和浮点型变量的默认值为0。
     
     - 字符串变量的默认值为空字符串。
     
     - 布尔型变量默认为`false`。
     
     - 切片、函数、指针变量的默认为`nil`。
     
       标准格式
     
       ```go
       var 变量名 类型 = 表达式
       //举例
       var name string = "pprof.cn"
       var sex int = 1
       ```
     
       **类型推导**：有时候我们会将变量的类型省略，这个时候编译器会根据等号右边的值来推导变量的类型完成初始化。
     
       ```go
           var name = "pprof.cn"
           var sex = 1
       ```
     
       **短变量声明**：在函数内部，可以使用更简略的 := 方式声明并初始化变量。
     
       注意：
       
       - 局部变量前面不要使用var关键字
       - 局部变量不需要指定类型，赋值的时候已经确定了类型
       
       ```go
       package main
       
       import (
           "fmt"
       )
       // 全局变量m
       var m = 100
       
       func main() {
         n := 10
           m := 200 // 此处声明局部变量m
         array := [5]int{1, 2, 3}
           fmt.Println(m, n)
       }
       ```
       
       **匿名变量**：在使用多重赋值时，如果想要忽略某个值，可以使用`匿名变量（anonymous variable）`。 匿名变量用一个下划线_表示
       
       ```go
       func foo() (int, string) {
           return 10, "Q1mi"
       }
       func main() {
         x, _ := foo()
           _, y := foo()
        fmt.Println("x=", x)
        fmt.Println("y=", y)
       ```

    }
    ```
    
    匿名变量不占用命名空间，不会分配内存，所以匿名变量之间不存在重复声明。

  3. 常量
  
     常量适用const进行声明，常量在定义的时候必须赋值，并且赋值以后在真个程序运行期间值不能进行修改，如：
  
     ```go
         const pi = 3.1415
         const e = 2.7182
     ```
  
     如果需要同时声明多个常量，如果省略了值则表示和上面一行的值相同
  
     ```go
         const (
             n1 = 100
             n2
             n3
         )
     //n1、n2、n3的值都是100
     ```
  
  4. iota
  
     `iota`是`go`语言的常量计数器，只能在常量的表达式中使用。 `iota`在`const`关键字出现时将被重置为`0`。`const`中每新增一行常量声明将使`iota`计数一次(`iota`可理解为`const`语句块中的行索引)。 使用`iota`能简化定义，在定义枚举时很有用。
  
     ```go
       const (
                 n1 = iota //0
                 n2        //1
                 n3        //2
                 n4        //3
             )
     ```
  
     使用_跳过某些值
  
     ```go
         const (
                 n1 = iota //0
                 n2        //1
                 _
                 n4        //3
             )
     ```
  
     `iota`声明中间插队
  
     ```go
         const (
                 n1 = iota //0
                 n2 = 100  //100
                 n3 = iota //2
                 n4        //3
             )
         const n5 = iota //0
     ```
  
     定义数量级 （这里的`<<`表示左移操作，`1<<10`表示将`1`的二进制表示向左移`10`位，也就是由`1`变成了`10000000000`，也就是十进制的`1024`。同理`2<<2`表示将`2`的二进制表示向左移`2`位，也就是由`10`变成了`1000`，也就是十进制的`8`。）
  
     ```go
         const (
                 _  = iota
                 KB = 1 << (10 * iota)
                 MB = 1 << (10 * iota)
                 GB = 1 << (10 * iota)
                 TB = 1 << (10 * iota)
                 PB = 1 << (10 * iota)
             )
     ```
  
     多个`iota`定义在一行
  
     ```go
         const (
                 a, b = iota + 1, iota + 2 //1,2
                 c, d                      //2,3
                 e, f                      //3,4
             )
     ```

#### 基本类型

| 类型          | 长度(字节) | 默认值 | 说明                                      |
| ------------- | ---------- | ------ | ----------------------------------------- |
| bool          | 1          | false  |                                           |
| byte          | 1          | 0      | uint8                                     |
| rune          | 4          | 0      | Unicode Code Point, int32                 |
| int, uint     | 4或8       | 0      | 32 或 64 位                               |
| int8, uint8   | 1          | 0      | -128 ~ 127, 0 ~ 255，byte是uint8 的别名   |
| int16, uint16 | 2          | 0      | -32768 ~ 32767, 0 ~ 65535                 |
| int32, uint32 | 4          | 0      | -21亿~ 21亿, 0 ~ 42亿，rune是int32 的别名 |
| int64, uint64 | 8          | 0      |                                           |
| float32       | 4          | 0.0    |                                           |
| float64       | 8          | 0.0    |                                           |
| complex64     | 8          |        |                                           |
| complex128    | 16         |        |                                           |
| uintptr       | 4或8       |        | 以存储指针的 uint32 或 uint64 整数        |
| array         |            |        | 值类型                                    |
| struct        |            |        | 值类型                                    |
| string        |            | ""     | UTF-8 字符串                              |
| slice         |            | nil    | 引用类型                                  |
| map           |            | nil    | 引用类型                                  |
| channel       |            | nil    | 引用类型                                  |
| interface     |            | nil    | 接口                                      |
| function      |            | nil    | 函数                                      |

1. 整型

   整型分为以下两个大类： 按长度分为：`int8`、`int16`、`int32`、`int64`对应的无符号整型：`uint8`、`uint16`、`uint32`、`uint64`

2. 浮点型

   Go语言支持两种浮点型数：`float32`和`float64`。这两种浮点型数据格式遵循`IEEE 754`标准： `float32` 的浮点数的最大范围约为`3.4e38`，可以使用常量定义：`math.MaxFloat32`。 `float64` 的浮点数的最大范围约为 `1.8e308`，可以使用一个常量定义：`math.MaxFloat64`。

3. 复数

   `complex64`和`complex128`复数有实部和虚部，`complex64`的实部和虚部为32位，`complex128`的实部和虚部为64位。

4. 布尔值

   Go语言中以`bool`类型进行声明布尔型数据，布尔型数据只有`true（真）`和`false（假）`两个值。

   - 布尔类型变量的默认值为false。
   - Go 语言中不允许将整型强制转换为布尔型.
   - 布尔型无法参与数值运算，也无法与其他类型进行转换。

5. 字符串

   Go 语言里的字符串的内部实现使用UTF-8编码。 字符串的值为双引号(")中的内容，可以在Go语言的源码中直接添加非`ASCII`码字符

6. 字符串转义

   | 转义 | 含义                               |
   | ---- | ---------------------------------- |
   | \r   | 回车符（返回行首）                 |
   | \n   | 换行符（直接跳到下一行的同列位置） |
   | \t   | 制表符                             |
   | \'   | 单引号                             |
   | \"   | 双引号                             |
   | \    | 反斜杠                             |

7. 多行字符串

   Go语言中要定义一个多行字符串时，就必须使用反引号\`，反引号间换行将被作为字符串中的换行，但是所有的转义字符均无效，文本将会原样输出。

   ```go
       s1 := `第一行
       第二行
       第三行
       `
       fmt.Println(s1)
   ```

8. 字符串常用操作

   | 方法                                | 介绍           |
   | ----------------------------------- | -------------- |
   | len(str)                            | 求长度         |
   | +或fmt.Sprintf                      | 拼接字符串     |
   | strings.Split                       | 分割           |
   | strings.Contains                    | 判断是否包含   |
   | strings.HasPrefix,strings.HasSuffix | 前缀/后缀判断  |
   | strings.Index(),strings.LastIndex() | 子串出现的位置 |
   | strings.Join(a[]string, sep string) | join操作       |

9. byte和rune类型

   Go 语言的字符有以下两种：

   - uint8类型，或者叫 byte 型，代表了ASCII码的一个字符。
   - rune类型，代表一个 UTF-8字符。

   当需要处理中文、日文或者其他复合字符时，则需要用到`rune`类型。`rune`类型实际是一个`int32`。 Go 使用了特殊的 `rune` 类型来处理 `Unicode`，让基于 `Unicode`的文本处理更为方便，也可以使用 `byte` 型进行默认字符串处理，性能和扩展性都有照顾

   **字符串是不能修改的 字符串是由byte字节组成，所以字符串的长度是byte字节的长度。 rune类型用来表示utf8字符，一个rune字符由一个或多个byte组成。**

10. 修改字符串

    要修改字符串，需要先将其转换成`[]rune或[]byte`，完成后再转换为`string`。无论哪种转换，都会重新分配内存，并复制字节数组。

11. 类型转换

    Go语言中只有强制类型转换，没有隐式类型转换。该语法只能在两个类型之间支持相互转换的时候使用。

    强制类型转换的基本语法如下：

    ```go
      T(表达式)
    ```

#### 数组Array

1. 数组特点
   - 相同类型的固定长度
   
   - 数组定义：var a [len]int，比如：var a [5]int，数组长度必须是常量，且是类型的组成部分。一旦定义，长度不能变。
   
   - 长度是数组类型的一部分，因此，var a[5] int和var a[10]int是不同的类型。
   
   - 数组可以通过下标进行访问，下标是从0开始，最后一个元素下标是：len-1
   
   - 访问越界，如果下标在数组合法范围之外，则触发访问越界，会panic
   
   - 数组是值类型，赋值和传参会复制整个数组，而不是指针。因此改变副本的值，不会改变本身的值。
   
   - 支持 "=="、"!=" 操作符，因为内存总是被初始化过的。
   
   - 指针数组 [n]*T，数组指针 *[n]T。
   
     ```go
     var array0 [5]in = [5]int{1,2,3}//空余的为0
     var array1 = [5]int{1,2,3,4}
     var array2 = [...]int{1,2,3,4,5,6}//由内容确定长度
     var str = [5]string{3:"test",4:"number 5"}//按下标初始化值，下标未3的初始化test，下标未4值的初始化“number 5”
     //多维数组
   var array1 [][5]int = [][5]int{{1, 2, 3, 4, 5}, {2, 3, 4}, {1, 2, 3}}//3行5列数组
     //局部：
     a := [2][3]int{{1, 2, 3}, {4, 5, 6}}
     b := [...][2]int{{1, 1}, {2, 2}, {3, 3}} // 第 2 纬度不能用 "..."。
     ```
     

#### 切片Slice

slice 并不是数组或数组指针。它通过内部指针和相关属性引用数组片段，以实现变长方案。

1. 切片：切片是数组的一个引用，因此切片是引用类型。但自身是结构体，值拷贝传递。
2. 切片的长度可以改变，因此，切片是一个可变的数组。
3. 切片遍历方式和数组一样，可以用len()求长度。表示可用元素数量，读写操作不能超过该限制。 
4. cap可以求出slice最大扩张容量，不能超出数组限制。0 <= len(slice) <= len(array)，其中array是slice引用的数组。
5. 切片的定义：var 变量名 []类型，比如 var str []string  var arr []int。
6. 如果 slice == nil，那么 len、cap 结果都等于 0。

##### 1. 切片创建

```go
s2 := []int{}
var s3 []int = make([]int, 3)
fmt.Println(s2, s3)
var s4 []int = make([]int, 5, 5)
fmt.Println(s4)

s5 := []int{1, 2, 3}
fmt.Println(s5)

arr := [...]int{1, 2, 3, 4, 5}
var s6 []int
s6 = arr[1:4]
fmt.Println(s6)
```
##### 2. 切片初始化

![切片](http://www.topgoer.com/static/3.8/0.jpg)

```go
package main

import (
    "fmt"
)

var arr = [...]int{0, 1, 2, 3, 4, 5, 6, 7, 8, 9}
var slice0 []int = arr[2:8]
var slice1 []int = arr[0:6]        //可以简写为 var slice []int = arr[:end]
var slice2 []int = arr[5:10]       //可以简写为 var slice[]int = arr[start:]
var slice3 []int = arr[0:len(arr)] //var slice []int = arr[:]
var slice4 = arr[:len(arr)-1]      //去掉切片的最后一个元素
func main() {
    fmt.Printf("全局变量：arr %v\n", arr)
    fmt.Printf("全局变量：slice0 %v\n", slice0)
    fmt.Printf("全局变量：slice1 %v\n", slice1)
    fmt.Printf("全局变量：slice2 %v\n", slice2)
    fmt.Printf("全局变量：slice3 %v\n", slice3)
    fmt.Printf("全局变量：slice4 %v\n", slice4)
    fmt.Printf("-----------------------------------\n")
    arr2 := [...]int{9, 8, 7, 6, 5, 4, 3, 2, 1, 0}
    slice5 := arr[2:8]
    slice6 := arr[0:6]         //可以简写为 slice := arr[:end]
    slice7 := arr[5:10]        //可以简写为 slice := arr[start:]
    slice8 := arr[0:len(arr)]  //slice := arr[:]
    slice9 := arr[:len(arr)-1] //去掉切片的最后一个元素
    fmt.Printf("局部变量： arr2 %v\n", arr2)
    fmt.Printf("局部变量： slice5 %v\n", slice5)
    fmt.Printf("局部变量： slice6 %v\n", slice6)
    fmt.Printf("局部变量： slice7 %v\n", slice7)
    fmt.Printf("局部变量： slice8 %v\n", slice8)
    fmt.Printf("局部变量： slice9 %v\n", slice9)
}
```
##### 3. make创建切片

```go
    var slice []type = make([]type, len)
    slice  := make([]type, len)
    slice  := make([]type, len, cap)
```

![make创建切片](http://www.topgoer.com/static/3.8/1.jpg)

make(类型，切片长度，底层数组长度)

make(类型，切片长度)

![切片内存布局](http://www.topgoer.com/static/3.8/2.jpg)

切片的操作实际上是底层数组的操作。所以改变切片中元素的值，原来数组中对应的值也会改变。如：

```go
package main

import (
    "fmt"
)

func main() {
    data := [...]int{0, 1, 2, 3, 4, 5}

    s := data[2:4]
    s[0] += 100
    s[1] += 200

    fmt.Println(s)
    fmt.Println(data)
}
```

输出结果：

```console
[102 203]
[0 1 102 203 4 5]
```

#### 指针

Go语言中的指针不能进行偏移和运算，是安全指针。Go语言中的函数传参都是值拷贝，当我们想要修改某个变量的时候，我们可以创建一个指向该变量地址的指针变量。传递数据使用指针，而无须拷贝数据。Go语言中的指针操作非常简单，只需要记住两个符号：`&`（取地址）和`*`（根据地址取值）。

##### 1、指针地址

​	每个变量在运行时都拥有一个地址，这个地址代表变量在内存中的位置。Go语言中使用&字符放在变量前面对变量进行“取地址”操作。 

```go
ptr := &v    // v的类型为T
v:代表被取地址的变量，类型为T
ptr:用于接收地址的变量，ptr的类型就为*T，称做T的指针类型。*代表指针。
//举例

var ga int = 100
var pa *int = &ga
func main() {
	fmt.Printf("ga:%d ptr-ga:%p \n", ga, &ga)
	fmt.Printf("pa:%d ptr-pa:%p value-pa:%p \n", pa, &pa, *pa)
	fmt.Println("----------------------------------")
    a := 10
    b := &a
    fmt.Printf("a:%d ptr:%p\n", a, &a) // a:10 ptr:0xc00001a078
    fmt.Printf("b:%d ptr-b:%p value-b:%p \n", b, &b, *b) // b:0xc00001a078 type:*int
    fmt.Println(&b)                    // 0xc00000e018
}
```

![指针](http://www.topgoer.com/static/3.9/1.png)

##### 2、指针类型

​		Go语言中的值类型`（int、float、bool、string、array、struct）`都有对应的指针类型，如：`*int、*int64、*string`等。取变量指针的语法如下：

##### 3、指针取值

​		在对普通变量使用&操作符取地址后会获得这个变量的指针，然后可以对指针使用`*`操作，也就是指针取值，代码如下。

```go
func main() {
    //指针取值
    a := 10
    b := &a // 取变量a的地址，将指针保存到b中
    fmt.Printf("type of b:%T\n", b)
    c := *b // 指针取值（根据指针去内存取值）
    fmt.Printf("type of c:%T\n", c)
    fmt.Printf("value of c:%v\n", c)
}
```

***对一个非指针类型的变量进行去追操作，编译器会报错***

1.对变量进行取地址（&）操作，可以获得这个变量的指针变量。
2.指针变量的值是指针地址。
3.对指针变量进行取值（*）操作，可以获得指针变量指向的原变量的值。

##### 4、空指针nil

​		当一个指针被定义后没有分配到任何变量时，它的值为 nil

#### new和make

​		在Go语言中对于引用类型的变量，我们在使用的时候不仅要声明它，还要为它分配内存空间，否则我们的值就没办法存储。而对于值类型的声明不需要分配内存空间，是因为它们在声明的时候已经默认分配好了内存空间。要分配内存，就引出来今天的new和make。 Go语言中new和make是内建的两个函数，主要用来分配内存。

##### 1、new

​		new是一个内置的函数，它的函数签名如下：

```go
func new(Type) *Type
//1.Type表示类型，new函数只接受一个参数，这个参数是一个类型
//2.*Type表示类型指针，new函数返回一个指向该类型内存地址的指针。
```

***new函数不太常用，使用new函数得到的是一个类型的指针，并且该指针对应的值为该类型的零值。***

##### 2、make

make也是用于内存分配的，区别于new，它只用于slice、map以及chan的内存创建，而且它返回的类型就是这三个类型本身，而不是他们的指针类型，因为这三种类型就是引用类型，所以就没有必要返回他们的指针了。make函数的函数签名如下：

```go
func make(t Type, size ...IntegerType) Type
```

make函数是无可替代的，我们在使用slice、map以及channel的时候，都需要使用make进行初始化，然后才可以对它们进行操作。

```go
func main() {
    var b map[string]int
    b = make(map[string]int, 10)
    b["测试"] = 100
    fmt.Println(b)
}
```

##### 3、new和make区别
1.二者都是用来做内存分配的。
2.make只用于slice、map以及channel的初始化，返回的还是这三个引用类型本身；
3.而new用于类型的内存分配，并且内存对应的值为类型零值，返回的是指向类型的指针。

#### Map

Go语言中的map是引用类型，必须初始化才能使用。map的定义语法如下

```go
    map[KeyType]ValueType
    //KeyType:表示键的类型。
    //ValueType:表示键对应的值的类型。
```

初始化语法：

```go
   make(map[KeyType]ValueType, [cap])
   //其中cap表示map的容量，该参数虽然不是必须的，但是我们应该在初始化map的时候就为其指定一个合适的容量。
```