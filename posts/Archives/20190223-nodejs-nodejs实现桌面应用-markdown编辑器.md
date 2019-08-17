title: "nodejs实现桌面应用-markdown编辑器"
date: 2015-05-30
tags: [markdown编辑器,nodejs,node-webkit,nw,markdown] 
---
安装node-webkit
======
	npm install -g nw 


什么是node-webkit
===========
node-webkit是使用nodejs和Chromium为核心开发桌面软件的技术，我们可以使用html/javascript/css来开发桌面应用的界面，用nodejs技术来开发桌面应用的逻辑业务。  
<!--more-->
我们先用`npm init`创建一个package.json文件，随便填或者一路回车就可以。
修改package.json文件

	{
	  "name": "test1",
	  "version": "0.0.0",
	  "description": "",
	  "main": "index.html",//这里修改为index.html
	  "scripts": {
	    "test": "echo \"Error: no test specified\" && exit 1"
	  },
	  "author": "",
	  "license": "BSD-2-Clause"
	}

然后创建一个index.html文件，文件内容大概是这样：

	<!DOCTYPE html>
	<html>
	  <head>
	    <title>test1</title>
	  </head>
	  <body>
		<script>document.write('这是我们的第一个nodejs桌面应用');</script>
	  </body>
	</html>

在shell中运行 `nw .`

![运行的图片](https://img.geyuxu.com/2015-05-30-001.png)

由图我们可以得知，这其实就是一个浏览器。我们需要将导航条去掉，在package.json中加入`"window:{"toolbar":false}"`

	{
	  "name": "test1",
	  "version": "0.0.0",
	  "description": "",
	  "main": "index.html",
	  "window":{
		"toolbar": false
	  },
	  "scripts": {
	    "test": "echo \"Error: no test specified\" && exit 1"
	  },
	  "author": "",
	  "license": "BSD-2-Clause"
	}

再次启动`nw .`

![运行的图片](https://img.geyuxu.com/2015-05-30-002.png)

安装markdown-js
================
我一直为写markdown而困扰，好用的markdown编辑器收费高昂，且本人是三平台工作（mac/linux/windows），也没有一个好用的跨平台markdown编辑器。自己写一个编辑器的想法早就有，奈何技术复杂并且工作繁忙。但如果使用node-webkit平台去做，这件事将非常简单。markdown-js是nodejs的markdown语法解析器，它可以将markdown直接翻译成html文本。我们要做的无非是将markdown翻译出的html放到一个标签中。  
安装markdown-js  

	npm install markdown

引入markdown模块，并调用markdown.toHTML()方法。

	<!DOCTYPE html>
	<html>
	  <head>
	    <title>test1</title>
	  </head>
	  <body>
		<script>
			var markdown = require('markdown').markdown;
			
			document.write(markdown.toHTML('hello **geyuxu**'));
		</script>
	  </body>
	</html>

![运行的图片](https://img.geyuxu.com/2015-05-30-003.png)	

写一个左右布局的页面
=================
至此，我们可以在页面中使用markdown模块了。既然如此，为什么不写一个左右布局的页面。左边嵌套一个文本框作为markdown输入框，右边填入markdown-js转换后的html，这样就可以实现市面上常见的markdown所见即所得编辑器了。

修改index.html

	<!DOCTYPE html>
	<html>
	  <head>
	    <title>test1</title>
		<script src="./jquery-1.7.1.js"></script>
		<style>
			*{font-size:18px;margin:0}
			html,body{height:100%}
			.left{width:50%;height:100%;float:left;}
			.right{width:50%;height:100%;float:left;}
			.left textarea{width:98%;height:100%;background:transparent;border-style:none;}
		</style>
	  </head>
	  <body>
		<div class="left"><textarea></textarea></div>
		<div class="right"></div>
		
		<script>
			//引入markdown
			var markdown = require('markdown').markdown;
			
			//相应textarea的keyup事件，当输入字符时刷新右边的效果	
			$('.left textarea').on('keyup',function(){
				var content = $(this).val();
				content = markdown.toHTML(content);
				$('.right').html(content);
			});
	
		</script>
	  </body>
	</html>

![运行的图片](https://img.geyuxu.com/2015-05-30-004.png)	

至此我们的简易markdown编辑器就算完成了，是不是很简单。

用fs模块保存文件
==================
引入fs模块`var fs = require('fs');` fs模块是nodejs核心模块，不需要npm install  
使用`fs.writeFileSync(路径,内容)`就可以保存文件了。

	<!DOCTYPE html>
	<html>
	  <head>
	    <title>test1</title>
		<script src="./jquery-1.7.1.js"></script>
		<style>
			*{font-size:18px;margin:0}
			html,body{height:100%}
			.left{width:50%;height:100%;float:left;}
			.right{width:50%;height:100%;float:left;}
			.left textarea{width:98%;height:100%;background:transparent;border-style:none;}
		</style>
	  </head>
	  <body>
		<div class="left"><textarea></textarea></div>
		<div class="right"></div>
		<input id="fileDialog" type="file" nwsaveas />
		<script>
			var markdown = require('markdown').markdown;
			var fs = require('fs');	
			
			$(function(){
				$('.left textarea').on('keyup',function(){
					var content = $(this).val();
					content = markdown.toHTML(content);
					$('.right').html(content);
				});
				
				//相应file标签的change事件，
				$('#fileDialog').on('change',function(){
					fs.writeFileSync($(this).val(),$('.left textarea').val());
					$(this).val("");
				});
			});
			
	
		</script>
	  </body>
	</html>

`<input id="fileDialog" type="file" nwsaveas />` node0-webkit对html做了扩展，`nsaveas`会弹出一个保存对话框。

将程序打包成exe
=========
将我们程序的所有文件缩成一个zip文件  
![运行的图片](https://img.geyuxu.com/2015-05-30-005.png)	

打开nodejs安装目录，找到nw所在目录，将zip文件拷贝到nwjs下  
![运行的图片](https://img.geyuxu.com/2015-05-30-006.png)

在cmd中用`copy /b`命令将nw.exe和test1.zip合并成一个可执行文件

	>copy /b nw.exe+test1.zip test1.exe
![运行的图片](https://img.geyuxu.com/2015-05-30-007.png)

注意，nw.exe必须在前面。

![运行的图片](https://img.geyuxu.com/2015-05-30-008.png)

双击test1.exe就可以运行了，这时test1.exe还不能单独运行，它需要依赖dll文件。我们可以用[Enigma Virtual Box](http://enigmaprotector.com/assets/files/enigmavb.exe)将所有需要的dll文件打包。
![运行的图片](https://img.geyuxu.com/2015-05-30-009.png)
点击Process之后，生成的test1_boxed.exe，就可以单独拷贝出去运行了。

---------------
参考：
> [node-webkit中文文档](http://www.xuanhun521.com/?search=node)  
> [www.cnblogs.com/xuanhun/p/3653816.html](http://www.cnblogs.com/xuanhun/p/3653816.html)  
> [node-webkit官方网站](https://github.com/nwjs/nw.js)  
> [node-webkit：开发桌面+WEB混合型应用的神器](http://damoqiongqiu.iteye.com/blog/2010720)