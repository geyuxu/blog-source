---
title: "fortify 基本命令"
date: 2016-03-17 17:03:00
tags: [fortify]
---

转换单一个文件 myServlet.java 并指定classpath为 lib/j2ee.jar其命令为:

	sourceanalyzer -b MyServlet -cp lib/j2ee.jar MyServlet.java

	
转换src目录下的所有.java文件并指定classpath为 lib目录下的所有.jar文件其命令为:


	sourceanalyzer -b MyProject -cp "lib/*.jar" "src/**/*.java“
<!--more-->
	
转换文件 myCode.java并指定使用javac编译器其命令为:


	sourceanalyzer -b mybuild –c javac -cp libs.jar MyCode.java

J2EE项目转换简便方法

把项目的所有文件和库都放在一个目录下,运行下面的命令:


	sourceanalyzer -Xmx1000m -b pName -encoding "UTF-8" -cp "**/*.jar" .

	
	sourceanalyzer -Xmx1000m -b pName -appserver weblogic -appserver-verion 9 –appserver-home “d:\bea\webloigc\server\lib” -encoding "UTF-8" -cp "**/*.jar" .

	

Java项目扫描实例


1 clean


	sourceanalyzer -b eightball -clean
2 translate


	sourceanalyzer -b eightball -Xmx1250m -jdk 1.5 -debug -cp *.jar .

	
3 show -file


	sourceanalyzer -b eightball -show-files

	
4 scan


	sourceanalyzer -b eightball -Xmx1250m -debug -scan -f
	 eightball.fpr -disable-source-rendering

导出 pdf/rtf/xml


	ReportGenerator -format [pdf/rtf/xml] -f 生成文件路径 -source fpr文件路径 template "指定模板.xml"

	

以下是我写的一个脚本，用于循环扫描所有工程，每个工程的路径放在fortify_scan_dir_list.txt中


	fortify_result_path="/root/fortify"
	svn_list="/root/fortify_scan_dir_list.txt"

	for line in $(<$svn_list)
	do
		targe_name=${line##*/}
		echo  ------ $targe_name 开始

		echo  -clean-
		sourceanalyzer -b $targe_name -clean
		echo  -转换-
		sourceanalyzer -b $targe_name -cp "$line/**/*.jar" -source 1.7 -encoding UTF-8 "$line"
		file_name=$targe_name-`date --date="-24 hour" +%Y%m%d`
		echo  -扫描-
		sourceanalyzer -b $targe_name -format fpr -f $fortify_result_path/$file_name.fpr -scan
		echo  -生成报告-
		ReportGenerator -format rtf -f $fortify_result_path/$file_name.rtf -source $fortify_result_path/$file_name.fpr template "Developer Workbook 2014CN.xml"
		ReportGenerator -format xml -f $fortify_result_path/$file_name.xml -source $fortify_result_path/$file_name.fpr template "Developer Workbook 2014CN.xml"
		ReportGenerator -format pdf -f $fortify_result_path/$file_name.pdf -source $fortify_result_path/$file_name.fpr template "Developer Workbook 2014CN.xml"

		echo ----- $targe_name 结束
	done

----------
   
