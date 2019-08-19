---
title: "svn常用命令"
date: 2016-03-25
tags: [svn]
---
checkout:

	svn --username yourusername --password yourpassword co svn_path local_path 

diff:

	svn diff -r 3 text.c

list:

	svn list -v http://svn.test.com/svn
	
cat:

	svn cat -r 4 test.c 
	
svn add file|dir -- 添加文件或整个目录

svn commit  -- 提交本地修改代码

svn status    -- 查看本地修改代码情况：修改的或本地独有的文件详细信息

svn merge   -- 合并svn和本地代码

svn revert   -- 撤销本地修改代码

svn resolve -- 合并冲突代码

svn help [command] -- 查看svn帮助，或特定命令帮助
------
参考资料：

[http://www.iteye.com/problems/58166](http://www.iteye.com/problems/58166)

[http://www.cnblogs.com/zhenjing/archive/2012/12/22/svn_usage.html](http://www.cnblogs.com/zhenjing/archive/2012/12/22/svn_usage.html)

