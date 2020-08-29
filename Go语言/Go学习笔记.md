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

