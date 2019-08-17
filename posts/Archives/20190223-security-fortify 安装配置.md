---
title: "fortify 安装配置"
date: 2016-03-17 15:03:00
tags: [fortify]
---
![HP Fortify SCA](https://img.geyuxu.com/2016-03-17-001.png)

<!--more-->

1.安装文件

	Windows
	
	Linux
	
	Mac OS X

一路 next...

*以 linux 为例默认安装目录为

	/opt/HP_Fortify/HP_Fortify_SCA_and_Apps_4.30
2.将 license 拷贝到默认安装目录下

3.复制规则库

	*离线规则库在 http://support.fortify.com 中下载
	安装目录/Core/config/rules/
	
4.修改配置文件 Core/config/fortify.properties
	
	com.fortify.locale=zh_CN


----------
   
