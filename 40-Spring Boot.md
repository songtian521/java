# 40-Spring Boot

# 1. Spring Boot 概述

Spring Boot帮助您创建可以运行的独立的，基于生产级的基于Spring的应用程序。我们对Spring平台和第三方库持固执己见的观点，这样您就可以以最小的麻烦开始使用。大多数Spring Boot应用程序只需要很少的Spring配置。

您可以使用Spring Boot创建Java应用程序，可以通过使用`java -jar`或更传统的战争部署来启动Java应用程序。我们还提供了一个运行“ spring脚本”的命令行工具。

**Spring Boot 主要特点是：**

- 创建独立的Spring应用，为所有 Spring 的开发者提供一个非常快速的、广泛接受的入门体验
- 直接嵌入应用服务器，如tomcat、jetty、undertow等；不需要去部署war包
- 提供固定的启动器依赖去简化组件配置；实现开箱即用（启动器starter-其实就是Spring Boot提供的一个jar包），通过自己设置参数（.properties或.yml的配置文件），即可快速使用。
- 自动地配置Spring和其它有需要的第三方依赖
- 提供了一些大型项目中常见的非功能性特性，如内嵌服务器、安全、指标，健康检测、外部化配置等
- 绝对没有代码生成，也无需 XML 配置。

**Spring Boot系统要求：**

Spring Boot 2.3.3.RELEASE**需要Java 8**，并且与**Java 14（包括）兼容**。 还需要Spring Framework 5.2.8.RELEASE或更高版本。

官网地址：https://spring.io/projects/spring-boot

# 2. Spring Boot快速入门

1. 创建普通maven工程，并添加如下坐标

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <project xmlns="http://maven.apache.org/POM/4.0.0"
            xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
            xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
       <modelVersion>4.0.0</modelVersion>
   
       <groupId>org.example</groupId>
       <artifactId>spring_boot_init</artifactId>
       <version>1.0-SNAPSHOT</version>
   
       <properties>
           <java.version>1.8</java.version>
       </properties>
       
       <!-- spring-boot-starter-parent 本身不提供任何依赖关系 -->
       <parent>
           <groupId>org.springframework.boot</groupId>
           <artifactId>spring-boot-starter-parent</artifactId>
           <version>2.3.3.RELEASE</version>
       </parent>
   
       <dependencies>
           <!-- 需要添加必要的依赖关系 -->
           <!-- 引入Spring Boot提供的自动配置依赖，我们称为 启动器 。 -->
           <dependency>
               <groupId>org.springframework.boot</groupId>
               <artifactId>spring-boot-starter-web</artifactId>
           </dependency>
       </dependencies>
       <build>
           <plugins>
               <plugin>
                   <groupId>org.springframework.boot</groupId>
                   <artifactId>spring-boot-maven-plugin</artifactId>
               </plugin>
           </plugins>
       </build>
   
   </project>
   ```

2. Spring Boot项目通过main函数即可启动，所以需要创建一个启动类

   注意：启动类所在的位置很关键，Spring Boot扫描相关注解是根据启动类所在位置及其子包进行扫描的，也就是说，如果位置不正确，SpringBoot无法注入Bean

   ```java
   @SpringBootApplication // 标注SpringBoot的启动类
   public class Application {
   
       /**
        * Application类”是指SpringBoot项目入口类。这个类的位置很关键：
        * 也就是说，spring会根据这个类的位置进行包扫描，扫描这个类所在的包及其子包哪
        */
       public static void main(String[] args) {
   
           // 代表运行SpringBoot的启动类，参数为SpringBoot启动类的字节码对象
           SpringApplication.run(Application.class,args);
       }
   
   }
   ```

3. 编写Controller

   ```java
   @RestController
   public class HelloController {
       @RequestMapping("/hello")
       public String hello(){
           System.out.println("hello spring boot");
           return "hello spring boot";
       }
   }
   ```

4. 启动Spring Boot，查看控制台输出

   可以看到的是，Spring Boot帮我们启动了很多组件，其中就包括了Tomcat，默认启动的端口是8080

5. 打开浏览器，测试，查看控制台输出以及页面内容

   http://localhost:8080/hello

# 3. Spring Boot 配置

## 3.1 java配置

java配置主要靠java类和一些注解，比较常用的注解有：

- @Configuration ：声明一个类作为配置类，代替xml文件
- @Bean ：声明在方法上，将方法的返回值加入Bean容器，代替` <bean>` 标签
- @Value ：属性注入
- @PropertySource ：指定外部属性文件。

测试配置数据源

1. 添加坐标信息

   ```xml
   <!--采用druid数据源-->
   <dependency>
       <groupId>com.alibaba</groupId>
       <artifactId>druid</artifactId>
       <version>1.1.6</version>
   </dependency>
   ```

2. 创建jdbc.properties配置文件

   ```properties
   jdbc.driverClassName=com.mysql.cj.jdbc.Driver
   jdbc.url=jdbc:mysql://localhost:3306/sys?useSSL=false&serverTimezone=UTC
   jdbc.username=root
   jdbc.password=password
   ```

3. 编写配置类信息

   注解说明：

   - @Configuration ：声明我们 JdbcConfig 是一个配置类
   - @PropertySource ：指定属性文件的路径是: classpath:jdbc.properties
   - 通过 @Value 为属性注入值
   - 通过@Bean将 dataSource() 方法声明为一个注册Bean的方法，Spring会自动调用该方法，将方法的返回值加入Spring容器中。

   然后我们就可以在任意位置通过 @Autowired 注入DataSource了！

   ```java
   @Configuration // 声明此类是一个配置类
   @PropertySource("classpath:jdbc.properties") // 指定配置文件路径
   public class JdbcConfig {
       @Value("${jdbc.url}") // 注入值
       String url;
       @Value("${jdbc.driverClassName}")
       String driverClassName;
       @Value("${jdbc.username}")
       String username;
       @Value("${jdbc.password}")
       String password;
       
       @Bean // 声明为一个注册Bean的方法，Spring会自动调用该方法，将方法的返回值加入Spring容器中
       public DataSource dataSource(){
           DruidDataSource dataSource = new DruidDataSource();
           dataSource.setDriverClassName(driverClassName);
           dataSource.setUrl(url);
           dataSource.setUsername(username);
           dataSource.setPassword(password);
           return dataSource;
       }
   }
   ```

4. 在Controller类中进行测试

   ```java
   @RestController
   public class HelloController {
       @Autowired
       private DataSource dataSource;
       
       @RequestMapping("/hello")
       public String hello(){
           System.out.println(dataSource);
           return "hello spring boot";
       }
   }
   ```

5. 查看输出

   此时，控制台输出如下内容，如果需要具体查看是否正确配置了数据源，可debug查看

   ```
   {
   	CreateTime:"2020-09-13 18:55:14",
   	ActiveCount:0,
   	PoolingCount:0,
   	CreateCount:0,
   	DestroyCount:0,
   	CloseCount:0,
   	ConnectCount:0,
   	Connections:[
   	]
   }
   ```

## 3.2 Spring Boot 属性注入

**注：**使用属性注入的时候，配置文件的默认文件名必须是：application.properties或application.yml

**注意：**SpringBoot**默认是以iso-8859 的编码方式读取application.properties**，以**utf-8读取application.yml**，所以推荐使用yml文件，不然在application.properties的中文参数会变成乱码。

在上面的案例中，我们实验了java配置方式。不过属性注入使用的是**@Value注解**。这种方式虽然可行，但是不够强大，因为**它只能注入基本类型值。**

在Spring Boot中，**提供了一种新的属性注入方式，支持各种java基本数据类型及复杂类型**的注入。

1. 新建属性注入类

   说明：

   - 在类上通过@ConfigurationProperties注解声明当前类为属性读取类
   - **prefix="jdbc" 读取属性文件中，前缀为jdbc的值。**
   - 在类上定义各个属性，名称必须与属性文件中 jdbc. 后面部分一致
   - 需要注意的是，这里我们并没有指定属性文件的地址，所以**我们需要把jdbc.properties名称改为application.properties，**这是Spring Boot默认读取的属性文件名

   ```java
   @ConfigurationProperties(prefix = "jdbc") // 声明当前类为属性读取类
   public class JdbcProperties {
   
       // 名称必须与属性文件中 jdbc. 后面部分一致
       private String url;
       private String driverClassName;
       private String username;
       private String password;
   
   	// 省略get and set方法
   }
   ```

   注意：此时`@ConfigurationProperties(prefix = "jdbc") `注解下会多一道红色波浪线，但它不影响运行，可以通过添加如下坐标去掉上述提示

   ```xml
   <dependency>
       <groupId> org.springframework.boot </groupId>
       <artifactId>spring-boot-configuration-processor</artifactId>
       <!--不传递依赖-->
       <optional>true</optional>
   </dependency>
   ```

2. 修改jdbcConfig.java文件

   说明：通过 `@EnableConfigurationProperties(JdbcProperties.class) `来声明要使用 JdbcProperties 这个类的对象

   ```java
   @Configuration // 声明此类是一个配置类
   // 来声明要使用 JdbcProperties 这个类的对象
   @EnableConfigurationProperties({JdbcProperties.class})
   public class JdbcConfig {
   
       @Bean // 声明为一个注册Bean的方法，Spring会自动调用该方法，将方法的返回值加入Spring容器中
       public DataSource dataSource(JdbcProperties jdbc){
           DruidDataSource dataSource = new DruidDataSource();
           dataSource.setDriverClassName(jdbc.getDriverClassName());
           dataSource.setUrl(jdbc.getUrl());
           dataSource.setUsername(jdbc.getUsername());
           dataSource.setPassword(jdbc.getPassword());
           return dataSource;
       }
   }
   ```

3. 要使用配置的话；可以通过以下方式注入JdbcPropertie

   - @Autowired注入

     ```java
     @Autowired
     private JdbcProperties prop;
     ```

   - 构造函数注入

     ```java
     private JdbcProperties prop;
     public JdbcConfig(Jdbcproperties prop){
         this.prop = prop;
     }
     ```

   - 声明有@Bean的方法参数注入

     ```java
     @Bean
     public Datasource dataSource(JdbcProperties prop){
         // ...
     }
     ```

4. 测试

   ```java
   @RestController
   public class HelloController {
   
       @Autowired
       private JdbcProperties prop;
   
       @RequestMapping("/hello")
       public String hello(){
           System.out.println("hello spring boot");
           System.out.println(prop);
           return "hello spring boot";
       }
   }
   ```

   查看输出

   ```
   hello spring boot
   com.it.song.config.JdbcProperties@c91845c
   ```

5. 疑问：

   也许大家会觉得这种方式似乎更麻烦了，事实上这种方式有更强大的功能，也是Spring Boot推荐的注入方式。与@Value对比关系：

   | 二者区别             | @ConfigurationProperties | @Value     |
   | -------------------- | ------------------------ | ---------- |
   | 功能                 | 批量注入配置文件中的属性 | 一个个指定 |
   | 松散绑定（松散语法） | 支持                     | 不支持     |
   | SpEL                 | 不支持                   | 支持       |
   | JSR303数据校验       | 支持                     | 不支持     |
   | 复杂类型封装         | 支持                     | 不支持     |

   Relaxed binding：松散绑定

   - 不严格要求属性文件中的属性名与成员变量名一致。支持驼峰，中划线，下划线等等转换，甚至支持对象

     - 举个栗子：学生类当中的 firstName 属性，在配置文件中可以叫 firstName、first-name、first_name 以及 FIRST_NAME。而 @ConfigurationProperties 是支持这种命名的，@Value 不支持。

     - 引导。比如：user.friend.name：代表的是user对象中的friend属性中的name属性，显然friend也是对象。@value注解就难以完成这样的注入方式。

   - meta-data support：元数据支持，帮助IDE生成属性提示（写开源框架会用到）。

## 3.3 优雅注入

事实上，如果一段属性只有一个Bean需要使用，我们无需将其注入到一个类（JdbcProperties，将该类上的所有注解去掉）中。而是直接在需要的地方声明即可；

再次修改 JdbcConfig 类为如下代码：

```java
@Configuration
public class JdbcConfig {
    @Bean
    // 声明要注入的属性前缀，Spring Boot会自动把相关属性通过set方法注入到DataSource中
    @ConfigurationProperties(prefix = "jdbc")
    public DataSource dataSource() {
        return new DruidDataSource();
    }
}
```

我们直接把` @ConfigurationProperties(prefix = "jdbc") `声明在需要使用的 @Bean 的方法上，然后SpringBoot就会自动调用这个Bean（此处是DataSource）的set方法，然后完成注入。**使用的前提是：该类必须有对应属性的set方法！**

## 3.4 Yaml配置文件

yaml简介：

> YML文件格式是YAML (YAML Aint Markup Language)编写的文件格式，YAML是一种直观的能够被电脑识别的的数据数据序列化格式，并且容易被人类阅读，容易和脚本语言交互的，可以被支持YAML库的不同的编程语言程序导入，比如： C/C++, Ruby, Python, Java, Perl, C#, PHP等。YML文件是以数据为核心的，比传统的xml方式更加简洁。

配置文件除了可以使用`application.properties`类型，还可以使用后缀名为：`.yml`或者`.yaml`的类型，也就是：`application.yml`或者`application.yaml`

基本格式：key: value

```yaml
name: zhangsan
```

注意：value之前有一个空格

示例：

```yaml
jdbc:
  driverClassName: com.mysql.cj.jdbc.Driver
  url: jdbc:mysql://localhost:3306/sys?useSSL=false&serverTimezone=UTC
  username: root
  password: password
```

把application.properties修改为application.yml进行测试

> 如果两个配置文件都有，会把两个文件的配置合并，如果有重复属性，以properties中的为准。
> 如果是配置数组、list、set等结构的内容，那么在yml文件中格式为：
>
> ```yaml
> key:
>     - value1
>     - value2
> ```
>
> 

再次运行，访问http://localhost:8080/hello，查看是否能正确输出

## 3.5 多Yaml配置

当一个项目中有多个yml配置文件的时候，可以以`application-**.yml`命名；application.yml中配置项目使用激活这些配置文件即可。

1. 创建application-def.yml文件如下

   ```yaml
   it:
     test: 1234
   ```

2. 在 application.yml 文件中添加如下配置：

   ```yml
   #加载其它配置文件
   spring:
     profiles:
       # abc是文件application-***.yaml的 *
       active: abc,def
   
   ```

   多个文件名只需要写application-之后的名称，在多个文件之间使用,隔开

3. 测试

   ```java
   @RestController
   public class HelloController {
   
       @Value("${it.test}")
       private String test;
   
       @Autowired
       private JdbcProperties prop;
   
       @RequestMapping("/hello")
       public String hello(){
           System.out.println("hello spring boot");
           System.out.println(prop);
   
           System.out.println(test);
           return "hello spring boot";
       }
   }
   ```

   输出：

   ```
   hello spring boot
   com.it.song.config.JdbcProperties@291f93aa
   1234
   ```

# 4. 自动配置原理

## 4.1 @SpringBootApplication注解

使用Spring Boot之后，一个整合了SpringMVC的WEB工程开发，变的无比简单，那些繁杂的配置都消失不见了，这是如何做到的？

一切魔力的开始，都是从我们的main函数来的，所以我们再次来看下启动类：

```java
@SpringBootApplication // 标注SpringBoot的启动类
public class Application {

    /**
     * Application类”是指SpringBoot项目入口类。这个类的位置很关键：
     * 也就是说，spring会根据这个类的位置进行包扫描，扫描这个类所在的包及其子包哪
     */
    public static void main(String[] args) {
        // 代表运行SpringBoot的启动类，参数为SpringBoot启动类的字节码对象
        SpringApplication.run(Application.class,args);
    }
}
```

可以发现有**两个特别的地方**：

- 注解：` @SpringBootApplication`
- run方法：` SpringApplication.run(Application.class,args);`

我们可以点进`@SpringBootApplication`注解**查看源码：**

```java
@Target({ElementType.TYPE})
@Retention(RetentionPolicy.RUNTIME)
@Documented
@Inherited
@SpringBootConfiguration
@EnableAutoConfiguration
@ComponentScan(
    excludeFilters = {@Filter(
    type = FilterType.CUSTOM,
    classes = {TypeExcludeFilter.class}
), @Filter(
    type = FilterType.CUSTOM,
    classes = {AutoConfigurationExcludeFilter.class}
)}
)
public @interface SpringBootApplication {
}
```

可以看到 `SpringBootApplication` 注解是一个组合注解，其主要组合了以下三个注解：

- `@SpringBootConfiguration`：该注解表示这是一个 SpringBoot 的配置类，其实它就是一个 @Configuration 注解而已。
- `@ComponentScan`：开启组件扫描。
- `@EnableAutoConfiguration`：从名字就可以看出来，就是这个类开启自动配置的。嗯，自动配置的奥秘全都在这个注解里面。



接着再次点击进入@SpringBootConfiguration注解源码

```java
@Target({ElementType.TYPE})
@Retention(RetentionPolicy.RUNTIME)
@Documented
@Configuration
public @interface SpringBootConfiguration {
}
```

通过这段我们可以看出，在这个注解上面，又有一个 @Configuration 注解。通过上面的注释阅读我们知道：这个注解的作用就是声明当前类是一个配置类，然后Spring会自动扫描到添加了 @Configuration 的类，并且读取其中的配置信息。而 @SpringBootConfiguration 是来声明当前类是SpringBoot应用的配置类，项目中只能有一个。所以一般我们无需自己添加。

## 4.2 @EnableAutoConfiguration注解说明

定义：

```java
@Target({ElementType.TYPE})
@Retention(RetentionPolicy.RUNTIME)
@Documented
@Inherited
@AutoConfigurationPackage
@Import({AutoConfigurationImportSelector.class})
public @interface EnableAutoConfiguration {}
```

- @AutoConfigurationPackage：就是将主配置类（`@SpringBootConfiguration`标注的类）的**所在包及下面所有子包**里面的所有组件扫描到Spring容器中。所以说，默认情况下主配置类包及子包以外的组件，Spring 容器是扫描不到的。
- Import({AutoConfigurationImportSelector.class})：该注解给当前配置类导入另外的 spring Boot已经配置好的类

**总结**：Spring Boot内部对大量的第三方库或Spring内部库进行了默认配置，这些配置是否生效，取决于我们是否引入了对应库所需的依赖，如果有那么默认配置就会生效。

所以，我们使用SpringBoot构建一个项目，只需要引入所需框架的依赖，配置就可以交给SpringBoot处理了。除非你不希望使用SpringBoot的默认配置，它也提供了自定义配置的入口。

## 4.3 @ComponentScan 注解说明

配置组件扫描的指令。提供了类似与 `<context:component-scan> `标签的作用

通过basePackageClasses或者basePackages属性来指定要扫描的包。如果没有指定这些属性，那么将从声明这个注解的类所在的包开始，扫描包及子包

而我们的@SpringBootApplication注解声明的类就是main函数所在的启动类，因此扫描的包是该类所在包及其子包。**因此，一般启动类会放在一个比较前的包目录中**。



## 4.4 @EnableAutoConfiguration自动配置原理

在SpringApplication类构建的时候，有一段初始化代码，通过loadFactoryNames尝试加载一些FactoryName，然后利用createSpringFactoriesInstances将这些加载到的类名进行实例化，此处会利用类加载器加载某个文件： FACTORIES_RESOURCE_LOCATION ，然后解析其内容。我们找到这个变量的声明：

```java
public static final String FACTORIES_RESOURCE_LOCATION = "META-INF/spring.factories";
```

可以发现，其地址是： META-INF/spring.factories ，我们知道，ClassLoader默认是从classpath下读取文件，因此，SpringBoot会在初始化的时候，加载所有classpath:META-INF/spring.factories文件，包括jar包当中的。

而在Spring的一个依赖包：spring-boot-autoconfigure中，就有一个spring.factories这样的文件，以后我们引入的任何第三方启动器，只要实现自动配置，也都会有类似文件。

## 4.5 总结

SpringBoot为我们提供了默认配置，而默认配置生效的步骤：

- @EnableAutoConfiguration注解会去寻找 META-INF/spring.factories 文件，读取其中以EnableAutoConfiguration 为key的所有类的名称，这些类就是提前写好的自动配置类
- 这些类都声明了 @Configuration 注解，并且通过 @Bean 注解提前配置了我们所需要的一切实例
- 但是，这些配置不一定生效，因为有 @ConditionalOn 注解，满足一定条件才会生效。比如条件之一： 是一些相关的类要存在
- 类要存在，我们只需要引入了相关依赖（启动器），依赖有了条件成立，自动配置生效。
- 如果我们自己配置了相关Bean，那么会覆盖默认的自动配置的Bean
- 我们还可以通过配置application.yml文件，来覆盖自动配置中的属性

1. 启动器

   所以，我们如果不想配置，只需要引入依赖即可，而依赖版本我们也不用操心，因为只要引入了SpringBoot提供的stater（启动器），就会自动管理依赖及版本了。

   因此，玩SpringBoot的第一件事情，就是找启动器，SpringBoot提供了大量的默认启动器

2. 全局配置
   另外，SpringBoot的默认配置，都会读取默认属性，而这些属性可以通过自定义application.properties 文件来进行覆盖。这样虽然使用的还是默认配置，但是配置中的值改成了我们自定义的。

   因此，玩SpringBoot的第二件事情，就是通过 application.properties 来覆盖默认属性值，形成自定义配置。我们需要知道SpringBoot的默认属性key，非常多，可以再idea中自动提示

3. 属性文件支持两种格式，`application.properties`和`application.yml`

   yml语法示例

   ```yml
   jdbc:
       driverClassName: com.mysql.jdbc.Driver
       url: jdbc:mysql://127.0.0.1:3306/springboot_test
       username: root
       password: root
   server:
       port: 80
   ```

   如果properties和yml文件都存在，如果有重叠属性，默认以Properties优先。

# 5. Spring Boot 整合

## 5.1 认识Lombok

我们编写pojo时，经常需要编写构造函数和getter、setter方法，属性多的时候，就非常浪费时间，使用lombok插件可以解决这个问题：

在IDEA中安装lombok插件；不安装插件在IDEA中使用lombok的注解虽然编译能通过，但是源码会报错。所以为了让IDEA更好的辨别lombok注解则才安装插件

添加坐标

```xml
<dependency>
    <groupId>org.projectlombok</groupId>
    <artifactId>lombok</artifactId>
</dependency>
```

然后可以在Bean上使用：

- @Data ：自动提供getter和setter、hashCode、equals、toString等方法
- @Getter：自动提供getter方法
- @Setter：自动提供setter方法
- @Slf4j：自动在bean中提供log变量，其实用的是slf4j的日志功能。

例如；在javabean上加@Data，那么就可以省去getter和setter等方法的编写，lombok插件会自动生成

## 5.2  整合SpringMVC

虽然默认配置已经可以使用SpringMVC了，不过我们有时候需要进行自定义配置。

### 5.2.1 配置日志级别控制级别

可以在 application.yml 文件中配置日志级别控制级别

```yml
# 日志级别控制
logging:
  level:
    com.it.song: debug
    org.springframework: info
```

### 5.2.2 修改默认tomcat端口

查看SpringBoot的全局属性可知，端口通过以下方式配置：

修改 application.yml 配置文件，添加如下配置：

```yml
# 映射端口
server:
  port: 80
```

重启服务后测试，查看能否正常输出

### 5.2.3 访问静态资源

默认的静态资源路径为：

```
classpath:/META-INF/resources/
classpath:/resources/
classpath:/static/
classpath:/public
```

只要静态资源放在这些目录中任何一个，SpringMVC都会帮我们处理。我们习惯会把静态资源放在 classpath:/static/ 目录下

示例：创建一个a.html文件，放在static文件夹下

```html
<!DOCTYPE html>
<html lang="en">
    <head>
        <meta charset="UTF-8">
        <title>Title</title>
    </head>
    <body>
        <h1> 测试静态资源 </h1>
    </body>
</html>
```

访问：http://localhost:80/a.html，测试能否正常显示

### 5.2.4 添加拦截器

拦截器不是一个普通属性，而是一个类，所以就要用到java配置方式了。在SpringBoot官方文档中有这么一段说明：

> 如果你想要保持Spring Boot 的一些默认MVC特征，同时又想自定义一些MVC配置（包括：拦截器，格式化器,视图控制器、消息转换器 等等），你应该让一个类实现 WebMvcConfigurer ，并且添加 @Configuration 注解，但是千万不要加 @EnableWebMvc 注解。如果你想要自定义 HandlerMapping 、 HandlerAdapter 、ExceptionResolver 等组件，你可以创建一个 WebMvcRegistrationsAdapter 实例 来提供以上组件。如果你想要完全自定义SpringMVC，不保留SpringBoot提供的一切特征，你可以自己定义类并且添加@Configuration 注解和 @EnableWebMvc 注解

总结：通过实现 WebMvcConfigurer 并添加 @Configuration 注解来实现自定义部分SpringMvc配置。

1. 创建MyInterceptor.java 拦截器，内容如下：

   ```java
   @Slf4j
   public class MyInterceptor implements HandlerInterceptor {
   
       @Override
       public boolean preHandle(HttpServletRequest request, HttpServletResponse response, Object handler) throws Exception {
           log.debug("preHandle.....测试");
           return true;
       }
   
       @Override
       public void postHandle(HttpServletRequest request, HttpServletResponse response, Object handler, ModelAndView modelAndView) throws Exception {
           log.debug("postHandle.....测试");
       }
   
       @Override
       public void afterCompletion(HttpServletRequest request, HttpServletResponse response, Object handler, Exception ex) throws Exception {
           log.debug("afterCompletion.....测试");
       }
   }
   
   ```

2.  定义配置类\MvcConfig.java ，用于注册拦截器，内容如下：

   ```java
   @Configuration
   public class MvcConfig implements WebMvcConfigurer {
       // 将拦截器注册到spring ioc容器
       @Bean
       public MyInterceptor myInterceptor(){
           return new MyInterceptor();
       }
       /**
        * 重写该方法；往拦截器链添加自定义拦截器
        * @param registry 拦截器链
        */
       @Override
       public void addInterceptors(InterceptorRegistry registry) {
           //通过registry添加myInterceptor拦截器，并设置拦截器路径为 /*
           registry.addInterceptor(myInterceptor()).addPathPatterns("/*");
       }
   }
   
   ```

3. 接下来访问http://localhost:80/hello 并查看日志

## 5.3 整合jdbc和事务

spring中的jdbc连接和事务是配置中的重要一环，在SpringBoot中该如何处理呢？

答案是不需要处理，我们只要找到SpringBoot提供的启动器即可，在 pom.xml 文件中添加如下依赖：

```xml
<!--整合jdbc和事务-->
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-jdbc</artifactId>
</dependency>
<!--数据库驱动-->
<dependency>
    <groupId>mysql</groupId>
    <artifactId>mysql-connector-java</artifactId>
    <version>8.0.12</version>
</dependency>
```

至于**事务，SpringBoot中通过注解来控制**。就是我们熟知的` @Transactional` 使用的时候设置在对应的类或方法上即可

1. 创建实体类

   ```java
   //自动提供getter和setter、hashCode、equals、toString等方法
   //@Data
   
   @Getter
   @Setter
   @Table(name = "user")
   public class User {
   
       private Integer id;
       private String username;
       private String password;
       private Date birthday;
   }
   
   ```

2. 创建userService.java

   ```java
   @Service
   public class userService {
       public User queryId(Integer id){
          return new User();
       }
   }
   ```

3. 修改controller

   ```java
   @RestController
   public class HelloController {
       @RequestMapping("/user/{id}")
       public User user(@PathVariable Integer id){
           return userService.queryId(id);
       }
   }
   
   ```

4. 访问http://localhost:80/user/2

   输出如下：

   ```json
   {
       id: null,
       username: null,
       password: null,
       birthday: null
   }
   ```

   为什么都是null？原因是我们new了一个user直接返回，没有进行数据库查询

## 5.4 整合链接池

其实，在刚才引入jdbc启动器的时候，SpringBoot已经自动帮我们引入了一个连接池：

默认连接池：`com.zaxxer:HikariCP:3.4.5`

因此，我们只需要指定连接池参数即可；打开 application.yml 添加修改配置如下:

```yml
spring:
  datasource:
    driver-class-name: com.mysql.cj.jdbc.Driver
    url: jdbc:mysql://localhost:3306/sys?useSSL=false&serverTimezone=UTC
    username: root
    password: password
```

【注意】
把 JdbcConfig 类中的druid的相关配置删除或注释

测试：访问http://localhost:80/hello，查看后台输出，一样可以在HelloController中获取到datasource

controller类

```java
@RestController
public class HelloController {

    @Autowired
    private DataSource dataSource;

    @RequestMapping("/hello")
    public String hello(){
        System.out.println("hello spring boot");
        System.out.println(dataSource);
        return "hello spring boot";
    }
}
```

输出如下：

```
hello spring boot
HikariDataSource (null)
```

## 5.5 整合mybatis

### 5.5.1 mybatis

SpringBoot官方并没有提供Mybatis的启动器，不过Mybatis官网自己实现了。在项目的 pom.xml 文件中加入如下依赖

```xml
<!--mybatis -->
<dependency>
    <groupId>org.mybatis.spring.boot</groupId>
    <artifactId>mybatis-spring-boot-starter</artifactId>
    <version>2.0.1</version>
</dependency>
```

配置 application.yml ，常用配置如下：

```yml
# mybatis相关配置
mybatis:
  # 实体类别名
  type-aliases-package: com.it.song.pojo
  # 映射文件路径
#  mapper-locations: classpath:mappers/*.xml
  configuration:
    # 控制台输出执行sql
    log-impl: org.apache.ibatis.logging.stdout.StdOutImpl
```

配置Mapper扫描
需要注意，这里没有配置mapper接口扫描包，因此我们需要给每一个Mapper接口添加 `@Mapper `注解，才能被识别

```java
@Mapper
public interface UserMapper {
}
```

或者，我们也可以不加注解，而是在启动类上添加扫描包注解**(推荐)**：

```java
@SpringBootApplication // 标注SpringBoot的启动类
@MapperScan("com.it.song.mapper") // 是在启动类上添加mapper扫描包注解(推荐)：
public class Application {
    public static void main(String[] args) {
        SpringApplication.run(Application.class,args);
    }
}
```

### 5.5.2 通用mapper

1. 通用Mapper的作者也为自己的插件编写了启动器，我们直接引入即可。在项目的 pom.xml 文件中加入如下依赖：

   ```xml
   <!-- 通用mapper -->
   <dependency>
       <groupId>tk.mybatis</groupId>
       <artifactId>mapper-spring-boot-starter</artifactId>
       <version>2.1.5</version>
   </dependency>
   ```

   注意：一旦引入了通用Mapper的启动器，会覆盖Mybatis官方启动器的功能，因此**需要移除对官方Mybatis启动器的依赖**

2. 编写UserMapper

   无需任何配置就可以使用了。如果有特殊需要，可以到通用mapper官网查看：https://github.com/abel533/Mapper/wiki/3.config

   编写userMapper.java

   ```java
   import tk.mybatis.mapper.common.Mapper;
   public interface userMapper extends Mapper<User> {
       
   }
   ```

3. 把启动类上的`@MapperScan`注解修改为通用mapper中自带的：

   ```java
   import tk.mybatis.spring.annotation.MapperScan;
   
   @SpringBootApplication // 标注SpringBoot的启动类
   @MapperScan("com.it.song.mapper") // 是在启动类上添加mapper扫描包注解(推荐)
   public class Application {
       public static void main(String[] args) {
           SpringApplication.run(Application.class,args);
       }
   }
   ```

4. 在User实体类上加JPA注解

   ```java
   //自动提供getter和setter、hashCode、equals、toString等方法
   //@Data
   
   @Getter
   @Setter
   @Table(name = "user")
   public class User {
   
       @Id // 表明这个是主键
       @KeySql(useGeneratedKeys = true) //开启主键自动回填
       private Integer id;
       
   //  @Column(name = "username")  // 当属性名和数据库的字段相同时，可省略
       private String username;
       private String password;
       private Date birthday;
   }
   
   ```

5.  对 UserService 的代码进行简单改造

   ```java
   @Service
   public class userService {
   
       @Autowired
       private userMapper userMapper;
   
       public User queryId(Integer id){
           // 根据id查询
           //通用 Mapper 的 selectByPrimaryKey 方法无法识别 int 类型，
           // 需要在 POJO 类中将 int 改为包装类型 Integer
           // 需要对主键加上注解 @Id
           return userMapper.selectByPrimaryKey(id);
      
       }
   
       @Transactional
       public void saveUser(User user){
           userMapper.insertSelective(user);
           System.out.println("新增用户...");
       }
   }
   
   ```

6. 启动测试

   修改controller

   ```java
   @RestController
   public class HelloController {
   
       @Autowired
       private userService userService;
   
       @RequestMapping("/user/{id}")
       public User user(@PathVariable Integer id){
           return userService.queryId(id);
       }
   }
   ```

7. 测试访问：http://localhost:80/user/2

   输出：

   ```json
   {
       id: 2,
       username: "zhangsan1",
       password: "12341",
       birthday: null
   }
   ```

## 5.6 Junit测试

1. 在springboot项目中如果要使用Junit进行单元测试，则需要添加如下的依赖：

   ```xml
   <!-- 测试相关类 -->
   <dependency>
       <groupId>org.springframework.boot</groupId>
       <artifactId>spring-boot-starter-test</artifactId>
       <scope>test</scope>
   </dependency>
   ```

2. 在测试包下编写测试类
   在测试类上面必须要添加 @SpringBootTest 注解

   ```java
   @RunWith(SpringRunner.class)
   @SpringBootTest
   public class userServiceTest {
       @Autowired
       private userService userService;
   
       @Test
       public void queryById(){
           User user = userService.queryId(2);
           System.out.println(user);
       }
   
       @Test
       public void  saveUser(){
           User user = new User();
           user.setUsername("lisi");
           user.setPassword("123123123");
           user.setBirthday(new Date());
           userService.saveUser(user);
   
       }
   }
   ```

3. 执行测试，查看控制台输出以及数据库

## 5.7 整合Redis

1. 在 pom.xml 文件中添加如下依赖

   ```xml
   <!--整合Redis-->
   <dependency>
       <groupId>org.springframework.boot</groupId>
       <artifactId>spring-boot-starter-data-redis</artifactId>
   </dependency>
   ```

2. 配置 application.yml 文件；

   ```yml
   spring:
     redis:
       host: localhost
       port: 6379
   ```

3. 测试

   记得启动redis软件

   ```java
   @RunWith(SpringRunner.class)
   @SpringBootTest
   public class RedisTest {
   
       @Autowired
       private RedisTemplate redisTemplate;
   
       @Test
       public void test(){
           //string字符串
           //redisTemplate.opsForValue().set("str", "heima");
           redisTemplate.boundValueOps("str").set("heima");
           System.out.println("str = " + redisTemplate.opsForValue().get("str"));
           //hash散列
           redisTemplate.boundHashOps("h_key").put("name", "黑马");
           redisTemplate.boundHashOps("h_key").put("age", 13);
           //获取所有域对应的值
           Set set = redisTemplate.boundHashOps("h_key").keys();
           System.out.println("hash散列所有的域：" + set);
           List list = redisTemplate.boundHashOps("h_key").values();
           System.out.println("hash散列所有的域值：" + list);
           //list列表
           redisTemplate.boundListOps("l_key").leftPush("c");
           redisTemplate.boundListOps("l_key").leftPush("b");
           redisTemplate.boundListOps("l_key").leftPush("a");
           list = redisTemplate.boundListOps("l_key").range(0, -1);
           System.out.println("列表的值：" + list);
           //set集合
           redisTemplate.boundSetOps("set_key").add("a", "b", "c");
           set = redisTemplate.boundSetOps("set_key").members();
           System.out.println("集合的元素：" + set);
           //sorted set有序集合
           redisTemplate.boundZSetOps("z_key").add("a", 30);
           redisTemplate.boundZSetOps("z_key").add("b", 20);
           redisTemplate.boundZSetOps("z_key").add("c", 10);
           set = redisTemplate.boundZSetOps("z_key").range(0, -1);
           System.out.println("有序集合的元素：" + set);
       }
   
   }
   ```

4. 查看输出

   ```
   str = heima
   hash散列所有的域：[age, name]
   hash散列所有的域值：[13, 黑马]
   列表的值：[a, b, c, a, b, c]
   集合的元素：[c, b, a]
   有序集合的元素：[c, b, a]
   ```

   

# 6. Spring Boot项目部署

1. 添加项目的pom.xml插件；在pom.xml要显式的加入插件spring-boot-maven-plugin，否则无法产生 jar 清单文件，导致打出来的 jar 无法使用命令运行；

   ```xml
   <build>
       <plugins>
           <plugin>
               <!-- 打jar包时如果不配置该插件，打出来的jar包没有清单文件 -->
               <groupId>org.springframework.boot</groupId>
               <artifactId>spring-boot-maven-plugin</artifactId>
           </plugin>
       </plugins>
   </build>
   ```

2. 使用package进行打包，打包出来的文件在target目录中

3. 运行

   运行打出来的包；使用命令：` java –jar `包全名 或者写一个 bat 文件，里面包含 java –jar 包全名；这样就可以双击启动应用

   示例：

   ```
   java -jar heima-springboot-1.0-SNAPSHOT.jar
   ```

4. 测试访问

   http://localhost:80/hello