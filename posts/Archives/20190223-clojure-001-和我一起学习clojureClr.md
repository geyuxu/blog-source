---
title: "和我一起学习clojureClr-Hello World"
date: 2015-05-20 00:44:45
tags: [clojure,clojure-clr]
---
>俗话说的好，java能做的事.net也能做，java不能做的事，.net还是能做。。。
<!--more-->
在官网下载最新版本的clojureClr

[主页](https://github.com/clojure/clojure-clr) 

[下载页面](https://github.com/clojure/clojure-clr/wiki/Getting-binaries)

我是在SourceForge下载 [https://sourceforge.net/projects/clojureclr/files/](https://sourceforge.net/projects/clojureclr/files/)

将压缩包解压到你方便操作的目录
![演示](https://img.geyuxu.com/2015-5-20-001.png)

###使用REPL

运行Clojure.Main.exe，进入REPL界面。
![演示](https://img.geyuxu.com/2015-5-20-002.png)

万年不变的hello world

	(println "hello world")
![演示](https://img.geyuxu.com/2015-5-20-003.png)

###栗子

创建目录结构和文件com\geyuxu\examples\hello.clj

![演示](https://img.geyuxu.com/2015-5-20-004.png)

在hello.clj中输入以下代码

	(defn hello [name]
		(str "hello, " name))

保存hello.clj后，回到REPL中输入

	(use 'com.geyuxu.examples.hello)

	(hello "geyuxu")
![演示](https://img.geyuxu.com/2015-5-20-006.png)

![演示](https://img.geyuxu.com/2015-5-20-007.png)

我们来简单分析一下：

1. use '  类似于java的import xxx或者c#的using xxx，但有所不同，这个以后再说。注意，上引号不可以丢掉。

2. 创建一个函数的方法
		
		(defn 函数名 [参数] (函数体))

3. 调用函数
	
		(函数名 参数)


######参考：

> [Clojure CLR 入门](http://www.cnblogs.com/me-sa/archive/2013/03/25/2981192.html)
 
> [《Clojure程序设计》](http://book.douban.com/subject/21459993/)

> [《Clojure编程》](http://book.douban.com/subject/21661495/)

> [clojure官网](http://clojure.org/)