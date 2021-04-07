# 自定义Spring组件



1. 注册启动文件/类注册
2. 注册配置文件提示信息



## 类注册

https://www.cnblogs.com/huanghzm/p/12217630.html

在Spring中，启动类执行包扫描的时候往往值扫描自己模块下的类，如果想要被管理的bean不在Spring容器的包扫描路径下，那么就需要去注册该bean



## 注册配置文件提示信息

https://docs.spring.io/spring-boot/docs/2.3.9.RELEASE/reference/html/appendix-configuration-metadata.html#configuration-metadata-annotation-processor

导入依赖

```xml
<!--		用于配置springboot配置文件 -->
		<dependency>
			<groupId>org.springframework.boot</groupId>
			<artifactId>spring-boot-configuration-processor</artifactId>
			<optional>true</optional>
		</dependency>
```

如果在导入上述依赖同时导入了spring-boot-maven-plugin，需要按照如下配置修改

```xml
<project>
    <build>
        <plugins>
            <plugin>
                <groupId>org.springframework.boot</groupId>
                <artifactId>spring-boot-maven-plugin</artifactId>
                <configuration>
                    <excludes>
                        <exclude>
                            <groupId>org.springframework.boot</groupId>
                            <artifactId>spring-boot-configuration-processor</artifactId>
                        </exclude>
                    </excludes>
                </configuration>
            </plugin>
        </plugins>
    </build>
</project>
```

additional-spring-configuration-metadata.json文件

```json
{
  "properties": [
	{
        //自定义配置名，必须
	  "name": "spring.qc.locations",
        //配置信息的数据类型，可省略
	  "type": "java.lang.String",
        //描述信息，可省略
	  "description": "包名，如果存在多个包，以,分割",
        //需要注入的类中，可省略
	  "sourceType": "xlo.qc.init.QcApplication"
	}
  ]
}
```

