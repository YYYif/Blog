<h2>先了解一下什么是fastCGI</h2>

	Php是一门后端脚本语言，与go语言不同，其自身不提供web功能，要实现web应用，需要借助web服务器。由此引出cgi的概念

**先看看什么是cgi（Common Gateway Interface）**

    早期的web服务器只处理html等静态文件，但像php等动态语言出现后，webserver处理不了了，就交给php解析器处理。但php解释器如何与web服务器通信呢？Cgi协议的出现，就是为了解决不同语言解释器（如php,python）与web服务器的通信。
    简单的说，cgi是用来和web服务器打交道的，web服务器收到用户请求，就会把请求提交给cgi程序，cgi程序（php-fpm,hhvm）根据请求提交的参数作相应处理（解析php），然后将输出返回给web服务器，再返回给客户端。

**cgi的改进->fastcgi:**

	cgi有个弊端，每次web请求，都会重新fork一个cgi进程，结束再kill掉，资源消耗大，不适合高并发。

	于是fastcig就应运而生了。它事先早已启动好，一直运行等待web请求过来，再交给php解析，并将结果返回web服务器，继续等待下一个请求。



### 什么是HHVM、php-fpm

php-fpm和hhvm都是fastCGI协议的一种实现

Php-fpm:

  php-Fastcgi Process Manage，是php的fastCGI实现，并提供了进程管理的功能。进程包含 master 进程和 worker 进程两种进程。master 进程只有一个，负责监听端口，接收来自 Web Server 的请求，而 worker 进程则一般有多个(具体数量根据实际需要配置)，每个进程内部都嵌入了一个 PHP 解释器，是 PHP 代码真正执行的地方。

HHVM:

  是Facebook开源的PHP执行引擎，可支持cli，fastcgi server（相当于php-fpm），http server(相当于nginx+phpfpm)三种运行方式。它将php转换为c++,再编译为二进制文件运行，所以性能更好。

二者的区别：

  相比php5.2，hhvm性能更高，更省cpu，据说能省40%-60%

  但php7出来后，据说可以替代hhvm



### 介绍一下HHVM

**一、 简介**

- HHVM (HipHop Virtual Machine) 是 Facebook 开源的 PHP 执行引擎;
- HHVM 采用一种JIT（just-in-time）的编译机制实现了高性能，同时又保持对 PHP 语法的充分支持;
- 可支持cli, fastcgi server（相当于phpfpm)，http server（相当于nginx+phpfpm）三种运行方式;

二、

1.优势: 

- 高性能：从目前已经上线的应用看，与php5.2相比，普遍节省cpu `40%` ~ `60%`。

- 易用的扩展API：HHVM扩展编写非常方便，同一个扩展，平均代码量不到zend扩展的一半。

- 单进程架构：更方便运维，方便共享全局数据结构（如配置、字典甚至网络连接）

  **注: HHVM生成和执行PHP的在中间字节码，执行时通过JIT(Just In Time即时编译，软件优化技术，指在运行时才会去编译字节码为机器码)转换为机器码执行。JIT将大量重复执行的字节码在运行时编译为机器码，达到提高执行效率的目的。通常触发JIT的条件是代码或函数被多次重复调用。**
![image.png](https://upload-images.jianshu.io/upload_images/15108341-5fed77aaa0d372ab.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

- 为什么会有优势呢:
  zend vm 是解释型虚拟机，hhvm是二进制翻译型虚拟机，那么为什么zend 比hhvm慢呢？ 
  其实编译过程到生成中间码的阶段2种引擎(zend vm、hhvm)性能差距并不大，大家都是一样的词法、语法解析和优化生成为中间码，差距是在执行引擎的过程中也就是runtime中：
  zend 执行引擎中opcode被cache opcode 后，就不需要重新编译了，然后每次都去调用c的接口，然后c接口再翻译为机器码执行；
  hhvm执行引擎hhbc(bytecode)生成后也不需要重新翻译了，如果非JIT模式其实和zend+cache模式原理是一样的，通过opcode出栈调用c接口，然后再将C翻译为机器码执行；而JIT模式首次需要将opcode翻译为机器码并且将其cache住，之后每次则执行cache中的机器码而不去执行C层的代码，少了一层翻译过程，直接执行二进制代码了，这样效率就上去了。

- 

![image.png](https://upload-images.jianshu.io/upload_images/15108341-5bfbe588d7d7bc8d.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


未完结.....持续进行
