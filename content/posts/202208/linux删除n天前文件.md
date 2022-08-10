---
title: "linux删除n天前文件"
date: 2022-08-10
tags: [linux]
---

通过find命令可以查找到指定日期的文件，再通过-exec参数可以执行删除操作。

命令: `find 目录 -mtime +日期 -name "文件名" -exec rm -rf {} \;`

例：清除1天前的日志    
`find . -mtime +1 -name "*2022*" -exec rm -rf {} \;`

find命令说明：    
-mtime +1 ：查找1天前的文件.   
-name "\*2022\*" ： 查找只包含\*2022\*的文件.   
-exec rm -rf {} \; ： 对查到到的文件进行删除操作.   


