---
title: "npm使用国内镜像"
date: 2015-05-28
tags: [nodejs,npm,registry] 
---
切换方法
-------
方法一：在命令上指定

	npm --registry http://registry.npm.taobao.org

方法二：通过config命令

	npm config set registry http://registry.npm.taobao.org

方法三：编辑 [User Home]/.npmrc 加入下面内容，此方法同方法二等效

	registry = http://registry.npm.taobao.org
<!--more-->
国内镜像
-------
淘宝镜像：[http://registry.npm.taobao.org](http://registry.npm.taobao.org)

cnpmjs：[http://registry.cnpmjs.org](http://registry.cnpmjs.org)


npm publish
-----------
npm publish命令会发布你自己的模块到npm社区，所以在使用该命令前要切换回原先的registry：https://registry.npmjs.org

参考：

[http://cnpmjs.org/](http://cnpmjs.org/)

[http://npm.taobao.org/](http://npm.taobao.org/)