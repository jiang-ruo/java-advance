# Spring Boot

1. [快速搭建Spring Boot项目](#qs)
2. [Spring Boot配置文件](#cf) 
3. [个人配置](#ll)
4. [springboot启动机制][boot]
5. [自定义spring组件][plugin]



[boot]: boot
[plugin]: plugin



## <span id="qs">快速搭建Spring Boot项目</span>

​	idea从Spring Initializr建立Spring Boot项目的教程网上有很多，因此不多做赘述，这里从零开始建立Spring Boot项目。

​	\> 新建一个Maven项目

​	\>  导入pom依赖

```xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0"
		 xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
		 xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
	<modelVersion>4.0.0</modelVersion>

	<groupId>xxx.xxx</groupId>
	<artifactId>xxx</artifactId>
	<version>1.0.0</version>

	<parent>
		<groupId>org.springframework.boot</groupId>
		<artifactId>spring-boot-starter-parent</artifactId>
		<version>2.4.1</version>
	</parent>

	<build>
		<plugins>
			<plugin>
				<groupId>org.springframework.boot</groupId>
				<artifactId>spring-boot-maven-plugin</artifactId>
				<version>2.4.1</version>
			</plugin>
		</plugins>
	</build>

	<dependencies>
<!--		导入需要的spring-boot-starter-* 依赖-->
<!--		这里导入的是spring-web -->
		<dependency>
			<groupId>org.springframework.boot</groupId>
			<artifactId>spring-boot-starter-web</artifactId>
		</dependency>

	</dependencies>
</project>
```

\> 新建一个启动类，此处以Application命名

```java
@SpringBootApplication
public class Application {

	public static void main(String[] args) {
		SpringApplication.run(Application.class, args);
	}

}
```

\> 一个最简单的Spring Boot项目就建好了



## <span id="cf">Spring Boot配置文件</span>



## <span id="ll">个人配置</span>

```xml
pom.xml

<?xml version="1.0" encoding="UTF-8"?>

<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
	<modelVersion>4.0.0</modelVersion>

	<groupId>xxx.xxx</groupId>
	<artifactId>xxx</artifactId>
	<version>1.0.0</version>

	<name>xxx</name>

	<parent>
		<groupId>org.springframework.boot</groupId>
		<artifactId>spring-boot-starter-parent</artifactId>
		<version>2.4.1</version>
	</parent>

	<build>
		<finalName>xxx</finalName>
		<plugins>
			<plugin>
				<groupId>org.springframework.boot</groupId>
				<artifactId>spring-boot-maven-plugin</artifactId>
				<version>2.4.1</version>
			</plugin>
		</plugins>
	</build>

	<dependencies>
<!--		测试包-->
		<dependency>
			<groupId>junit</groupId>
			<artifactId>junit</artifactId>
			<version>4.11</version>
			<scope>test</scope>
		</dependency>

<!--		spring-boot的测试包-->
		<dependency>
			<groupId>org.springframework.boot</groupId>
			<artifactId>spring-boot-starter-test</artifactId>
			<scope>test</scope>
		</dependency>

<!--		spring-boot包-->
		<dependency>
			<groupId>org.springframework.boot</groupId>
			<artifactId>spring-boot-starter-web</artifactId>
		</dependency>

<!--lombok依赖-->
		<dependency>
			<groupId>org.projectlombok</groupId>
			<artifactId>lombok</artifactId>
		</dependency>
	</dependencies>
</project>

```
／－

　｜－config/　　　　//配置文件目录

　｜－webapp/　　　//静态资源目录

　｜－src/　　　　　//源码目录

```properties
application.yml

server:
  port: 80

spring:
  #  配置静态资源位置
  web:
    resources:
      static-locations: file:webapp/

#mybatis:
#  mapper-locations: file:config/dao/*.xml
#  configuration:
##    驼峰转下划线
#    map-underscore-to-camel-case: true
```

## [springboot启动机制][boot]

## [自定义spring组件][plugin]