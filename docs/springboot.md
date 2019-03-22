[一.helloworld](#1)

[二.引入配置的两种方式？](#2)

[三. 激活指定的profile？](#3)

[四.配置文件优先级顺序？](#4)

[五.自动配置原理？](#5)

[六.日志的实现？](#6)

#### <span id="1">一. helloworld </span>

1. 创建maven工程 ，引入相关依赖

   ```xml
    <parent>
           <groupId>org.springframework.boot</groupId>
           <artifactId>spring-boot-starter-parent</artifactId>
           <version>1.5.9.RELEASE</version>
       </parent>
   
       <dependencies>
           <dependency>
               <groupId>org.springframework.boot</groupId>
               <artifactId>spring-boot-starter-web</artifactId>
           </dependency>
       </dependencies>
   
       <build>
           <!-- 这个插件，可以将应用打包成一个jar包 -->
           <plugins>
               <plugin>
                   <groupId>org.springframework.boot</groupId>
                   <artifactId>spring-boot-maven-plugin</artifactId>
               </plugin>
           </plugins>
       </build>
   ```

   2. 编写主程序类

   ```java
   @SpringBootApplication
   public class HelloWorldMainApplication {
   
       public static void main(String[] args) {
           SpringApplication.run(HelloWorldMainApplication.class, args);
       }
   }
   ```

   3.创建controller

   ```java
   @Controller
   public class HelloController {
   
       @ResponseBody
       @RequestMapping("/hello")
       public String hello(){
           return "hello world";
       }
   }
   ```

      启动main方法，访问localhost:8080/hello



#### <span id="2">二. 引入配置的两种方式？</span>

1.对于全局配置文件（application.properties）

|                      | @configurationProperties(prefix="") | @value                       |
| -------------------- | ----------------------------------- | ---------------------------- |
| 功能                 | 批量注入配置文件中的属性            | 一个个指定                   |
| 松散绑定（松散语法） | 支持（last-name 等价于lastName）    | 不支持                       |
| spel                 | 不支持                              | 支持                         |
| JSR303数据校验       | 支持                                | 不支持                       |
| 复杂类型封装         | 支持                                | 不支持（只能取基本类型数据） |

JSR303数据校验：

```java
@Component
@ConfigurationProperties(prefix = "person")
@Validated
public class Person {

    private String name;

    @Email
    //@Value("${person.last-name}")
    private String email;
}
```



2.对于其它指定的配置文件

@propertySource（value = {"classpath:person.properties}）



#### <span id="3">三. 激活指定的profile？</span>

1. 在配置文件编写的时候，文件名可以是 application-{profile}.pr.properties , 默认使用application.properties

 2. 激活指定的profile

    （1）配置文件：

```properties
spring.profiles.active=dev
```

​	（2）命令行方式

```properties
java -jar xxx.jar --spring.profiles.active=dev	
```



#### <span id="4">四. 配置文件优先级顺序？</span>

file:/config/   >  file:/    >  classpath:/config/   >   classpath:/

优先级高的会覆盖优先级低的相同配置，不同的配置会按优先级依次加载



#### <span id=5>五. 自动配置原理？</span>

​	1.springboot启动时加载主配置类，开启自动配置功能@EnableAutoConfiguration

​	2.@EnableAutoConfiguration利用EnableAutoConfigurationImportSelector选择器给容器中导入组件，扫描所有jar包类路径下的META-INF/spring.factories，获取到EnableAutoConfiguration的值添加在容器中。这样每一个XXAutoConfiguration都是容器中的一个组件，都加入到容器中做自动配置

​	3.每一个自动配置类进行自动配置功能



#### <span id="6">六. 日志的实现？</span>

​	1.日志种类

| 日志门面（日志的抽象层） | 日志实现                                |
| ------------------------ | --------------------------------------- |
| JCL SLF4j  jboss-logging | log4j  log4j2  Logback（是log4j的升级） |

springboot选用： 日志门面——SLF4j	日志实现——Logback

​	2.日志级别

由高到低：trace <debug < info < warn < error （springboot默认是info级别的）

​	3.springboot日志配置

```properties
#设置日志级别
logging.level.com.kaka=trace  
#当前项目下生成日志(也可以写路径)
logging.file=springboot.log  
#指定路径
logging.path=/spring/log   
#（文件中）日志格式
logging.pattern.file=%d{yyyy-MM-dd}[%thread] %-5level %logger{50} -%msg%n  
```

