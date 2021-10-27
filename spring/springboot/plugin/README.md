# 自定义Spring组件



1. 注册启动文件/类注册
2. 注册配置文件提示信息



## 类注册

https://www.cnblogs.com/huanghzm/p/12217630.html

在Spring中，启动类执行包扫描的时候往往值扫描自己模块下的类，如果想要被管理的bean不在Spring容器的包扫描路径下，那么就需要去注册该bean

方法一：

在启动类上使用@Import注解

方法二：

resources/META-INF下的

spring.factories文件

接口=实现类,实现类,实现类

一般使用自定义的bean需要使用org.springframework.boot.autoconfigure.EnableAutoConfiguration做为键才能加入队列中，不需要@Service之类的注解

```properties
# PropertySource Loaders
org.springframework.boot.env.PropertySourceLoader=\
org.springframework.boot.env.PropertiesPropertySourceLoader,\
org.springframework.boot.env.YamlPropertySourceLoader

# Run Listeners
org.springframework.boot.SpringApplicationRunListener=\
org.springframework.boot.context.event.EventPublishingRunListener

# Error Reporters
org.springframework.boot.SpringBootExceptionReporter=\
org.springframework.boot.diagnostics.FailureAnalyzers

# Application Context Initializers
org.springframework.context.ApplicationContextInitializer=\
org.springframework.boot.context.ConfigurationWarningsApplicationContextInitializer,\
org.springframework.boot.context.ContextIdApplicationContextInitializer,\
org.springframework.boot.context.config.DelegatingApplicationContextInitializer,\
org.springframework.boot.rsocket.context.RSocketPortInfoApplicationContextInitializer,\
org.springframework.boot.web.context.ServerPortInfoApplicationContextInitializer

# Application Listeners
org.springframework.context.ApplicationListener=\
org.springframework.boot.ClearCachesApplicationListener,\
org.springframework.boot.builder.ParentContextCloserApplicationListener,\
org.springframework.boot.cloud.CloudFoundryVcapEnvironmentPostProcessor,\
org.springframework.boot.context.FileEncodingApplicationListener,\
org.springframework.boot.context.config.AnsiOutputApplicationListener,\
org.springframework.boot.context.config.ConfigFileApplicationListener,\
org.springframework.boot.context.config.DelegatingApplicationListener,\
org.springframework.boot.context.logging.ClasspathLoggingApplicationListener,\
org.springframework.boot.context.logging.LoggingApplicationListener,\
org.springframework.boot.liquibase.LiquibaseServiceLocatorApplicationListener

# Environment Post Processors
org.springframework.boot.env.EnvironmentPostProcessor=\
org.springframework.boot.cloud.CloudFoundryVcapEnvironmentPostProcessor,\
org.springframework.boot.env.SpringApplicationJsonEnvironmentPostProcessor,\
org.springframework.boot.env.SystemEnvironmentPropertySourceEnvironmentPostProcessor,\
org.springframework.boot.reactor.DebugAgentEnvironmentPostProcessor

# Failure Analyzers
org.springframework.boot.diagnostics.FailureAnalyzer=\
org.springframework.boot.context.properties.NotConstructorBoundInjectionFailureAnalyzer,\
org.springframework.boot.diagnostics.analyzer.BeanCurrentlyInCreationFailureAnalyzer,\
org.springframework.boot.diagnostics.analyzer.BeanDefinitionOverrideFailureAnalyzer,\
org.springframework.boot.diagnostics.analyzer.BeanNotOfRequiredTypeFailureAnalyzer,\
org.springframework.boot.diagnostics.analyzer.BindFailureAnalyzer,\
org.springframework.boot.diagnostics.analyzer.BindValidationFailureAnalyzer,\
org.springframework.boot.diagnostics.analyzer.UnboundConfigurationPropertyFailureAnalyzer,\
org.springframework.boot.diagnostics.analyzer.ConnectorStartFailureAnalyzer,\
org.springframework.boot.diagnostics.analyzer.NoSuchMethodFailureAnalyzer,\
org.springframework.boot.diagnostics.analyzer.NoUniqueBeanDefinitionFailureAnalyzer,\
org.springframework.boot.diagnostics.analyzer.PortInUseFailureAnalyzer,\
org.springframework.boot.diagnostics.analyzer.ValidationExceptionFailureAnalyzer,\
org.springframework.boot.diagnostics.analyzer.InvalidConfigurationPropertyNameFailureAnalyzer,\
org.springframework.boot.diagnostics.analyzer.InvalidConfigurationPropertyValueFailureAnalyzer

# FailureAnalysisReporters
org.springframework.boot.diagnostics.FailureAnalysisReporter=\
org.springframework.boot.diagnostics.LoggingFailureAnalysisReporter

```



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

resources/META-INF下的

additional-spring-configuration-metadata.json文件

或spring-configuration-metadata.json文件

```java
//最简配置
{
"properties": [
	{
        //自定义配置名，必须
	  "name": "spring.xlo.qc.log-level",
        //配置信息的数据类型，可省略
	  "type": "java.lang.String",
        //描述信息，可省略
	  "description": "包名，如果存在多个包，以,分割",
        //需要注入的类中，可省略
	  "sourceType": "xlo.qc.builder.QcConfig",
        //说明默认值的字段，没有任何实际作用
	  "defaultValue": "info"
	}
      // hints可省略，用于定义默认值
  ]
}
```



```json
{
  // group 完全不知道有啥用的属性，可以直接忽略，
  "group": [
	{
	  "name": "spring.xlo.qc",
	  "type": "xlo.qc.builder.QcConfig",
	  "sourceType": "xlo.qc.builder.QcConfig"
	}
  ], "properties": [
	{
        //自定义配置名，必须
	  "name": "spring.xlo.qc.log-level",
        //配置信息的数据类型，可省略
	  "type": "java.lang.String",
        //描述信息，可省略
	  "description": "包名，如果存在多个包，以,分割",
        //需要注入的类中，可省略
	  "sourceType": "xlo.qc.builder.QcConfig",
        //说明默认值的字段，没有任何实际作用
	  "defaultValue": "info"
	}
      // hints可省略，用于定义默认值
  ], "hints": [
    {
      // 定义log-level可选的值
	  "name": "spring.xlo.qc.log-level",
	  "values": [
		{
		  "value": "info",
          "description": "说明"
		},
		{
		  "value": "warning"
		},
		{
		  "value": "error"
		}
	  ]
    }
  ]
}
```

貌似idea可以直接自动生成该文件：https://www.cnblogs.com/yangtianle/p/9065365.html

https://sg.jianshu.io/p/fa396f022e86

```java
// 使用@value("${}")注解，则必须在yml中配置该属性，但是有些可以使用默认值不必配置，因此可以使用一下方法来注入
// https://www.cnblogs.com/jimoer/p/11374229.html
@Data
@Configuration
// ignoreInvalidFields = true,当用户输入错误的值时，继续执行
@ConfigurationProperties(prefix = "spring.xlo.qc", ignoreInvalidFields = true)
public class QcConfig {

    // 支持logLevel支持的属性：
    // logLevel
    // loglevel
    // log_level
    // log-level
    // LOG_LEVEL
	private String logLevel = "info";

}

```



