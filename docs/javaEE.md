[1. spring中的aop？](#1)

[2. aop中相关术语？](#2)

[3. aop实现日志配置](#3)

[4. aop实现事务配置](#4)

[5. ioc三种注入方式？](#5)

[6.@Resource和@Autowired的区别](#6)

[7. bean的作用域](#7)

[8. @Autowired的流程](#8)

[9.springMvc处理流程？](#9)

[10. url和uri的区别？](#10)

[11. ${} 和 #{}的区别？](#11)

[12. mybatis实体属性名和表中字段不一样怎么办？](#12)

[13. mybatis的mapper接口的工作原理？](#13)

[14. mybatis怎么获得自动生成的主键值](#14)

[15. mybatis动态sql 及其原理？](#15)

[16. mybatis 怎么解决第一个<if>为null导致的前and的问题](# 16)

[17. mybatis如何实现模糊查询？](#17)

[18. mybatis中除了增删改查标签，还有哪些？](#18)

[19. mybatis的缓存？](#19)

[20. 讲一下dubbo是什么？](#20)

[21. dubbo怎样向zookeeper注册和服务调用](#21)

### <span id="1">1.spring中的aop？</span>

​	（1）aop，面向切面编程，是对面向对象的一种补充（横向 纵向）就是找出对多个对象产生公共影响的行为，封装成一个可重用的模块。这样可以减少代码重复，降低耦合度（可用于事务 日志等）

​	（2）aop的关键在于aop框架自动创建aop代理，分为静态代理（在编译阶段生成代理类），动态代理（spring aop）。spring框架采用动态代理

​	（3）动态代理又有两种方式：JDK动态代理，cglib动态代理（当目标对象实现接口时用JDK动态代理，没有实现接口时用cglib代理）





### <span id="2">2. aop中相关术语？</span>

​	（1）切面：通用的业务逻辑代码

​	（2）连接点：虚概念，是指在应用执行过程中能够插入切面的一个点（可以是方法调用，抛出异常等）（spring只支持方法类型连接点，所以在spring中指方法调用）

​	（3）切点：具体确定的方法

​	（4）通知：拦截到连接点后具体要执行的代码（分为前置 后置 异常 环绕 返回）

​	（5）织入：将切面应用到目标对象的过程



### <span id="3">3. aop日志的具体实现</span>

​	基于注解实现

​	（1）springmvc.xml

```xml
<aop:aspectj-autoproxy proxy-target-class="true">  /*启用cglib代理*/
```

​		（proxy-target-class是基于类的代理  false将启用基于接口的JDK动态代理）

​	（2）配置切面

```java
@Aspect  
@Component  
public class LogInterceptor {  
    private final Logger logger = LoggerFactory.getLogger(LogInterceptor.class);  
    @Before(value = "execution(* com.gray.user.controller.*.*(..))")  
    public void before(){  
        logger.info("login start!");  
    }  
    @After(value = "execution(* com.gray.user.controller.*.*(..))")  
    public void after(){  
        logger.info("login end!");  
    }  
}
```



### <span id="4">4.aop实现事务配置</span>

​	（1）配置事务管理器

```xml
    <bean name="transactionManager" 	class="org.springframework.jdbc.datasource.DataSourceTransactionManager">
        <property name="dataSource" ref="dataSource"></property>
    </bean>
```

​	（2）开启基于注解的aop事务

```xml
 <tx:annotation-driven transaction-manager="transactionManager" />
```

​	（3）在使用事务的地方使用@Transaction注解（同时可以使用isolatio属性指定隔离级别 timeout指定超时时间 等等）



### <span id="5">5. ioc的三种注入方式？</span>

​	（1）构造方法注入

```xml
<bean id="userService" class="com.lyu.spring.service.impl.UserService">
	<constructor-arg ref="userDaoJdbc"></constructor-arg>
</bean>
```

​	（2）setter注入

```xml
<bean id="userService" class="com.lyu.spring.service.impl.UserService">
	<property name="userDao" ref="userDaoMyBatis"></property>
</bean>
```

​	（3）注解方式注入

@component @Repository @Controller @Service



### <span id="6">6. @Resource和@Autowired的区别？</span>

​	（1）Autowired是spring注解，默认ByType方式装配对象

​	（2）Resource是java注解，默认ByName方式装配对象



## <span id="7">7. bean的作用域</span>（scope=）

​	（1）Singleton：单实例

​	（2）Protype：多实例

​	（3）Request：web环境下，一次请求创建一个bean实例

​	（4）Session：web环境下，一次会话创建一个bean实例



### <span id="8">8. @Autowired的流程</span>

​	（1）先按类型到容器中找对应的组件，找到一个就装配

​	（2）没找到，抛异常

​	（3）找到多个，按变量名作为id继续装配

​	（4）如果变量名和id不一致，可修改变量名，也可加@Qualifier(“book”)指定id



### <span id="9">9. spring mvc处理流程？</span>

​	（1）用户向服务器发起请求，被前端控制器DispatcherServlet拦截

​	（2）DispatcherServlet对URL进行解析，得到URI，根据URI调用handlermapping得到Handler配置的所有相关对象（包括handler对象和handler对象对应的拦截器）返回个DispatcherServlet

​	（3）DispatcherServlet根据得到的handler选择合适的处理器适配器HandlerAdapter，执行Handler

​	（4）执行完成返回ModelAndView对象，选择一个合适的视图解析器ViewResolver返回给DispatcherServlet

​	（5）ViewResolver结合Model和View渲染视图，将渲染结果返回客户端

​	

### <span id="10">10.URL和URI的区别？</span>

​	URI:统一资源标志符，可以唯一标识一个资源

​	URL:统一资源定位符，不仅可以唯一标识一个资源，还可以唯一定位一个资源

​	（所以说URL是RUI的子集）

​	

### <span id="11">11. ${}和#{}的区别？</span>

​	（1）${}是字符串替换，mybatis会将${}替换为变量的值

​	（2）#{}是预编译处理，mybatis会将#{}替换为？，调用PreparedStatement的set方法来赋值，可以防止sql注入



### <span id="12">12.mybatis实体属性名和表中字段不一样怎么办？</span>

​	（1）sql语句中定义字段名的别名

```xml
	<select id=”selectorder” parametertype=”int” 		                               resultetype=”me.gacl.domain.order”>
       select order_id id, order_no orderno ,order_price price form orders where order_id=#{id};
    </select>

```

​	（2）通过<resultMap>来映射字段名和类属性名的对应关系

```xml
 <select id="getOrder" parameterType="int" resultMap="orderresultmap">
        select * from orders where order_id=#{id}
    </select>
 
   <resultMap type=”me.gacl.domain.order” id=”orderresultmap”>
        <!–用id属性来映射主键字段–>
        <id property=”id” column=”order_id”>
 
        <!–用result属性来映射非主键字段，property为实体类属性名，column为数据表中的属性–>
        <result property = “orderno” column =”order_no”/>
        <result property=”price” column=”order_price” />
    </reslutMap>

```



### <span id="13">13. mybatis的mapper接口的工作原理？</span>

​	mapper接口的全限定名就是映射文件中namespace的值，接口的方法名就是映射文件中MapperStatement的id值，接口方法内的参数，就是传递给sql的参数。工作原理就是JDK动态代理，mybatis运行时会使用JDK动态代理为mapper接口生成代理对象，代理对象拦截接口的方法，转而执行mapperstatment所代表的sql，然后将sql执行结果返回



### <span id="14">14. mybatis怎么获得自动生成的主键值？</span>



​	（1）方式一：自动生成的键在insert方法执行完后被设置到传入的参数对象中	

```xml
<insert id=”insertname” usegeneratedkeys=”true” keyproperty=”id”>
     insert into names (name) values (#{name})
</insert>

```

​	（2）方式二：使用<selectKey>标签

```xml
<insert id=”insertname”>
	<selectKey resultType="java.lang.Long" order="AFTER" keyProperty="id">
		SELECT_LAST_INSERT_ID()
	</selectKry>
     insert into names (name) values (#{name})
</insert>
```



### <span id="15">15. mybatis动态sql 及其原理？</span>

​	以标签的形式根据OGNL表达式的值动态的拼接sql

​	mybatis提供9种动态sql标签：if where trim choose  when  set otherwise bind（模糊查询）  foreach



### <span id="16">16. mybatis 怎么解决第一个if标签为null导致的前and的问题?</span>

​	（1）使用where标签

```xml
<select id="ListStudentByCondition" resultType="com.cn.cmc.bean.Student" >
    select * 
    from student
    <where>
        <if test="id!=null">
            and id = #{id}
        </if>
        <if test="name!=null and name.trim()!=''">
            and name = #{name}
        </if>
        <if test="age!=null && age!=""">
            and age = #{age}
        </if>
        <if test="sex=='M' or sex=='F'" >
            and trim(sex) = #{sex}
        </if>
    </where>
</select>

```

​	（2）使用trim标签

```xml
<select id="ListStudentByConditionTrim" resultType="com.cn.cmc.bean.Student" >
    <!-- trim标签拼接字符串
         prefix：给拼串后的字符串加上一个前缀
         prefixOverrides：去掉拼串后的字符串的一个前缀
         suffix：给拼串后的字符串加上一个后缀
         suffixOverrides：去掉拼串后字符串的一个前缀
         set标签：在更新中使用，可以去除多余的逗号
     -->
     select * 
     from student
    <trim prefix="where" prefixOverrides="and" suffix="" suffixOverrides="">
        <if test="id!=null">
            and id = #{id}
        </if>
        <if test="name!=null and name.trim()!=''">
            and name = #{name}
        </if>
        <if test="age!=null && age!=""">
            and age = #{age}
        </if>
        <if test="sex=='M' or sex=='F'" >
            and trim(sex) = #{sex}
        </if>
    </trim>
</select>

```



### <span id="17">17. mybatis如何实现模糊查询？</span>

​	使用bind标签，从OGNL表达式创建一个变量并绑定到元素中方便使用

```xml
<!-- 按名字模糊查询学生，假设在接口中有方法public Student getStudentByName(@Param("name")String name) ;
       bind标签：将OGNL表达式绑定到指定变量中
       name:绑定的变量名
       value：OGNL表达式
   -->
  <select id="getStudentByName" resultType="com.cn.cmc.bean.Student">
        <bind name="_name" value="'%'+name+'%'"/>
        select * 
        from student
        where name like #{_name}
  </select>

```



### <span id="18">18. mybatis中除了增删改查标签，还有哪些？</span>

​	<resultMap>:封装结果集

​	<sql>:sql片段标签

​	<include>:通过include引入sql片段

​	

### <span id="19">19. mybatis的缓存？</span>

​	（1）一级缓存：默认开启，是sqlsession层面进行的缓存（同一个sqlsession多次调用同一个方法，只会进行一次数据库查询）

​	（2）二级缓存：默认关闭，是mapper级别的缓存 （需要在mapper.xml文件下添加<cache/>来手动开启）



### <span id="20">20. 讲一下dubbo是什么？</span>

​	Dubbo是一个分布式服务框架，用来提供RPC远程服务调用方案，以及SOA（soa：面向服务的架构。Dubbo是soa的实现 ）服务治理方案。一般是一个系统充当服务端，向注册中心注册服务，一个是客户端，订阅注册中心的服务（zookeeper）



### <span id="21">21. dubbo怎样向zookeeper注册和订阅服务？</span>

​	（1）首先引入dubbo zookeeper相关jar包

​	（2）spring配置文件中

​		<dubbo:application name=*"dubbo**x**demo-service"*/>  

​		<dubbo:registry address=*"zookeeper://**192.16.25.132**:2181"*/> 

​		<dubbo:annotation package="cn.itcast.dubboxdemo.service" /> 

​	（3）在服务提供类进行@service注解，调用服务的地方用@refence注解