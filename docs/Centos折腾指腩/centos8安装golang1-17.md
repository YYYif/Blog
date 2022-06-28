1、访问go的官网
*   Go官网下载地址：[https://studygolang.com/dl](https://studygolang.com/dl)
*   Go官方镜像站（推荐）: [https://golang.google.cn/dl/](https://golang.google.cn/dl/)
```
wget https://studygolang.com/dl/golang/go1.17.7.linux-amd64.tar.gz
```

2、解压到/usr/local文件夹下
```
tar -C /usr/local -xzf go1.17.7.linux-amd64.tar.gz
```

3、gopath文件夹
home目录下, 建立一个名为gopath的目录，gopath下再建立3个目录pkg、bin、src
执行以下命令：
```
mkdir -p /home/go/src /home/go/pkg /home/go/bin
```

4、编辑环境变量 
由于我用的是oh-my-zsh，所以我编辑.zshrc，也可以直接编辑vi /etc/profile
```
vim ~/.zshrc
```
切换到文件最后按下"o"键，下一行开始插入
export GOROOT=/usr/local/go
export GOPATH=/home/gopath
export PATH=$PATH:$GOROOT/bin:$GOPATH/bin

5、配置生效 
source ~/.zshrc
或者source /etc/profile
取决于你上面编辑的哪个文件

6、go env执行是否生效

7、vim main.go
```
package main

import "fmt"

func main() {
    fmt.Println("hello, world")
}
```

gopath设定
代码开发必须在go path src目录下，不然，就有问题。
依赖手动管理
依赖包没有版本可言

8、go mod
官方定义：
模块是相关Go包的集合。modules是源代码交换和版本控制的单元。 go命令直接支持使用modules，包括记录和解析对其他模块的依赖性。modules替换旧的基于GOPATH的方法来指定在给定构建中使用哪些源文件。

注意版本：go modules 是 golang 1.11 新加的特性，需要>=1.11版本才可

```
go env -w GO111MODULE=on
go env -w GOPROXY=https://goproxy.cn,direct
```
9、关于GO111MODULE
GO111MODULE 有三个值：off, on和auto（默认值）。

GO111MODULE=off，go命令行将不会支持module功能，寻找依赖包的方式将会沿用旧版本那种通过vendor目录或者GOPATH模式来查找。
GO111MODULE=on，go命令行会使用modules，而一点也不会去GOPATH目录下查找。
GO111MODULE=auto，默认值，go命令行将会根据当前目录来决定是否启用module功能。这种情况下可以分为两种情形：

    a.当前目录在GOPATH/src之外且该目录包含go.mod文件
当前文件在包含go.mod文件的目录下面。
    b.当modules功能启用时，依赖包的存放位置变更为$GOPATH/pkg，允许同一个package多个版本并存，且多个项目可以共享缓存的 module

10、如何使用
```
mkdir goproject
cd goproject
go mod init goproject #此时会发现目录下生成了go.mod
```
执行 go run main.go 运行代码会发现 go mod 会自动查找依赖自动下载


