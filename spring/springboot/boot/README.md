# Spring Boot启动机制

问题备注：

1、springboot事件机制：GenericApplicationListener

```java
package xlo.qc.init;

import org.springframework.beans.factory.annotation.Value;
import org.springframework.context.ApplicationEvent;
import org.springframework.context.event.GenericApplicationListener;
import org.springframework.core.ResolvableType;

/**
 * @author XiaoLOrange
 * @time 2021.04.04
 * @title
 * 使用在main类上添加@import注解添加该类，
 * 在配置文件中添加spring.qc.locations配置，
 * 值为包名多个包名可以以逗号分割
 */

public class QcApplication implements GenericApplicationListener {

	@Value("${spring.qc.locations}")
	private String locations;

	/**
	 * Determine whether this listener actually supports the given event type.
	 *
	 * @param eventType the event type (never {@code null})
	 * @return true - 执行 {@link #supportsSourceType(Class)}
	 * @return false - 不做任何操作
	 */
	@Override
	public boolean supportsEventType(ResolvableType eventType) {
		System.out.println(eventType);
		return true;
	}

	/**
	 * Determine whether this listener actually supports the given source type.
	 * <p>The default implementation always returns {@code true}.
	 *
	 * @param sourceType the source type, or {@code null} if no source
	 *
	 * @return true - 执行 {@link #onApplicationEvent(ApplicationEvent)}
	 * @return false - 不做任何操作
	 */
	@Override
	public boolean supportsSourceType(Class<?> sourceType) {
		System.out.println(0);
		return false;
	}

	/**
	 * Handle an application event.
	 *
	 * @param event the event to respond to
	 */
	@Override
	public void onApplicationEvent(ApplicationEvent event) {
		System.out.println(2);
	}
}

```

2、springboot @Order注解

ApplicationRunner和CommandLineRunner

```java
package xlo.qc.init;

import org.springframework.beans.factory.annotation.Value;
import org.springframework.boot.CommandLineRunner;
import org.springframework.core.annotation.Order;

/**
 * @author XiaoLOrange
 * @time 2021.04.04
 * @title
 * 使用在main类上添加@import注解添加该类，
 * 在配置文件中添加spring.qc.locations配置，
 * 值为包名多个包名可以以逗号分割
 */

//在启动时执行任务，使用Order注解并实现CommandLineRunner
@Order
public class QcApplication implements CommandLineRunner {

	@Value("${spring.qc.locations}")
	private String locations;

	@Override
	public void run(String... args) throws Exception {
		for (String arg: args){
			System.out.println(arg);
		}
		System.out.println(locations);
	}
}

```



3、@PostConstruct和@PreDestroy

```java
package com.dada.test.config;
 
import org.springframework.stereotype.Component;
 
import javax.annotation.PostConstruct;
import javax.annotation.PreDestroy;
 
/**
 * @Description:
 * @author: ztd
 * @date 2019/4/25 下午7:35
 */
@Component
public class BeanInitDestory {
    @PostConstruct
    public void init() {
        System.out.println("bean初始化之后执行...");
    }
 
    @PreDestroy
    public void destory() {
        System.out.println("bean销毁之后执行...");
    }
}
————————————————
版权声明：本文为CSDN博主「NemoHero」的原创文章，遵循CC 4.0 BY-SA版权协议，转载请附上原文出处链接及本声明。
原文链接：https://blog.csdn.net/zsj777/article/details/103151205
```



4、@PostConstruct和@Order的区别

　　初步了解：

　　@Order在所有bean加载完成后按照order的顺序执行其中的run方法，

　　@PostConstruct在bean构造完成后执行被注解的方法



目标：

1、了解Springboot的启动机制，

2、了解Springboot中各个注解的加载顺序及区别

