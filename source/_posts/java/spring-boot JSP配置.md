title: "spring-boot JSP配置"
date: 2016-04-09 19:53:00
tags: [java,spring boot,jsp]
---

新建一个maven工程，pom.xml 文件如下:

	<?xml version="1.0" encoding="UTF-8"?>
    <project xmlns="http://maven.apache.org/POM/4.0.0"
             xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
             xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
        <modelVersion>4.0.0</modelVersion>

        <groupId>com.geyuxu</groupId>
        <artifactId>jsp-sample</artifactId>
        <version>1.0-SNAPSHOT</version>

        <parent>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-parent</artifactId>
            <version>1.3.3.RELEASE</version>
        </parent>

        <dependencies>
            <!--spring boot-->
            <dependency>
                <groupId>org.springframework.boot</groupId>
                <artifactId>spring-boot-devtools</artifactId>
                <optional>true</optional>
            </dependency>
            <dependency>
                <groupId>org.springframework.boot</groupId>
                <artifactId>spring-boot-starter</artifactId>
            </dependency>
            <dependency>
                <groupId>org.springframework.boot</groupId>
                <artifactId>spring-boot-starter-tomcat</artifactId>
            </dependency>
            <dependency>
                <groupId>org.springframework</groupId>
                <artifactId>spring-webmvc</artifactId>
            </dependency>

            <!--tomcat-->
            <dependency>
                <groupId>org.apache.tomcat.embed</groupId>
                <artifactId>tomcat-embed-jasper</artifactId>
            </dependency>

        </dependencies>

        <build>
            <plugins>
                <plugin>
                    <groupId>org.springframework.boot</groupId>
                    <artifactId>spring-boot-maven-plugin</artifactId>
                </plugin>
            </plugins>
        </build>
    </project>
<!--more-->
maven install 后，创建 Application 类，用于启动工程：

	package com.geyuxu;
    
    import org.springframework.boot.SpringApplication;
    import org.springframework.boot.autoconfigure.SpringBootApplication;
    
    /**
     * @author 葛于旭
     * @version V3.0
     * @ClassName: Application
     * @date 2016年04月09日 20:03:03
     */
    @SpringBootApplication
    public class Application {
        public static void main(String[] args) {
            SpringApplication.run(Application.class, args);
        }
    }
   
   
创建 WebConfig 类，此类用于配置 web 工程 ，替代了原先的配置文件：

	package com.geyuxu.config;
    
    import org.springframework.context.annotation.Bean;
    import org.springframework.context.annotation.ComponentScan;
    import org.springframework.context.annotation.Configuration;
    import org.springframework.web.servlet.DispatcherServlet;
    import org.springframework.web.servlet.config.annotation.DefaultServletHandlerConfigurer;
    import org.springframework.web.servlet.config.annotation.EnableWebMvc;
    import org.springframework.web.servlet.config.annotation.WebMvcConfigurerAdapter;
    import org.springframework.web.servlet.view.InternalResourceViewResolver;
    
    /**
     * @author 葛于旭
     * @version V3.0
     * @ClassName: WebConfig
     * @date 2016年04月09日 15:20:03
     */
    @EnableWebMvc
    @ComponentScan
    @Configuration
    public class WebConfig extends WebMvcConfigurerAdapter {
    
        @Bean
        public InternalResourceViewResolver defaultViewResolver() {
            InternalResourceViewResolver viewResolver = new InternalResourceViewResolver();
            viewResolver.setPrefix("/pages/");
            viewResolver.setSuffix(".jsp");
            return viewResolver;
        }
    
        @Bean
        public DispatcherServlet dispatcherServlet() {
            return new DispatcherServlet();
        }
    
        @Override
        public void configureDefaultServletHandling(
                DefaultServletHandlerConfigurer configurer) {
            configurer.enable();
        }
    }
    
在 src/main/webapp/WEB-INF 目录下创建 web.xml 文件：

	<?xml version="1.0" encoding="UTF-8"?>
    <web-app version="2.5" xmlns="http://java.sun.com/xml/ns/javaee"
             xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
             xsi:schemaLocation="http://java.sun.com/xml/ns/javaee http://java.sun.com/xml/ns/javaee/web-app_2_5.xsd">
    
        <servlet>
            <servlet-name>appServlet</servlet-name>
            <servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>
            <init-param>
                <param-name>contextClass</param-name>
                <param-value>org.springframework.web.context.support.AnnotationConfigWebApplicationContext</param-value>
            </init-param>
            <init-param>
                <param-name>contextConfigLocation</param-name>
                <param-value>com.bailiangroup.blcp.message.config</param-value>
            </init-param>
            <load-on-startup>1</load-on-startup>
        </servlet>
    
        <servlet-mapping>
            <servlet-name>appServlet</servlet-name>
            <url-pattern>/</url-pattern>
        </servlet-mapping>
    
        <!-- Disables Servlet Container welcome file handling. Needed for compatibility
            with Servlet 3.0 and Tomcat 7.0 -->
        <welcome-file-list>
            <welcome-file></welcome-file>
        </welcome-file-list>
    
    </web-app>
    
下面写一个 Controller 验证是否成功

	@Controller
	public class MyController {

		@RequestMapping(value="/test")
		public String test(){
			return "a";
		}
		
	}
    
在 webapp/pages 目录下创建 a.jsp

	<%@ page contentType="text/html;charset=UTF-8" language="java" %>
	<html>
		<head>
 	  		<title></title>
		</head>
		<body>
 	 		<h1>hi~</h1>
		</body>
	</html>

运行 Application.main() 方法，在浏览器大开默认地址 http://localhost:8080/test 。如果以上步骤正确，就能看到结果了。

------
[]()

