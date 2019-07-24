---
layout: post
title: Spring Boot 注解详解
category: springboot
tags: [springboot]
keywords: Spring Boot

---

## Spring Boot 注解详解

### @SpringBootApplication：
申明让spring boot自动给程序进行必要的配置，这个配置等同于：**@Configuration** ，**@EnableAutoConfiguration** 和 **@ComponentScan** 三个配置。
示例代码：

``` java
@SpringBootApplication
public class SpringBootWebApplication {
	
	public static void main(String[] args) {
		SpringApplication.run(SpringBootWebApplication.class, args);
	}
	
}
```

### @RequestMapping：提供路由信息，负责URL到Controller中的具体函数的映射。

示例代码：

``` java
// 页面跳转
@RequestMapping("/users")
public String pageadddatasource() {

	return "userlist";
	
}
```

### @ResponseBody：

表示该方法的返回结果直接写入HTTP response body中，一般在异步获取数据时使用，用于构建RESTful的api。在使用 **@RequestMapping**后，返回值通常解析为跳转路径，加上 **@Responsebody**后返回结果不会被解析为跳转路径，而是直接写入HTTP response body中。比如异步获取json数据，加上 **@Responsebody**后，会直接返回json数据。该注解一般会配合 **@RequestMapping**一起使用。

示例代码：

``` java
@RequestMapping(value = "/getdatasource", method = RequestMethod.POST)
@ResponseBody
public PageInfo dataGrid(int page, int rows, String sort, String order,String inputcode, String classid) {

    PageInfo pageInfo = new PageInfo(page, rows, sort, order);
    Map<String, Object> condition = new HashMap<String, Object>();
    condition.put("inputcode", inputcode);
    condition.put("classid", classid);
    pageInfo.setCondition(condition);
    adddataSetService.findDataGrid(pageInfo);
    return pageInfo;
    
}
```

### @Controller：

用于定义控制器类，在spring项目中由控制器负责将用户发来的URL请求转发到对应的服务接口（service层），一般这个注解在类中，通常方法需要配合注解 **@RequestMapping**。示例代码：

示例代码：

``` java
@Controller
@RequestMapping("/user")
public class UserController {
}
```

### @RestController：

用于标注控制层组件(如struts中的action)，**@ResponseBody**和 **@Controller**的合集。示例代码：

示例代码：

``` java
@RestController
public class UserController {
}
```

### @EnableAutoConfiguration：

SpringBoot自动配置（auto-configuration）：尝试根据你添加的jar依赖自动配置你的Spring应用。例如，如果你的classpath下存在HSQLDB，并且你没有手动配置任何数据库连接beans，那么我们将自动配置一个内存型（in-memory）数据库”。你可以将 **@EnableAutoConfiguration**或者 **@SpringBootApplication**注解添加到一个 **@Configuration**类上来选择自动配置。如果发现应用了你不想要的特定自动配置类，你可以使用 **@EnableAutoConfiguration**注解的排除属性来禁用它们。

### @ComponentScan：

表示将该类自动发现扫描组件。个人理解相当于，如果扫描到有 **@Component**、**@Controller**、**@Service**等这些注解的类，并注册为Bean，可以自动收集所有的Spring组件，包括 **@Configuration**类。我们经常使用 **@ComponentScan**注解搜索beans，并结合@Autowired注解导入。可以自动收集所有的Spring组件，包括 **@Configuration **类。如果没有配置的话，Spring Boot会扫描启动类所在包下以及子包下的使用了 **@Service**,**@Repository**等注解的类。

### @Configuration：

相当于传统的xml配置文件，如果有些第三方库需要用到xml文件，建议仍然通过 **@Configuration**类作为项目的配置主类——可以使用 **@ImportResource** 注解加载xml配置文件。

### @Import：

用来导入其他配置类。

### @ImportResource：

用来加载xml配置文件。

### @Autowired：

自动导入依赖的bean。byType方式。把配置好的Bean拿来用，完成属性、方法的组装，它可以对类成员变量、方法及构造函数进行标注，完成自动装配的工作。当加上（required=false）时，就算找不到bean也不报错。

示例代码：

``` java
@Autowired
AddDataSetService adddataSetService;
```

### @Service：

一般用于修饰service层的组件

``` java
@Service
public class AddDataSetServiceImpl implements AddDataSetService {

	private static Logger LOGGER = LoggerFactory.getLogger(AddDataSetServiceImpl.class);

	/*省略代码*/
	
}
```
### @Repository：

使用 **@Repository**注解可以确保DAO或者repositories提供异常转译，这个注解修饰的DAO或者repositories类会被ComponetScan发现并配置，同时也不需要为它们提供XML配置项。

### @Bean：

用@Bean标注方法等价于XML中配置的bean。

### @Value：

注入Spring boot application.properties配置的属性的值。

示例代码：

``` java
@Value("${mail.fromMail.addr}")
private String from;
```

### @Inject：

等价于默认的@Autowired，只是没有required属性；

### @Component：

泛指组件，当组件不好归类的时候，我们可以使用这个注解进行标注。

### @Bean:

相当于XML中的,放在方法的上面，而不是类，意思是产生一个bean,并交给spring管理。

### @Qualifier：

当有多个同一类型的Bean时，可以用@Qualifier(“name”)来指定。与@Autowired配合使用。@Qualifier限定描述符除了能根据名字进行注入，但能进行更细粒度的控制如何选择候选者，具体使用方式如下：

### @Resource

**@Resource(name=”name”,type=”type”)：**

没有括号内内容的话，默认byName。与@Autowired干类似的事。

sadfaf