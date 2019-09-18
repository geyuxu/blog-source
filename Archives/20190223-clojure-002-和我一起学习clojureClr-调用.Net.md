---
title: "和我一起学习clojureClr-调用.Net"
date: 2015-05-21
tags: [clojure,clojure-clr]
---
打开REPL，输入：

	(System.Console/WriteLine "********")
![演示](https://img.geyuxu.com/2015-5-20-011.png)

显然，可以通过这种形式调用.Net方法。但不够美观，我们将代码做一个封装。
	
	(defn writeline [message]
		(System.Console/WriteLine message))
![演示](https://img.geyuxu.com/2015-5-20-012.png)
<!--more-->
既然可以用Console，能不能使用MessageBox呢？当然可以，以下代码加载MessageBox。

	(System.Reflection.Assembly/Load "System.Windows.Forms, Version=2.0.0.0, 
	 Culture=neutral, PublicKeyToken=b77a5c561934e089")

	(import (System.Windows.Forms MessageBox))
![演示](https://img.geyuxu.com/2015-5-20-013.png)
	
	(MessageBox/Show "内容" "标题")
![演示](https://img.geyuxu.com/2015-5-20-014.png)

封装MessageBox

	(defn show-message 
		([content] (show-message content "Message")) 
		([content title] (MessageBox/Show content title)))
![演示](https://img.geyuxu.com/2015-5-20-015.png)

显示消息框是一件很无聊的事情，WinForms 怎么能没有form，说干就干。

	(import (System.Windows.Forms Form))
	(.Show (Form.))
![演示](https://img.geyuxu.com/2015-5-20-016.png)

form是出来了，但是form上面什么都没有，并且程序死掉了。先这样吧，睡觉了。

参考：

>[clojure 新手指南（10）:与java交互](http://my.oschina.net/clopopo/blog/143000)

>[Clojure CLR 入门](http://www.cnblogs.com/me-sa/archive/2013/03/25/2981192.html)

>[Clojure - Functional Programming for the JVM](http://java.ociweb.com/mark/clojure/article.html)


