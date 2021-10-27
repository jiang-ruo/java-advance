# SpringBoot配置项

1. [Spring Core Properties](#cp)
2. [Spring Web Properties](#wp)



属性值之后跟随的值为默认值

## <span id="cp">Spring Core Properties</span>

概要：

```yml
debug:
info:
trace:
logging:
spring:
  aop:
  application:
  # 就是每次启动项目后控制台打印Spring字符的位置
  banner:
  config:
  # 配置META-INF和git相关
  info:
  jmx:
  main:
  # 返回消息相关
  messages:
  # springboot的其它配置文件
  profiles:
  quartz:
  # 定时任务
  task:
```



```yml
# debug logs
debug: false

# unknow
info.*=

# 启用跟踪日志
trace: false

logging:
  charset:
    # 输出到控制台时使用的字符集
    console:
    # 输出到文件时使用的字符集
    file:
  # 用于指定配置文件的位置，该文件配置日志的输出格式
  # e.g.: classpath:logback.xml
  config:
  # unknow
  # 记录异常时使用的转换字
  exception-conversion-word: %wEx
  file:
    # 日志文件名称
    name:
    # 日志文件的位置
    path:
  # unknow
  # 日志组
  group.*:
  # unknow
  # 日志级别严重性映射
  level.*:
  logback:
    rollingpolicy:
      # 是否在启动时清除存档的日志文件
      clean-history-on-start: false
      # 定义输出的文件名，使用%d之类的可以使文件名按数字自增
      file-name-pattern: ${LOG_FILE}.%d{yyyy-MM-dd}.%i.gz
      # 单个日志文件的最大大小
      # 到达最大大小后根据file-name-pattern配置，生成下一个日志文件的文件名
      # (如果只指定一个文件名，不知道会不会覆写)
      max-file-size: 10MB
      # 日志文件保留的天数
      max-history: 7
      # unknow
      # 要保留的日志备份的总大小
      total-size-cap: 0B
  # unknow
  # 里面的解释都不太理解，附加模式？
  # 里面的属性都有很长的默认值
  pattern:
    console: 
    dateformat: yyyy-MM-dd HH:mm:ss.SSS
    file:
    level: %5p
  # 初始化日志系统时，注册一个关闭挂钩
  register-shutdown-hook: false

spring:
  aop:
    # 开启spring AOP
    # (可能是在注解解析器里在class文件中某个位置添加@EnableAspectJAutoProsy注解?)
    auto: true
    # unknow
    # 与CGLIB和JDK代理有关，某种代理的手段，可能是启用CGLIB代理(，好像不论如何都启用JDK代理?)?
    proxy-target-class: true
  application:
    admin:
      # unknow
      # 是否为应用程序启用管理功能
      enabled: false
      # unknow
      jmx-name: org.springframework.boot:type=Admin,name=SpringApplication
    # 应用名称
    name:
  autoconfigure:
    # 要排除的自动配置类
    exclude: 
  # 配置标语文件Banner File 
  # 就是每次启动项目后控制台打印Spring字符的位置
  banner:
    charser: UTF-8
    image:
      bitdepth:
      height: 
      # unknow
      # 控制台颜色是黑色的时候是否反转图像？
      invert: false
      # 最新的springboot支持图片格式
      location: classpath:banner.gif
      # 左边距
      margin: 2
      # 渲染图像时使用的像素模式
      pixelmode: TEXT
      width: 76
    # 标语文字资源位置
    location: classpath: banner.txt
  # unknow
  # 是否跳过对BeanInfo类的搜索
  bannerinfo:
    ignore: true
  # unknow
  codec:
    log-request-details: false
    max-in-memory-size: 
  # 配置文件
  config:
    activate:
      # unknow
      # 怀疑和spring-cloud-config有关
      on-cloud-platform:
      # unknow
      on-profile:
    # 使用多个配置文件时，另一个配置文件的位置
    additional-location:
    # 导入其它配置数据
    import:
    # 修改配置文件的默认位置
    location:
    name: application
    # unknow
    # 是否兼容旧的配置方法？
    use-legacy-processing: false
  info:
    # 打包相关
    build:
      encoding: UTF-8
      location: classpath:META-INF/build-info.properties
    # unknow
    git:
      encoding: UTF-8
      location: classpath:git.properties
  # unknow
  # jmx系列，不知道是什么东西
  jmx:
    default-domain:
    enabled: false
    server: mbeanServer
    unique-names: false
  lifecycle:
    # unknow - 不知道any phase指的是什么
    # 配置每个生命周期的超时关闭时长
    # group of SmartLifecycle beans with the same 'phase' value
    timeout-per-shutdown-phase: 30s
  main:
    # unknow
    # 是否允许使用同名的bean覆盖bean定义
    allow-bean-definition-overriding: false
    # 显示banner的模式
    banner-mode: console
    # unknow
    # 覆盖Cloud Platform
    cloud-platform:
    # 懒加载，在使用时创建对应的bean放入bean容器中
    lazy-initialization: false
    # 启动时记录有关应用程序的信息
    log-startup-info: true
    # 应用程序注册一个关闭挂钩
    register-shutdown-hook: true
    # unknow
    # 要包括在ApplicationContext中的Sources(类名，包名或XML资源位置)
    soures: 
    # unknow
    # 在Web环境中运行应用程序(默认情况下自动检测)
    web-application-type:
  # 规定应用应当使用的编码
  mandatory-file-encoding:
  # 和返回消息给前端相关
  # 不知道messages系列作用在哪里
  messages:
    # 是否始终应用MessageFormat规则，甚至分析不带参数的消息
    always-use-message-format: false
    # unknow
    basename: messages
    # 加载的资源文件缓存持续时间，未指定后缀，则为s
    cache-duration:
    # 消息编码
    encoding: UTF-8
    # 未指定编码是否启用系统编码环境
    # 如果为false则使用messages.properties中指定的编码
    fallback-to-system-locale: true
    # true - 使用消息代码作为默认消息
    # false - 引发 NoSuchMessageException
    use-code-as-default-message: false
    ansi:
      # 配置ANSI输出
      enabled: detect
  # unknow
  pid:
    fail-on-write-error:
    file:
  # springboot的其它配置文件
  profiles:
    # 规定启用的配置文件类型
    # application-dev.yml -- active: dev
    # dev: 开发环境，test： 测试环境，prod： 生产环境
    # 怀疑active的值可以随意填写，只需要和application-之后的值相同即可
    # 可以输入多个值，以Comma(逗号？顿号？)分割
    active:
    # unknow
    # 用于同事激活多个配置文件
    # https://www.cnblogs.com/better-farther-world2099/articles/11738143.html
    include:
  # unknow
  quartz:
    # 初始化后是否自动启动调度程序
    auto-startup: true
    jdbc:
      # sql初始化脚本中单行注释的前缀
      comment-prefix: [#, --]
      # 数据库模式初始化模式
      initialize-schema: embedded
      # 用于初始化数据库模式的sql文件的路径
      schema: classpath:org/quartz/impl/jdbcjobstore/tables_@@platform@@.sql
    # unknow
    # quartz作业储存类型
    job-store-type: memory
    # know
    # 配置的作业是否覆盖现有作业
    overwrite-existing-jobs: false
    # know
    # quartz scheduler的其它属性
    properties.*:
    # 调度程序名称
    scheduler-name: quartzScheduler
    # unknow
    start-up-delay: 0s
    # unknow
    wait-for-jobs-to-complete-on-shutdown: false
    # unknow
    debug-agent:
      enabled: true
  # spring定时任务相关
  # spring定时任务的调用顺序为：任务调度线程调用任务执行线程执行任务
  # https://blog.csdn.net/qq_35067322/article/details/103982304
  task:
    # 任务执行线程池
    execution:
      pool:
        # 是否允许核心线程超时，可以动态增加/减少线程池
        allow-core-thread-timeout: true
        # 核心线程数
        core-size: 8
        # 线程最大空闲时间(超过该时长则结束线程)
        keep-alive: 60s
        # 最大线程数，任务数量≠线程数量
        max-size:
        # unknow
        # 队列容量，(没有该属性则max-size无效？)
        queue-capacity:
      shutdown:
        # 程序结束前是否等待线程完成任务
        await-termination: false
        # 接上，最长等待时间
        await-termination-period:
      # 线程前缀
      thread-name-prefix: task-
    # 任务调度线程池
    # 与@EnableScheduling、@Scheduled注解有关
    scheduling:
      pool:
        size: 1
      shutdown:
        # 程序结束前是否等待线程完成任务
        await-termination: false
        # 接上，最长等待时间
        await-termination-period:
      # 线程前缀
      thread-name-prefix: scheduling-
```



ref: https://docs.spring.io/spring-boot/docs/current/reference/html/appendix-application-properties.html#common-application-properties-core



## <span id="wp">Spring Web Properties</span>

概要：

```yml
spring:
  jersey
```



```yml
spring:
  hateoas:
    # unknow
    use-hal-as-default-json-media-type: true
  # unknow
  jersey:
    # 如果指定，将覆盖@ApplicationPath的值
    application-path:
    filter:
      # jersey过滤器顺序
      order: 0
    # 给予jersey的初始化参数
    init.*:
    servlet:
      # 加载jersey servlet的启动优先级
      load-on-startup: -1
    # jersey的集成类型
    type: servlet
```



ref: https://docs.spring.io/spring-boot/docs/current/reference/html/appendix-application-properties.html#common-application-properties-web

