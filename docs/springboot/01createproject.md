# 搭建Spring Boot项目相关的配置

官方文档：https://spring.io/projects/spring-boot

创建Spring Boot脚手架地址：https://start.spring.io

------

## 第一章 添加依赖相关

### 1.基本依赖及编码设置

```java
	<properties>
		<project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
		<project.reporting.outputEncoding>UTF-8</project.reporting.outputEncoding>
		<java.version>1.8</java.version>
	</properties>

	<parent>
		<groupId>org.springframework.boot</groupId>
		<artifactId>spring-boot-starter-parent</artifactId>
		<version>2.1.6.RELEASE</version>
		<relativePath/>
	</parent>
	<dependencies>
		<dependency>
			<groupId>org.springframework.boot</groupId>
			<artifactId>spring-boot-starter</artifactId>
		</dependency>
		<dependency>
			<groupId>org.springframework.boot</groupId>
			<artifactId>spring-boot-starter-test</artifactId>
			<scope>test</scope>
		</dependency>
		<!--spring boot web starter-->
		<dependency>
			<groupId>org.springframework.boot</groupId>
			<artifactId>spring-boot-starter-web</artifactId>
			<version>RELEASE</version>
			<scope>compile</scope>
		</dependency>
	</dependencies>
```

### 2.几个maven插件

#### 1.spring-boot-maven-plugin

spring-boot-maven-plugin插件主要有以下几个功能：

1. 可以通过`mvn package`打包时，直接打成一个可以直接运行jar文件
2. 优化打包时依赖，打包时会把所有的依赖打进去
3. 你还可以指定要执行的类，如果不指定的话，Spring会找有这个【`public static void main(String[] args)`】方法的类，当做可执行的类。

不加这个插件，可能导致无法独立运行。

配置方式：

```
	<build>
		<plugins>
			<plugin>
				<groupId>org.springframework.boot</groupId>
				<artifactId>spring-boot-maven-plugin</artifactId>
			</plugin>
		</plugins>
	</build>
```

#### 2.spring-boot-devtools

spring-boot-devtools是Spring Boot的热部署插件

```
     <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-devtools</artifactId>
            <optional>true</optional>
     </dependency>
```

##### **集成注意事项**

​	1.如果发现没有热部署效果，则需要检查IDE配置中有没有打开自动编译。

​	2.如果使用Thymeleaf模板引擎，需要把模板默认缓存设置为false

```
`#禁止thymeleaf缓存（建议：开发环境设置为``false``，生成环境设置为``true``）``spring.thymeleaf.cache=``false`
```

​	3.针对devtools的可以指定目录或者排除目录来进行热部署

```
`#添加那个目录的文件需要restart``spring.devtools.restart.additional-paths=src/main/java``#排除那个目录的文件不需要restart``spring.devtools.restart.exclude=``static``/**,``public``/**`
```

​	4.设置idea让他实现文件修改自动重启项目

​		1.找到idea的Preferences -> Build, Execution, Deployment -> Compiler，勾选Build project automatically

​		2.help->Find Action,搜索Registry，勾选compiler.automake.allow.when.app.running

​		通过以上的设置就可以在不重启服务的情况下加载html，但如果修改java文件，服务在几秒后会自动重启，如果不希望服务重启需要在application.properties或application.yml中添加spring.devtools.reatart.enable=false

### 第二章 配置文件相关

1.修改配置文件为yml方式：

​	修改application.properties为application.yml

