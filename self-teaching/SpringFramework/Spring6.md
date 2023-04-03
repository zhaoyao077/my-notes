# Spring6

> 2023/3/3始

## 1. Java反射机制



### 1.1 反射机制概述

- Reflection是被视为动态语言的关键，反射机制允许程序在执行期借助于Reflection API取得任何类的内部信息，并能直接操作任意对象的内部属性和方法。
- 加载完类之后，在堆内存的方法区就产生了一个Class类型的对象，这个对象包含了完整的类的结构信息，我们可以通过这个对象看到类的结构。
- Java不是动态语言，可以称为”准动态语言“。



### 1.2 反射机制的功能

- 在运行时判断任意一个对象所属的类
- 在运行时构造任意一个类的对象
- 在运行时判断任意一个类所具有的成员变量和方法
- 在运行时获取泛型信息
- 在运行时调用任意一个对象的成员变量和方法
- 在运行时处理注解
- 生成动态代理



### 1.3 反射的实例

```java
Class clazz = Person.class;

//通过反射，创建Person类的对象
Constructor con = clazz.getConstructor(String.class,int,class);
Object obj = con.newInstance("Tom",12);
Person p = (Person)obj;

//通过反射调用对象的属性，方法
Field age = clazz.getDeclaredField("age");
Method show = clazz.getDeclaredMethod("show");
show.invoke(p);

//通过反射，可以调用Person类的私有属性和方法
Field name = clazz.getDeclaredField("name");
name.setAccessible(true);//设置属性为可访问
name.set(p1,"Jack");
```



### 1.4 反射与封装性

- 对于public的结构，一般使用类的属性和方法直接调用，而反射一般用在代码运行时才能确定需要创建哪个类的实例这种情况。
- 反射对于服务端程序很重要，因为要根据前端发来的URL来判断需要提供什么业务。



## 2. Spring概述



### 2.1 Spring是什么

- Spring是一款主流的Java EE轻量级开源框架，由Rod Johnson提出并创立，目的是简化Java企业级应用的开发难度和开发周期。
- 2004年4月发布Spring 1.0版本。当前最新版本是Spring6，且Spring6需要的Java版本最低是**jdk17**。



### 2.2 Spring的狭义与广义

- 狭义的Spring：Spring Framework。
  - IoC：Inverse of Control，控制反转。
  - AOP：Aspect Oriented Programming，面向切面编程。
- 广义：Spring的各种子模块



### 2.3 Spring Framework的特点

- 非侵入式
- 控制反转
- 面向切面编程
- 容器
- 组件化
- 一站式



### 2.4 Spring模块组成

- Spring Core
- Spring AOP
- Spring Data Access
- Spring Web
- Spring Message
- Spring Test



## 3. 入门案例



### 3.1 入门案例开发步骤

1. 引入Spring相关依赖
2. 创建类，定义属性方法
3. 创建xml配置文件
4. 在配置文件中配置相关信息
5. 测试运行



### 3.2 代码

> 见D:\code\self-teaching\spring_framework\spring6



### 3.3 Log4j2日志框架

- Apache Log4j2是一个开源的日志记录组件，使用非常的广泛，在工程中以易用方便代替了System.out等打印语句，是Java目前最流行的日志工具。
- 日志信息优先级：（优先级从低到高）
  - TRACE
  - DEBUG
  - INFO
  - WARE
  - ERROR
  - FATAL



## 4. IoC 控制反转



### 4.1 IoC概述

- IoC 是 Inversion of Control 的简写，译为“控制反转”，它不是一门技术，而是一种**设计思想**，是一个重要的面向对象编程法则，能够指导我们如何设计出松耦合、更优良的程序。
- Spring 通过 IoC 容器来管理**所有 Java 对象的实例化和初始化**，控制对象与对象之间的**依赖关系**。
- 我们将由 IoC 容器管理的 Java 对象称为 **Spring Bean**，它与使用关键字 new 创建的 Java 对象没有任何区别。
- IoC 容器是 Spring 框架中最重要的核心组件之一，它贯穿了 Spring 从诞生到成长的整个过程。



### 4.2 依赖注入

- DI（Dependency Injection），依赖注入，指Spring创建对象的过程中，将对象依赖属性通过配置进行注入。
- 依赖注入常见的实现方式包括两种：
  1. set注入
  2. 构造注入
- IoC 就是一种控制反转的思想， 而 DI 是对IoC的一种具体实现。



## 5. 基于XML管理Bean

### 5.1 获取Bean

- 在xml配置文件中配置

  ```xml
  <bean id="user" class="com.nju.spring6.iotxml.User"></bean>
  ```

- 第一种获取方式：根据id获取Bean

  ```java
  //根据id获取bean
  User user1 = (User) ctx.getBean("user");
  System.out.println("根据id获取bean: " + user1);
  ```

- 第二种获取方式：根据class获取Bean

  ```java
  //根据类型获取bean，注意这种方式一个类只允许创建一个对象
  User user2 = (User) ctx.getBean(User.class);
  System.out.println("根据class获取bean: " + user2);
  ```

- 第三种获取方式：根据id和class获取bean

  ```java
  //根据id和类型获取bean
  User user3 = (User) ctx.getBean("user",User.class);
  System.out.println("根据id和class类型获取bean: " + user3);
  ```

- 扩展

  - 如果组件类实现了接口，根据接口类型可以获取 bean 吗？
    - 可以，前提是**bean唯一**
  - 如果一个接口有多个实现类，这些实现类都配置了 bean，根据接口类型可以获取 bean 吗？
    - 不行，因为bean不唯一
  - 结论：根据类型来获取bean时，在满足bean唯一性的前提下，其实只是看：『对象 **instanceof** 指定的类型』的返回结果，只要返回的是true就可以认定为和类型匹配，能够获取到。



### 5.2 setter注入

1. 原生方法

   1. set方法注入，即通过属性的setter赋值
   2. 构造器注入，即通过构造器传参初始化

2. 通过spring实现

   1. 通过set方法注入

      需要Book类中对应的属性都有set方法

      ```xml
       <!--set方法注入-->
      <bean id="book" class="com.nju.spring6.iotxml.di.Book">
              <property name="bookName" value="front end"></property>
              <property name="author" value="zhaoyao"></property>
      </bean>
      ```

   2. 基于构造器方式注入

      需要Book类中有含对应参数的构造器

      ```xml
      <!--构造器方式注入-->
      <bean id="bookCon" class="com.nju.spring6.iotxml.di.Book">
              <constructor-arg name="bookName" value="back end"></constructor-arg>
              <constructor-arg name="author" value="zhao yao"></constructor-arg>
      </bean>
      ```




### 5.3 特殊值处理

1. 字面量属性赋值

   ```xml
   <!-- 使用value属性给bean的属性赋值时，Spring会把value属性的值看做字面量 -->
   <property name="name" value="张三"/>
   ```

2. null值

   ```xml
   <property name="name">
       <null />
   </property>
   ```

3. xml实体需要用特殊符号代替

   ```xml
   <!-- 小于号在XML文档中用来定义标签的开始，不能随便使用 -->
   <!-- 解决方案一：使用XML实体来代替 -->
   <property name="expression" value="a &lt; b"/>
   ```

4. CDATA节来使用特殊符号

   ```xml
   <property name="expression">
       <!-- 解决方案二：使用CDATA节 -->
       <!-- CDATA中的C代表Character，是文本、字符的含义，CDATA就表示纯文本数据 -->
       <!-- XML解析器看到CDATA节就知道这里是纯文本，就不会当作XML标签或属性来解析 -->
       <!-- 所以CDATA节中写什么符号都随意 -->
       <value><![CDATA[a < b]]></value>
   </property>
   ```



### 5.4 对象类型注入

> 见代码

1. 引用外部bean

   ```xml
   <!-- 方式一: 引入外部bean
        1.创建两个类的bean:dept和emp
        2.在emp的bean标签中使用property的ref属性引入外部bean的id
   -->
   <bean id="dept" class="com.nju.spring6.iotxml.ditest.Dept">
           <property name="deptName" value="保安"></property>
   </bean>
   
   <bean id="emp" class="com.nju.spring6.iotxml.ditest.Emp">
           <property name="empName" value="Jack"></property>
           <property name="age" value="30"></property>
           <property name="dept" ref="dept"></property>
   </bean>
   ```

2. 内部bean

   ```xml
   <!-- 方式二: 内部bean-->
   <bean id="emp2" class="com.nju.spring6.iotxml.ditest.Emp">
       <property name="empName" value="John"></property>
       <property name="age" value="40"></property>
       <property name="dept">
           <bean id="dept2" class="com.nju.spring6.iotxml.ditest.Dept">
               <property name="deptName" value="财务"></property>
           </bean>
       </property>
   </bean>
   ```

3. 级联属性赋值

   ```xml
   <!-- 方式三: 级联属性赋值-->
   <bean id="dept3" class="com.nju.spring6.iotxml.ditest.Dept">
       <property name="deptName" value="开发部门"></property>
   </bean>
   <bean id="emp3" class="com.nju.spring6.iotxml.ditest.Emp">
       <property name="empName" value="Mary"></property>
       <property name="age" value="20"></property>
       <property name="dept" ref="dept3"></property>
       <property name="dept.deptName" value="测试部门"></property>
   </bean>
   ```



### 5.5 数组类型注入

- Emp类的属性

  ```java
  private Dept dept;
  
  private String EmpName;
  
  private Integer age;
  
  private String[] hobbies;
  ```

- xml配置文件

  ```xml
  <!-- 数组类型注入 -->
      <bean id="dept" class="com.nju.spring6.iotxml.ditest.Dept">
          <property name="deptName" value="保安部门"></property>
      </bean>
      <bean id="emp" class="com.nju.spring6.iotxml.ditest.Emp">
          <!--普通属性-->
          <property name="empName" value="Jack"></property>
          <property name="age" value="20"></property>
          <!--对象属性-->
          <property name="dept" ref="dept"></property>
          <!--数组属性-->
          <property name="hobbies">
              <array>
                  <value>抽烟</value>
                  <value>喝酒</value>
                  <value>烫头</value>
              </array>
          </property>
      </bean>
  ```



### 5.6 其他类型注入

- List类型
- Map类型
- p名称空间注入



### 5.7 引入外部文件注入

- 较为繁琐，不常用



### 5.8 Bean的作用域

bean标签的scope属性：

- singleton 在IoC容器初始化时创建
- prototype 获取bean时创建



### 5.9 bean生命周期

1. bean对象创建（调用无参数构造器）
2. 给bean设置相关属性
3.  bean后置处理器（初始化之前）BeanPostProcessor
4. bean对象初始化 init-method
5. bean后置处理器（初始化之后）
6. bean对象创建完成 destroy-method
7. bean对象销毁
8. IoC容器关闭



### 5.10 FactoryBean

- FactoryBean是Spring提供的一种**整合第三方框架**的常用机制。和普通的bean不同，配置一个FactoryBean类型的bean，在获取bean的时候得到的并不是class属性中配置的这个类的对象，而是getObject()方法的返回值。通过这种机制，Spring可以帮我们把复杂组件创建的详细过程和繁琐细节都屏蔽起来，只把最简洁的使用界面展示给我们。
- 将来我们整合Mybatis时，Spring就是通过FactoryBean机制来帮我们创建SqlSessionFactory对象的。



### 5.11 基于xml自动装配

- 使用bean标签的autowire属性自动装配
  - byName
  - byType



## 6. 基于注解管理bean

### 6.1 创建bean对象

#### 注解

- 从 Java 5 开始，Java 增加了对注解（Annotation）的支持，它是代码中的**一种特殊标记**，可以在编译、类加载和运行时被读取，执行相应的处理。开发人员可以通过注解在不改变原有代码和逻辑的情况下，在源代码中嵌入补充信息。
- Spring 从 2.5 版本开始提供了对注解技术的全面支持，我们可以使用注解来实现自动装配，简化 Spring 的 XML 配置。

#### 步骤

1. 引入依赖
2. 开启组件扫描
3. 使用注解定义bean
4. 依赖注入

#### 使用注解定义Bean

| 注解        | 说明                                                         |
| ----------- | ------------------------------------------------------------ |
| @Component  | 该注解用于描述 Spring 中的 Bean，它是一个泛化的概念，仅仅表示容器中的一个组件（Bean），并且可以作用在应用的任何层次，例如 Service 层、Dao 层等。  使用时只需将该注解标注在相应类上即可。 |
| @Repository | 该注解用于将数据访问层（Dao 层）的类标识为 Spring 中的 Bean，其功能与 @Component 相同。 |
| @Service    | 该注解通常作用在业务层（Service 层），用于将业务层的类标识为 Spring 中的 Bean，其功能与 @Component 相同。 |
| @Controller | 该注解通常作用在控制层（如SpringMVC 的 Controller），用于将控制层的类标识为 Spring 中的 Bean，其功能与 @Component 相同。 |



### 6.2 @Autowire注解

#### 该注解可以标注在哪里？

1. 构造方法上

   ```java
   @Autowired//构造方法注入
   public UserController(UserService userService) {
       this.userService = userService;
   }
   ```

2. 只有一个构造器时，可以不使用Autowired

   ```java
   //只有一个构造器时，不需要加Autowired也可以完成注入
   public UserController(UserService userService) {
       this.userService = userService;
   }
   ```

3. 方法上

   ```java
   @Autowired//set方法注入
   public void setUserService(UserService userService) {
       this.userService = userService;
   }
   ```

4. 形参上

   ```java
   //形参上注入
   public UserController(@Autowired UserService userService) {
   	this.userService = userService;
   }
   ```

5. 属性上

   ```java
   @Autowired//属性注入
   private UserService userService;
   ```

6. 注解上

   ```java
   @Autowired
   @Qualifier("userDaoImpl") // 指定bean的名字
   private UserDao userDao;
   ```

#### 注意

该注解有一个required属性，默认值是true，表示在注入的时候要求被注入的Bean必须是存在的，如果不存在则报错。如果required属性设置为false，表示注入的Bean存在或者不存在都没关系，存在的话就注入，不存在的话，也不报错。



### 6.3 @Resource注解

#### @Resource和@Autowired注解的区别

- @Resource是JDK扩展包中的，也就是说属于JDK的一部分。所以该注解是标准注解，更加具有通用性；@Autowired是属于Spring框架的。
- **@Resource注解默认根据名称装配byName，未指定name时，使用属性名作为name。通过name找不到的话会自动启动通过类型byType装配；@Autowired注解默认根据类型装配byType，如果想根据名称装配，需要配合@Qualifier注解一起用。**
- @Resource注解用在属性上、setter方法上；@Autowired注解用在属性上、setter方法上、构造方法上、构造方法参数上。

#### 依赖（如果高于jdk11或低于jdk8，需要引入依赖）

```xml
<dependency>
    <groupId>jakarta.annotation</groupId>
    <artifactId>jakarta.annotation-api</artifactId>
    <version>2.1.1</version>
</dependency>
```



### 6.4 全注解开发

用@Configuration注释的配置类代替spring配置文件

```java
@Configuration//配置类
@ComponentScan("com.nju.spring6")//开启组件扫描
public class SpringConfig {
}
```



## 7. 手写IoC

> 见代码



## 8. AOP

### 8.1 代理模式

设计模式中的一种，属于结构型模式。它的作用就是通过提供一个代理类，让我们在调用目标方法的时候，不再是直接对目标方法进行调用，而是通过代理类**间接**调用。让不属于目标方法核心逻辑的代码从目标方法中剥离出来（**解耦**）。调用目标方法时先调用代理对象的方法，减少对目标方法的调用和打扰，同时让附加功能能够集中在一起也有利于统一维护。



### 8.2 AOP概念

AOP（Aspect Oriented Programming）是一种设计思想，是软件设计领域中的面向切面编程，它是面向对象编程的一种补充和完善，它以通过预编译方式和运行期动态代理方式实现，在不修改源代码的情况下，给程序动态统一添加额外功能的一种技术。利用AOP可以对业务逻辑的各个部分进行隔离，从而使得业务逻辑各部分之间的耦合度降低，提高程序的可重用性，同时提高了开发的效率。



### 8.3 AOP术语

1. 横切关注点
2. 通知（增强）

   1. 前置通知：在被代理的目标方法**前**执行
   2. 返回通知：在被代理的目标方法**成功结束**后执行（**寿终正寝**）
   3. 异常通知：在被代理的目标方法**异常结束**后执行（**死于非命**）
   4. 后置通知：在被代理的目标方法**最终结束**后执行（**盖棺定论**）
   5. 环绕通知：使用try...catch...finally结构围绕**整个**被代理的目标方法，包括上面四种通知对应的所有位置
3. 切面：封装通知方法的类。
4. 目标：被代理的目标对象。
4. 代理：向目标对象应用通知之后创建的代理对象。
4. 连接点：这也是一个纯逻辑概念，不是语法定义的。



### 8.4 动态代理

1. JDK动态代理：代理对象和目标对象实现同样的接口
2. cglib代理：通过继承被代理的目标类实现
3. AspectJ：是AOP思想的一种实现，本质上是静态代理，Spring借用了该注释，最终效果是动态的。



### 8.5 切入点语法

![img025](D:\课程\A其他\SpringFramework\img025-16787581183261.png)



### 8.6 AOP实例

```java
//在add方法调用前执行
@Before(value = "execution(public int com.nju.spring6.annoaop.CalculatorImpl.add(..))")
public void beforeMethod(){
    System.out.println("[Log]before method...");
}

//通过JointPoint获取目标的信息
@AfterReturning(value = "execution(* com.nju.spring6.annoaop.CalculatorImpl.*(..))",returning = "result")
public void afterReturnMethod(JoinPoint joinPoint, Object result){
    System.out.println(joinPoint.getSignature().getName() + " returns");
    System.out.println("result: " + result);
}
```



### 8.7 重用切入点表达式

```java
//重用切入点表达式
@Pointcut(value = "execution(* com.nju.spring6.annoaop.CalculatorImpl.*(..))")
public void pointCut(){}

@After(value = "pointCut()")
public void afterMethod(){
    System.out.println("[Log]after method...");
}
```



### 8.8 切面优先级

相同目标方法上同时存在多个切面时，切面的优先级控制切面的**内外嵌套**顺序。

- 优先级高的切面：外面
- 优先级低的切面：里面

使用@Order注解可以控制切面的优先级：

- @Order(较小的数)：优先级高
- @Order(较大的数)：优先级低



### 8.9 用xml实现AOP

不常用，知道即可

```xml
<context:component-scan base-package="com.atguigu.aop.xml"></context:component-scan>

<aop:config>
    <!--配置切面类-->
    <aop:aspect ref="loggerAspect">
        <aop:pointcut id="pointCut" 
                   expression="execution(* com.atguigu.aop.xml.CalculatorImpl.*(..))"/>
        
        <aop:before method="beforeMethod" pointcut-ref="pointCut"></aop:before>
        
        <aop:after method="afterMethod" pointcut-ref="pointCut"></aop:after>
        
        <aop:after-returning method="afterReturningMethod" returning="result" pointcut-ref="pointCut"></aop:after-returning>
        
        <aop:after-throwing method="afterThrowingMethod" throwing="ex" pointcut-ref="pointCut"></aop:after-throwing>
        
        <aop:around method="aroundMethod" pointcut-ref="pointCut"></aop:around>
    </aop:aspect>
</aop:config>
```



## 9. 事务

### 9.1 JDBCTemplate

- spring对JDBC进行了封装，称为JDBCTemplate
- 详情见代码`spring6\spring-jdbc-tx`



### 9.2 事务

#### 什么是事务

数据库事务( transaction)是访问并可能操作各种数据项的一个数据库操作序列，这些操作要么全部执行,要么全部不执行，是一个不可分割的工作单位。事务由事务开始与事务结束之间执行的全部数据库操作组成。

#### 事务的特性

**A：原子性(Atomicity)**

一个事务(transaction)中的所有操作，要么全部完成，要么全部不完成，不会结束在中间某个环节。事务在执行过程中发生错误，会被回滚（Rollback）到事务开始前的状态，就像这个事务从来没有执行过一样。

**C：一致性(Consistency)**

事务的一致性指的是在一个事务执行之前和执行之后数据库都必须处于一致性状态。

如果事务成功地完成，那么系统中所有变化将正确地应用，系统处于有效状态。

如果在事务中出现错误，那么系统中的所有变化将自动地回滚，系统返回到原始状态。

**I：隔离性(Isolation)**

指的是在并发环境中，当不同的事务同时操纵相同的数据时，每个事务都有各自的完整数据空间。由并发事务所做的修改必须与任何其他并发事务所做的修改隔离。事务查看数据更新时，数据所处的状态要么是另一事务修改它之前的状态，要么是另一事务修改它之后的状态，事务不会查看到中间状态的数据。

**D：持久性(Durability)**

指的是只要事务成功结束，它对数据库所做的更新就必须保存下来。即使发生系统崩溃，重新启动数据库系统后，数据库还能恢复到事务成功结束时的状态。
