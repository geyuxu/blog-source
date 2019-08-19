---
title: "使用hexo"
date: 2015-05-18
tags: [hexo] 
---



准备工作
-------------------------------------------------------------------------
安装nodejs


安装hexo
-------------------------------------------------------------------------

1.安装hexo-cli
	
	npm install hexo-cli -g

2.初始化blog文件夹
	
	hexo init blog

如果当前文件夹是空文件夹，也可以省略“blog”。

![演示](https://img.geyuxu.com/2015-5-19-003.png)
<!--more-->
3.用npm安装依赖包
	
	cd blog
	npm install
![演示](https://img.geyuxu.com/2015-5-19-004.png)

4.启动hexo服务器，预览效果
	
	hexo server
![演示](https://img.geyuxu.com/2015-5-19-005.png)

用浏览器打开[http://localhost:4000](http://localhost:4000)

![演示](https://img.geyuxu.com/2015-5-19-006.png)

恭喜你，拥有了自己的网站:)


用hexo写一篇日志
-------------------------------------------------------------------------

输入命令在source\_posts目录下新建一个markdown文件

	hexo new 服务端工程师入门与进阶Java版
![演示](https://img.geyuxu.com/2015-5-19-007.png)

用markdown编辑器打开文件，并编辑。
![演示](https://img.geyuxu.com/2015-5-19-008.png)

输入

	hexo g
	hexo s

在网页中查看效果

![演示](https://img.geyuxu.com/2015-5-19-009.png)


--------------------------------------------------------------------------
>[nodejs官网](https://nodejs.org/)
>
>[hexo官网](http://hexo.io/)
>
>[yilia主题](https://github.com/litten/hexo-theme-yilia)
>
>[《服务端工程师入门与进阶Java版》原文](http://www.kuqin.com/shuoit/20150420/345784.html)
>
>[文中使用的markdown编辑器](http://markdownpad.com/)

