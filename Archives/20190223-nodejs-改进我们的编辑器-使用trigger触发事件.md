---
title: "改进我们的编辑器-使用trigger触发事件"
date: 2015-05-31
tags: [markdown编辑器,jquery,trigger]
---
昨天我终于把我梦寐以求的编辑器写出来了，真是很简约（lou）啊。

![演示](https://img.geyuxu.com/2015-05-31-001.png)
<!--more-->
首先我们将package.json中的 toolbar设置为true，这样方便我们调试程序。

这个Choose File按钮实在太丑了，大大的“No file chosen”提示着一些不知所谓的东西，我们先把它换掉。 
 
![演示](https://img.geyuxu.com/2015-05-31-002.png)

在fileDialog后面添加一行`<a id="save">保存</a>`，然后添加如下脚本

	var $fileDialog = $('#fileDialog');
	$fileDialog.hide();
	
	$('#save').click(function(){
		$fileDialog.trigger('click');
	});

首先我们用jquery将fileDialog隐藏掉，然后响应“保存”的点击事件，trigger是jquery用于主动触发事件的函数，`$fileDialog.trigger('click');`相当于点击了Choose File按钮。

![演示](https://img.geyuxu.com/2015-05-31-003.png)