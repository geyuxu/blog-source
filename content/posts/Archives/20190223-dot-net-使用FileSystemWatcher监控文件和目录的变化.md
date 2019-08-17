---
title: "使用FileSystemWatcher监控文件和目录的变化"
date: 2015-05-22 00:29:09
tags: [.Net,FileSystemWatcher]
---
##常用属性：

Path: 要监控的路径

IncludeSubdirectories: 是否监控子目录

Filter: 筛选器，如
	
	“*.jpg | *.doc”

NotifyFilter: 设置哪些变动会触发事件

EnableRaisingEvents: 设值是否开始监控

##常用事件：

Changed ： 更改文件和目录时

Created ： 创建文件和目录时

Deleted ： 删除文件或目录时
 
Renamed ： 重命名文件或目录时
<!--more-->
	using System;
	using System.IO;
	
	namespace ConsoleApplication1
	{
	    class Program
	    {
	        static void Main(string[] args)
	        {
	            FileSystemWatcher fsw = new FileSystemWatcher();

	            fsw.Path = "C:\\test";

	            fsw.IncludeSubdirectories = true;

	            fsw.Filter = "*";

	            fsw.NotifyFilter = NotifyFilters.FileName | NotifyFilters.DirectoryName | NotifyFilters.Size;

	            fsw.Created += new FileSystemEventHandler(fileSystemWatcher_EventHandle);

	            fsw.Deleted += new FileSystemEventHandler(fileSystemWatcher_EventHandle);
	
	            fsw.Changed += new FileSystemEventHandler(fileSystemWatcher_EventHandle);
	
	            fsw.Renamed += new RenamedEventHandler(fileSystemWatcher_Renamed);
	
	            fsw.EnableRaisingEvents = true;
	
	            Console.ReadKey();
	
	        }
	
	        private static void fileSystemWatcher_Renamed(object sender, RenamedEventArgs e)
	        {
	            showLog(e);
	        }
	
	        private static void fileSystemWatcher_EventHandle(object sender, FileSystemEventArgs e)
	        {
	            showLog(e);
	        }
	
	        private static void showLog(FileSystemEventArgs e)
	        { 
	            Console.WriteLine(e.Name + " " + e.ChangeType + " " + e.FullPath); 
	        }
	    }
	}
![演示](https://img.geyuxu.com/2015-05-22-001.png)

------------------------------------------------

参考：

>[FileSystemWatcher用法详解](http://blog.csdn.net/hwt0101/article/details/8469285)

>[FileSystemWatcher 类（msdn）](https://msdn.microsoft.com/zh-cn/library/system.io.filesystemwatcher.aspx)

>[Linux和Mac OS X 中监视文件变化](http://blog.163.com/vic_kk/blog/static/49470524201041301257208/)