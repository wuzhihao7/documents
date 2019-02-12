# 注解

## @Autowired

我们可以使用`@Autowired`注解来标记Spring将要解析和注入的依赖关系。我们可以将这个注解与构造函数、setter函数或成员变量注入一起使用

> 构造器注入

```java
@RestController
public class Controller{
    private Service service;
    
    @Autowired
    public Controller(Service service){
        this.service = service;
    }
}
```

> setter注入

```java
@RestController
public class Controller{
    private Service service;
    
    @Autowired
    public void setService(Service service){
        this.service = service;
    }
}
```

> 领域注入

```java
@RestController
public class Controller{
    @Autowired
    private Service service;
}
```

## @Bean

`@Bean`是方法级注解，是xml元素的直接模拟。注解支持一些提供的属性，例如：`init-method`、`destroy-method`、`auto-wiring`和`name`。

> 可以在`@Configuration`或`@Component`注解类中使用`@Bean`

```java
@Configuration
public class App{
    @Bean
    public Service service(){
        return new Service();
    }
}
```

> 上述配置等价于xml配置

```xml
<beans>
    <bean id="service" class="com.house.service.Service"></bean>
</beans>
```

## @Qualifier

此注解有助于微调基于注解的自动布线。可能存在这样的情况：我们创建多个相同类型的bean，并且只想使用属性连接其中一个bean。这可以使用`@Qualifier`和`@Autowired`来控制。

示例：考虑使用EmailService和SMSService类来实现单个MessageSevice接口

> 为多个消息服务实现创建MessageService接口。

```java
public interface MessageService{
    void sendMsg(String message);
}
```

> 实现类

```java
public class SMSService implements MessageService{
    public void sendMsg(String message){
        System.out.println(message);
    }
}

public class EmailService implements MessageService{
    public void sendMsg(String message){
        System.out.println(message);
    }
}
```

> @Qualifier用法

```java
public interface MessageProcessor{
    void processMsg(String message);
}

public class MessageProcessorImpl implements MessageProcessor{
    private MessageService messageService;
    
    //constructor based DI
    @Autowired
    public MessageProcessorImpl(@Qualifier("emailService") MessageService messageService){
        this.messageService = messageService;
    }
    
    //setter based DI
    @Autowired
    @Qualifier("emailService")
    public void setMessageService(MessageService messageService){
        this.messageService = messageService;
    }
}
```

## @Required

`@Required`是一个方法级注解，并应用于bean的setter方法。此注解仅只是必须将setter方法配置为在配置时使用值依赖注入。

> 例如，对setter方法的`@Required`标记了我们想要通过xml填充的依赖项：

```java
@Required
public void setColor(String color){
    this.color = color;
}
```

```xml
<bean class = "com.house.Car">
    <property name = "color" value = "black"></property>
</bean>
```

> 否则，将抛出`BeanInitializationException`。

## @Value

该注解用于为变量和方法参数指定默认值。我们可以使用@Value注解来读取Spring环境变量以及系统变量。该注解支持SpEL。

> 为类属性指定默认值

```java
@Value("default value")
private String value;
```

> 注解参数可以只有字符串，但Spring会尝试将其转换为指定的类型。

```java
@Value("true")
private boolean flag;
@Value("10")
private int num;
```

> Spring环境变量

```java
@Value("${APP_NAME}")
private String appName;
```

> 系统变量

```java
@Value("${java.home}")
private String javaHome;
@Value("${HOME}")
private String homeDir;
```

> SpEL表达式

```java
@Value("#{systemProperties['java.home']}")
private String javaHome;
```

## @DependsOn

当前bean所依赖的bean。指定的任何bean都保证在此bean之前由容器创建。在bean没有通过属性或构造函数参数显式依赖于另一个bean的情况下很少使用，而是依赖于另一个bean的初始化的副作用。

依赖声明可以指定初始化时间依赖性，并且仅在单例bean的情况下，指定相应的销毁时间依赖性。在给定的bean本身被销毁之前，首先销毁定义与给定bean的依赖关系的从属bean。因此，依赖声明也可以控制关闭顺序。

可以在任何直接或间接使用注释`Component`方法注释的类上使用`Bean`。

`DependsOn`除非正在使用组件扫描，否则在类级别使用无效。如果`DependsOn`通过XML声明了一个带`DependsOn`注释的类， 则会忽略注释元数据，`<bean depends-on="..."/>`而是应该遵守注释元数据 。

> 示例：让我们创建FirstBean和SecondBean类。在此示例中，SecondBean在bean之前初始化FirstBean。

```java
public class FirstBean{
    @Autowired
    private SecondBean secondBean;
}

public class SecondBean{
    public SecondBean(){
        System.out.println("SecondBean Initialized via Constructor");
    }
}
```

> 基于配置类在java中声明上述bean

```java
@Configuration
public class AppConfig{
    @Bean("firstBean")
    @DependsOn("secondBean")
    public FirstBean firstBean(){
        return new FirstBean();
    }
    
    @Bean("secondBean")
    public SecondBean secondBean(){
        return new SecondBean();
    }
}
```

## @Lazy

默认情况下，Spring IOC容器在应用程序启动时创建并初始化所有单例bean。我们可以使用该注解来防止单例bean的这种预初始化。

> 示例：有两个bean FirstBean和SecondBean。在此示例中，我们将FirstBean使用`@Lazy`注解显示加载

```java
public class FirstBean{
    public FirstBean(){
        System.out.println("Constructor of FirstBean Class");
    }
}

public class SecondBean{
    public SecondBean(){
        System.out.println("Constructor of SecondBean Class")
    }
}
```

> 基于配置类在java中声明上述bean

```java
@Configuration
public class AppConfig{
    @Lazy(true)
    @Bean
    public FirstBean firstBean(){
        return new FirstBean();
    }
    
    @Bean
    public SecondBean secondBean(){
        return new SecondBean();
    }
}
```

> 我们可以看到，SecondBean由Spirng IOC容器初始化，而FirstBean则被显示初始化。

## @Lookup

这是一个作用在方法上的注解，被其标注的方法会被重写，然后根据其返回值的类型，容器调用BeanFactory的getBean()方法来返回一个bean。

```java
@Component
public class ClassA {
  public void printClass() {
    System.out.println("This is Class A: " + this);
    getClassB().printClass();
  }
 
  @Lookup
  public ClassB getClassB() {
    return null;
  }
}

@Component
@Scope(value = SCOPE_PROTOTYPE)
public class ClassB {
  public void printClass() {
    System.out.println("This is Class B: " + this);
  }
}
```

## @Primary

当存在多个相同类型的bean时，使用该注解给bean更高的优先级

```java
@Component
@Primary
class Car implements Vehicle{}

@Component
class Bike implements vehicle{}

@Component
class Driver{
    @Autowired
    Vehicle vehicle;
}

@Component
class Biker{
    @Autowired
    @Qualifier("bike")
    Vehicle vehicle;
}
```

## @Scope

我们使用`@Scope`来定义`@Component`或`@Bean`类的范围。它可以是单例、原型、请求、会话、全局会话或某些自定义范围。

```java
@Component
@Scope(ConfigurableBeanFactory.SCOPE_SINGLETON)
public class TwitterMessageService implements MessageService{}

@Component
@Scope(ConfigurableBeanFactory.SCOPE_PROTOTYPE)
public class TwitterMessageService implements MessageService{}
```

## @Profile



如果我们希望Spring仅在特定配置文件处于活动状态时使用`@Component`类或`@Bean`方法，可以使用该注解标记。我们可以使用该注解的value属性配置配置文件的名称

```java
@Component
@Profile("sportDay")
class Bike implements Vehicle{}
```

## @Import

该注解表示一个或多个`@Configuration`类入口

> 例如，在基于java的配置中，Spring提供了`@Import`注解，允许从另一个配置类加载`@Bean`定义

```java
@Configuration
public class ConfigA{
    @Bean
    public A a(){
        return new A();
    }
}

@Configuration
@Import(ConfigA.class)
public class ConfigB{
    @Bean
    public B b(){
        return new B();
    }
}
```

> 现在，在实例化上下文时，不需要同时指定ConfigA类和ConfigB类，只需要显示提供ConfigB。

## @ImportResource

该注解用于将applicationContext.xml文件中的bean加载到Spring IOC容器中。

```java
@Configuration
@ImportResource("classpath*:applicationContext.xml")
public class XmlConfiguration{}
```

## @PropertySource

该注解提供了一种方便的声明性机制，用于添加PropertySource Spring的Environment以与@Configuration类一起使用。

> 例如，我们从文件config.properties文件中读取数据库配置，并使用Environment将这些属性配置设置为DataSourceConfig类。

```java
@Configuration
@PropertySource("classpath:config.properties")
public class PropertySourceDemo implements InitializingBean{
    @Autowired
    private Environment env;
    
    //这个方法将在所有的属性被初始化后调用。但是会在init前调用。
    //如果是延迟加载的话，则马上执行。
    //可以在类上加上注解：@Lazy(false),这样spring容器初始化的时候afterPropertiesSet就会被调用。
    //只需要实现InitializingBean接口就行。
    @Override
    public void afterPropertiesSet() throws Exception{
        setDatabaseConfig();
    }
    
    private void setDatabaseConfig(){
        DataSourceConfig config = new DataSourceConfig();
        config.setDriver(env.getProperty("jdbc.driver"));
        config.setUrl(env.getProperty("jdbc.url"));
        config.setUsername(env.getProperty("jdbc.username"));
        config.setPassword(env.getProperty("jdbc.password"));
    }
}
```

## @PropertySources

该注解可以指定多个`@PropertySource`配置

```java
@PropertySources({
    @PropertySource("classpath:config.properties"),
    @PropertySource("classpath:db.properties")
})
public class AppConfig{}
```





