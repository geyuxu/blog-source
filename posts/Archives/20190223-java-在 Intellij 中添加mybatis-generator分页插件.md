title: "在 Intellij 中添加mybatis-generator分页插件"
date: 2015-11-02 20:19:39
tags: [java,mybatis,genreator]
---
mybatis-genreator并没有为我们实现分页操作，本文介绍如何编写mybatis-genreator插件实现分页。

前期工作：在 Intellij 中搭建 mybatis-genreator 环境。
<!--more-->
步骤：     
1.添加插件类

	package com.geyuxu.utils;
    
    import org.mybatis.generator.api.CommentGenerator;
    import org.mybatis.generator.api.IntrospectedTable;
    import org.mybatis.generator.api.PluginAdapter;
    import org.mybatis.generator.api.dom.java.*;
    import org.mybatis.generator.api.dom.xml.Attribute;
    import org.mybatis.generator.api.dom.xml.Document;
    import org.mybatis.generator.api.dom.xml.TextElement;
    import org.mybatis.generator.api.dom.xml.XmlElement;
    
    import java.util.List;
    
    public class OraclePaginationPlugin extends PluginAdapter {
        private static String PAGE_URI = "com.geyuxu.utils.Page";
    
        @Override
        public boolean modelExampleClassGenerated(TopLevelClass topLevelClass, IntrospectedTable introspectedTable) {
            addPage(topLevelClass, introspectedTable, "page");
            return super.modelExampleClassGenerated(topLevelClass,
                    introspectedTable);
        }
    
        @Override
        public boolean sqlMapDocumentGenerated(Document document,
                                               IntrospectedTable introspectedTable) {
            XmlElement parentElement = document.getRootElement();
    
            // 产生分页语句前半部分
            XmlElement paginationPrefixElement = new XmlElement("sql");
            paginationPrefixElement.addAttribute(new Attribute("id",
                    "OracleDialectPrefix"));
            XmlElement pageStart = new XmlElement("if");
            pageStart.addAttribute(new Attribute("test", "page != null"));
            pageStart.addElement(new TextElement(
                    "select * from ( select row_.*, rownum rownum_ from ( "));
            paginationPrefixElement.addElement(pageStart);
            parentElement.addElement(paginationPrefixElement);
    
            // 产生分页语句后半部分
            XmlElement paginationSuffixElement = new XmlElement("sql");
            paginationSuffixElement.addAttribute(new Attribute("id",
                    "OracleDialectSuffix"));
            XmlElement pageEnd = new XmlElement("if");
            pageEnd.addAttribute(new Attribute("test", "page != null"));
            pageEnd.addElement(new TextElement(
                    "<![CDATA[ ) row_ ) where rownum_ > #{page.begin} and rownum_ <= #{page.end} ]]>"));
            paginationSuffixElement.addElement(pageEnd);
            parentElement.addElement(paginationSuffixElement);
    
            return super.sqlMapDocumentGenerated(document, introspectedTable);
        }
    
        @Override
        public boolean sqlMapSelectByExampleWithoutBLOBsElementGenerated(
                XmlElement element, IntrospectedTable introspectedTable) {
    
            XmlElement pageStart = new XmlElement("include"); //$NON-NLS-1$
            pageStart.addAttribute(new Attribute("refid", "OracleDialectPrefix"));
            element.getElements().add(0, pageStart);
    
            XmlElement isNotNullElement = new XmlElement("include"); //$NON-NLS-1$
            isNotNullElement.addAttribute(new Attribute("refid",
                    "OracleDialectSuffix"));
            element.getElements().add(isNotNullElement);
    
            return super.sqlMapUpdateByExampleWithoutBLOBsElementGenerated(element,
                    introspectedTable);
        }
    
        /**
         * @param topLevelClass
         * @param introspectedTable
         * @param name
         */
        private void addPage(TopLevelClass topLevelClass,
                             IntrospectedTable introspectedTable, String name) {
            topLevelClass.addImportedType(new FullyQualifiedJavaType(
                    PAGE_URI));
            CommentGenerator commentGenerator = context.getCommentGenerator();
            Field field = new Field();
            field.setVisibility(JavaVisibility.PROTECTED);
            field.setType(new FullyQualifiedJavaType(PAGE_URI));
            field.setName(name);
            commentGenerator.addFieldComment(field, introspectedTable);
            topLevelClass.addField(field);
            char c = name.charAt(0);
            String camel = Character.toUpperCase(c) + name.substring(1);
            Method method = new Method();
            method.setVisibility(JavaVisibility.PUBLIC);
            method.setName("set" + camel);
            method.addParameter(new Parameter(new FullyQualifiedJavaType(
                    PAGE_URI), name));
            method.addBodyLine("this." + name + "=" + name + ";");
            commentGenerator.addGeneralMethodComment(method, introspectedTable);
            topLevelClass.addMethod(method);
            method = new Method();
            method.setVisibility(JavaVisibility.PUBLIC);
            method.setReturnType(new FullyQualifiedJavaType(
                    PAGE_URI));
            method.setName("get" + camel);
            method.addBodyLine("return " + name + ";");
            commentGenerator.addGeneralMethodComment(method, introspectedTable);
            topLevelClass.addMethod(method);
        }
    
        @Override
        public boolean validate(List<String> list) {
            return true;
        }
    }
      
2.添加Page类

	package com.geyuxu.utils;
    
    import java.util.List;

    public class Page<T> {
        // 分页查询开始记录位置
        private int begin;
        // 分页查看下结束位置
        private int end;
        // 每页显示记录数
        private int length;
        // 查询结果总记录数
        private int count;
        // 当前页码
        private int current;
        // 总共页数
        private int total;
    
        private List<T> list;

    
        /**
         * 构造函数
         */
    
        public Page(Integer pageNo) {
            this(pageNo,10);
        }
    
        public Page(Integer pageNo,Integer length) {
            this.current = pageNo;
            this.length = length == null?10:length;
            this.begin = (this.current - 1) * this.length ;
            this.end = this.begin + this.length;
        }
    
        /**
         * @return the begin
         */
        public int getBegin() {
            return begin;
        }
    
        /**
         * @return the end
         */
        public int getEnd() {
            return end;
        }
    
        /**
         * @param end the end to set
         */
        public void setEnd(int end) {
            this.end = end;
        }
    
        /**
         * @param begin the begin to set
         */
        public void setBegin(int begin) {
            this.begin = begin;
            if (this.length != 0) {
                this.current = (int) Math.floor((this.begin * 1.0d) / this.length) + 1;
            }
        }
    
        /**
         * @return the length
         */
        public int getLength() {
            return length;
        }
    
        /**
         * @param length the length to set
         */
        public void setLength(int length) {
            this.length = length;
            if (this.begin != 0) {
                this.current = (int) Math.floor((this.begin * 1.0d) / this.length) + 1;
            }
        }
    
        /**
         * @return the count
         */
        public int getCount() {
            return count;
        }
    
        /**
         * @param count the count to set
         */
        public void setCount(int count) {
            this.count = count;
            this.total = (int) Math.floor((this.count * 1.0d) / this.length);
            if (this.count % this.length != 0) {
                this.total++;
            }
        }
    
        /**
         * @return the current
         */
        public int getCurrent() {
            return current;
        }
    
        /**
         * @param current the current to set
         */
        public void setCurrent(int current) {
            this.current = current;
        }
    
        /**
         * @return the total
         */
        public int getTotal() {
            if (total == 0) {
                return 1;
            }
            return total;
        }
    
        /**
         * @param total the total to set
         */
        public void setTotal(int total) {
            this.total = total;
        }
    
        public List<T> getList() {
            return list;
        }
    
        public void setList(List<T> list) {
            this.list = list;
        }
    }

3.将OraclePaginationPlugin打成 jar 包

在菜单『File>Project Structure>Artifacts>+(Add)>Jar>Empty』    
Name 设置为pagination-0.1   
Output Directory 选择一个合适的目录，我这里选的是桌面『/Users/geyuxu/Desktop』 
添加一个目录，Directory Content      
![img](https://img.geyuxu.com/2015-11-02-1.png)    
选择 生成class 文件所在的目录，我这里是 target/classes

4.将打好的 jar 包导入到 maven 本地库中
Edit Configurations    
![img](https://img.geyuxu.com/2015-11-02-2.png)       
maven    
![img](https://img.geyuxu.com/2015-11-02-3.png)       
name 随便填，Working directory 选当前项目的目录  
![img](https://img.geyuxu.com/2015-11-02-4.png)      
Command line:    

	install:install-file -Dfile=/Users/geyuxu/Desktop/pagination-0.1.jar -DgroupId=com.geyuxu.utils -DartifactId=pagination -Dversion=0.1 -Dpackaging=jar
 
5.在[]添加如下选项
![img](https://img.geyuxu.com/2015-11-02-5.png)   

6.修改 pom文件

		</plugins>
			<plugin>
				<groupId>org.mybatis.generator</groupId>
				<artifactId>mybatis-generator-maven-plugin</artifactId>
				<version>1.3.2</version>
				<configuration>
					<verbose>true</verbose>
					<overwrite>true</overwrite>
				</configuration>
				<dependencies>
					<dependency>
						<groupId>com.geyuxu.utils</groupId>
						<artifactId>pagination</artifactId>
						<version>0.1</version>
					</dependency>
				</dependencies>
			</plugin>
		</plugins>

使用方法：

	int pageNum = 1;
	int pageSize = 10;
	StudentExample example = new StudentExample();
	Page<Student> page = new Page<Student>(pageNum,pageSize);
    example.setPage(page);
    page.setCount(studentMapper.countByExample(example));
    List<Student> students = studentMapper.selectByExample(example);
	page.setList(students);
	
-----------
参考资料：    
[Mybatis Generator实现分页功能](http://blog.csdn.net/aking21alinjuju/article/details/9357893)         
[mybatis generator生成带有分页的Mybatis代码](http://blog.csdn.net/xzknet/article/details/44158009)    
[http://bbs.csdn.net/topics/390841395](http://bbs.csdn.net/topics/390841395)    
[http://www.iteye.com/problems/103171](http://www.iteye.com/problems/103171)    
[Intellij IDEA 14中使用MyBatis-generator 自动生成MyBatis代码](http://blog.csdn.net/z69183787/article/details/46560071)
