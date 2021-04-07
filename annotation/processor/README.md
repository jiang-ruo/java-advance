# 注解解析

* 注解解析
  1. [源码注解](#p_source)
  2. [编译时注解](#p_class) ：**注解解析器**
     1. [注册解析器](#p_c_regist)
     2. [虚处理器](#p_c_ap)
     3. [处理器](#p_c_process)
     4. [代码生成](#p_c_code)
  3. [运行时注解](#p_runtime)

　　大家都知道注解的运行机制有3种：源码注解、编译期注解、运行时注解。网上有很多关于运行时注解的教程，但是却很少有关于了解源码注解和编译期注解是如何发挥作用的说明，即使有也是语焉不详。

## <span id="p_source">源码注解</span>

　　(空)

## <span id="p_class">编译期注解</span>：注解解析器

　　编译期注解，顾名思义，是在java代码编译为二进制码的时候起作用的注解。接下来将带领大家了解编译时到底如何对注解进行处理的，即注解解析器的工作原理。

　　既然是在编译器执行，那么自然不会和需要被编译的代码在同一个项目中，因此需要先独立于当前项目建立一个工程作为注解解析器，并打包成jar文件并引入当前项目中。

　　建立注解解析器工程Processor，此处以Maven工程为例

### <span id="p_c_regist">注册解析器</span>

　　／－

　　　｀－src

　　　　｜－test

　　　　｀－main

　　　　　｜－java

　　　　　｀－resources

　　在resources下建立META-INF文件夹，在META-INF中新建services文件夹，并在其中建立javax.annotation.processing.Processor配置文件

　　／－resources

　　　｀－META-INF

　　　　｀－services

　　　　　｀－javax.annotation.processing.Processor

　　将对应的类全名写入该文件中

```properties
package1.package2.Myprocessor1
package1.package2.Myprocessor2
package3.package4.Myprocessor3
com.TestProcessor
```

　　注：在idea中编译时有时会报下列错误

```java
Failed to execute goal org.apache.maven.plugins:maven-compiler-plugin:3.1:compile (default-compile) on project xlo-qc: Compilation failure
服务配置文件不正确, 或构造处理程序对象javax.annotation.processing.Processor: Provider com.TestProcessor not found时抛出异常错误
```

　　可以先删除META-INF等到打包完成后再将文件添加到jar中

### <span id="p_c_ap">虚处理器</span>

　　这里以TestProcessor为例，接下来进行处理器的编写。

　　每一个处理器都需要继承AbstractProcessor

```java
import javax.annotation.processing.AbstractProcessor;
import javax.annotation.processing.ProcessingEnvironment;
import javax.annotation.processing.RoundEnvironment;

public class TestProcessor extends AbstractProcessor {

	/**
	 * 每一个注解处理器类都必须有一个空的构造函数。
	 * 然而，这里有一个特殊的init()方法，
	 * 它会被注解处理工具调用，
	 * 并输入ProcessingEnviroment参数。
	 * ProcessingEnviroment提供很多有用的工具类Elements,
	 * Types和Filer。
	 */
	@Override
	public synchronized void init(ProcessingEnvironment processingEnv);

	/**
	 * 里你必须指定，这个注解处理器是注册给哪个注解的。
	 * 注意，它的返回值是一个字符串的集合，
	 * 包含本处理器想要处理的注解类型的合法全称。
	 * 换句话说，你在这里定义你的注解处理器注册到哪些注解上。。
	 * @return
	 */
	@Override
	public Set<String> getSupportedAnnotationTypes();

	/**
	 * 用来指定你使用的Java版本。
	 * 通常这里返回SourceVersion.latestSupported()。
	 * 然而，如果你有足够的理由只支持Java 6的话，
	 * 你也可以返回SourceVersion.RELEASE_6。
	 * @return
	 */
	@Override
	public SourceVersion getSupportedSourceVersion();

	/**
	 * 这相当于每个处理器的主函数main()。
	 * 你在这里写你的扫描、评估和处理注解的代码，
	 * 以及生成Java文件。输入参数RoundEnviroment，
	 * 可以让你查询出包含特定注解的被注解元素。
	 */
	@Override
	public boolean process(Set<? extends TypeElement> annotations, RoundEnvironment roundEnv);
    
}
```



#### init()

　　可以在init()中定义一些属性，

```java
import javax.annotation.processing.Messager;
import javax.lang.model.element.TypeElement;
import javax.lang.model.util.Elements;
import javax.lang.model.util.Types;

public class TestProcessor extends AbstractProcessor {
	// 用于处理TypeMirror的工具类
    private Types typeUtils;
    // 使用filer可以创建文件
    private Filer filer;
    // 用于输出编译信息
    private Messager messager;
    // 用于处理Element的工具类
    private Elements elementUtils;

    @Override
    public synchronized void init(ProcessingEnvironment processingEnvironment) {
        super.init(processingEnvironment);
        typeUtils = processingEnv.getTypeUtils();
        filer = processingEnv.getFiler();
        messager = processingEnv.getMessager();
        elementUtils = processingEnv.getElementUtils();
    }
}
```

　　但是查看AbstractProcessor的源码

```java
public abstract class AbstractProcessor implements Processor {
    /**
     * Processing environment providing by the tool framework.
     */
    protected ProcessingEnvironment processingEnv;
    
    /**
     * Initializes the processor with the processing environment by
     * setting the {@code processingEnv} field to the value of the
     * {@code processingEnv} argument.  An {@code
     * IllegalStateException} will be thrown if this method is called
     * more than once on the same object.
     *
     * @param processingEnv environment to access facilities the tool framework
     * provides to the processor
     * @throws IllegalStateException if this method is called more than once.
     */
    public synchronized void init(ProcessingEnvironment processingEnv) {
        if (initialized)
            throw new IllegalStateException("Cannot call init more than once.");
        Objects.requireNonNull(processingEnv, "Tool provided null ProcessingEnvironment");

        this.processingEnv = processingEnv;
        initialized = true;
    }
}
```

　　父类中在初始化时存在一个protected的ProcessingEnvironment和传入init()中的是同一个ProcessingEnvironment。



#### getSupportedAnnotationTypes()

　　在该方法中申明需要该处理器处理的注解。

```java
	@Override
	public Set<String> getSupportedAnnotationTypes() {
		Set<String> annotaions = new LinkedHashSet<>();
		annotaions.add(NotNull.class.getName());
		annotaions.add(Override.class.getName());
		return annotaions;
	}
```

　　在java7之后，可以使用类注解来代替该方法：

```java
@SupportedAnnotationTypes({
    // 合法注解全名的集合
})
public class TestProcessor extends AbstractProcessor {}
```



#### getSupportedSourceVersion()

　　用来指定使用的Java版本，但是经过测试，不实现该方法也没问题，解析器照样能够运行

```java
	@Override
	public SourceVersion getSupportedSourceVersion() {
		return SourceVersion.latestSupported();
	}
```

　　在java7之后，可以使用类注解来代替该方法：

```java
@SupportedSourceVersion(SourceVersion.latestSupported())
public class TestProcessor extends AbstractProcessor {}
```

　　注：该用法多见于网络，但是在idea上该写法无法通过编译，可能是被什么插件干扰了。



#### process()

　　相当于每个处理器的主函数main，

　　经测试，返回true或false时并不影响编译通过，不知道这个返回值的作用？



### <span id="p_c_process">处理器</span>

　　[一篇很详细的参考文档](https://blog.csdn.net/github_35180164/article/details/52055994)

　　在处理器的眼中，java文件只是一个文本文件，源代码的每一部分都是一个特定类型的Element，使用一种类似于解析XML文本一样的解析方式来结构化的处理源代码。

```java
package xxx.xxx; // PackageElement

public class TestClass{ // TypeElement
    private int a; // VariableElement
    private TestClass tc; // VariableElement
    public TestClass(){} // ExecuteableElement
    public void method1( // ExecuteableElement
    					int b // TypeElement
    					){}
}
```



### <span id="p_c_code">代码生成</span>

　　在编写代码生成模块时先参考lombok源码（待记录）：[Lombok][lombok]

[lombok]: lombok




## <span id="p_runtime">运行时注解</span>

