---
layout: post
title: SpringBoot
categories: Java
description: SpringBoot
keywords: SpringBoot
---

# SpringBoot

## SpringBoot介绍
### SpringBoot 特点： 
- SpringBoot 设计目的是用来简化新 Spring 应用的初始搭建以及开发过程。 
- 嵌入的 Tomcat，无需部署 WAR 文件 
- SpringBoot 并不是对 Spring 功能上的增强，而是提供了一种快速使用 Spring 的方式。

### 构建SpringBoot项目

#### 项目构建
- maven构建
- 修改pom.xml,修改jdk版本(SpringBoot 1.5/2+ jdk 1.7/1.8)
    ```xml
    <properties>
        <java.version>1.7</java.version>
    </properties>
    ```
- 依赖注入(坐标)
    ```xml
    <dependencies>
        <!-- springBoot的启动器 -->
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-web</artifactId>
        </dependency>
    </dependencies>
    ```

#### SpringBoot 启动器。
所谓的 springBoot 启动器其实就是一些 jar 包的集合。SprigBoot 一共提供 44 启动器。 
- spring-boot-starter-web 支持全栈式的 web 开发，包括了 romcat 和 springMVC 等 jar 
- spring-boot-starter-jdbc 支持 spring 以 jdbc 方式操作数据库的 jar 包的集合 
- spring-boot-starter-redis 支持 redis 键值存储的数据库操作

#### SpringBoot入门
- 编写Controller
- 编写SpringBoot启动类
- 注意：启动器存放的位置。启动器可以和 controller 位于同一个包下，或者位于 controller 的上一级 包中，但是不能放到 controller 的平级以及子包下。
```java
@Controller
public class HelloWorld {
    @RequestMapping("/hello")
    @ResponseBody
    public Map<String, Object> showHelloWorld(){
        Map<String, Object> map = new HashMap<>();
        map.put("msg", "HelloWorld");
        return map;
    }
}

@SpringBootApplication
public class App {

    public static void main(String[] args) {
        SpringApplication.run(App.class, args);
    }
}
```

## SpringBoot整合Web
### 整合Servlet
- 注解方式
```java
@WebServlet(name="FirstServlet",urlPatterns="/first")
public class FirstServlet extends HttpServlet {
    @Override
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        System.out.println("FirstServlet............");
    }
}

SpringBootApplication
@ServletComponentScan //在springBoot启动时会扫描@WebServlet，并将该类实例化
public class App {
    public static void main(String[] args) {
        SpringApplication.run(App.class, args);
    }

}
```
- 方法完成组件注册
```java
public class SecondServlet extends HttpServlet {
    @Override
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        System.out.println("SecondServlet..........");
    }
}

@SpringBootApplication
public class App2 {
    public static void main(String[] args) {
        SpringApplication.run(App2.class, args);
    }
    @Bean
    public ServletRegistrationBean getServletRegistrationBean(){
        ServletRegistrationBean bean = new ServletRegistrationBean(new SecondServlet());
        bean.addUrlMappings("/second");
        return bean;
    }
}
```

### 整合Filter
- 注解方式
```java
//@WebFilter(filterName="FirstFilter",urlPatterns={"*.do","*.jsp"})
@WebFilter(filterName="FirstFilter",urlPatterns="/first")
public class FirstFilter implements Filter {
    @Override
    public void destroy() {
        // TODO Auto-generated method stub
    }
    @Override
    public void doFilter(ServletRequest arg0, ServletResponse arg1, FilterChain arg2)
            throws IOException, ServletException {
        System.out.println("进入Filter");
        arg2.doFilter(arg0, arg1);
        System.out.println("离开Filter");
    }
    @Override
    public void init(FilterConfig arg0) throws ServletException {
        // TODO Auto-generated method stub
    }
}
```
- 方法完成组件注册
```java
@SpringBootApplication
public class App2 {
    public static void main(String[] args) {
        SpringApplication.run(App2.class, args);
    } 
    /**
     * 注册Servlet
     * @return
     */
    @Bean
    public ServletRegistrationBean getServletRegistrationBean(){
        ServletRegistrationBean bean = new ServletRegistrationBean(new SecondServlet());
        bean.addUrlMappings("/second");
        return bean;
    }  
    /**
     * 注册Filter
     */
    @Bean
    public FilterRegistrationBean getFilterRegistrationBean(){
        FilterRegistrationBean bean = new FilterRegistrationBean(new SecondFilter());
        //bean.addUrlPatterns(new String[]{"*.do","*.jsp"});
        bean.addUrlPatterns("/second");
        return bean;
    }
}
```

### 整合Listener
- 注解方式
```java
@WebListener
public class FirstListener implements ServletContextListener {
    @Override
    public void contextDestroyed(ServletContextEvent arg0) {
        // TODO Auto-generated method stub
    }
    @Override
    public void contextInitialized(ServletContextEvent arg0) {
        System.out.println("Listener...init......");
    }
}
```
- 方法完成组件注册
```java
@SpringBootApplication
public class App2 {
    public static void main(String[] args) {
        SpringApplication.run(App2.class, args);
    }
    /**
     * 注册listener
     */
    @Bean
    public ServletListenerRegistrationBean<SecondListener> getServletListenerRegistrationBean(){
        ServletListenerRegistrationBean<SecondListener> bean= new ServletListenerRegistrationBean<SecondListener>(new SecondListener());
        return bean;
    }
}
```

### 访问静态资源
- SpringBoot 从 classpath/static 的目录(注意目录名称必须是 static)
- ServletContext 根目录下(在 src/main/webapp 目录名称必须要 webapp)

### 文件上传
- 编写Controller
- 编写启动类
- 设置上传文件大小的默认值
    - 需要添加一个 springBoot 的配置文件(classpath路径下)
    - 设置单个上传文件的大小 spring.http.multipart.maxFileSize=200MB
    - 设置一次请求上传文件的总容量 spring.http.multipart.maxRequestSize=200MB
```java
@RestController //表示该类下的方法的返回值会自动做json格式的转换
public class FileUploadController {
    /*
     * 处理文件上传
     */
    @RequestMapping("/fileUploadController")
    public Map<String, Object> fileUpload(MultipartFile filename)throws Exception{
        System.out.println(filename.getOriginalFilename());
        filename.transferTo(new File("e:/"+filename.getOriginalFilename()));
        Map<String, Object> map = new HashMap<>();
        map.put("msg", "ok");
        return map;
    }
}
```

## SpringBoot视图层技术
### JSP
- 创建项目
- 修改pom文件，添加依赖
- 创建 springBoot 的全局配置文件，application.properties
    - spring.mvc.view.prefix=/WEB-INF/jsp/ 
    - spring.mvc.view.suffix=.jsp

```xml
<!-- pom.xml -->
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
  <modelVersion>4.0.0</modelVersion>
  <parent>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-parent</artifactId>
    <version>1.5.10.RELEASE</version>
  </parent>
  <groupId>com.bjsxt</groupId>
  <artifactId>spring-boot-view-jsp</artifactId>
  <version>0.0.1-SNAPSHOT</version> 
  <!-- jdk1.7 -->
  <properties>
    <java.version>1.7</java.version>
  </properties>
  <dependencies>
  <!-- springBoot的启动器 -->
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-web</artifactId>
    </dependency>
        <!-- jstl -->
    <dependency>
        <groupId>javax.servlet</groupId>
        <artifactId>jstl</artifactId>
    </dependency>
    <!-- jasper -->
    <dependency>
        <groupId>org.apache.tomcat.embed</groupId>
        <artifactId>tomcat-embed-jasper</artifactId>
        <scope>provided</scope>
    </dependency>
</dependencies>
</project>
```
### freemarker
- 创建项目
- 修改 pom 添加坐标
- 编写视图
    - 注意：springBoot要求模板形式的视图层技术的文件必须要放到src/main/resources目录下必须要一个名称为 templates 
```xml
<!-- pom.xml -->
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
  <modelVersion>4.0.0</modelVersion>
  <parent>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-parent</artifactId>
    <version>1.5.10.RELEASE</version>
  </parent>
  <groupId>com.bjsxt</groupId>
  <artifactId>09-spring-boot-view-freemarker</artifactId>
  <version>0.0.1-SNAPSHOT</version>
  
  <properties>
    <java.version>1.7</java.version>
  </properties>
  
  <dependencies>
  <!-- springBoot的启动器 -->
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-web</artifactId>
    </dependency>
   <!-- freemarker启动器的坐标 -->
   <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-freemarker</artifactId>
    </dependency>
</dependencies>
</project>
```
```ftl
<html>
    <head>
        <title>展示用户数据</title>
        <meta charset="utf-8"></meta>
    </head>   
    <body>     
        <table border="1" align="center" width="50%">      
            <tr>          
                <th>ID</th>
                <th>Name</th>
                <th>Age</th>
            </tr>        
            <#list list as user >
                <tr>
                    <td>${user.userid}</td>
                    <td>${user.username}</td>
                    <td>${user.userage}</td>
                </tr>
            </#list>    
        </table>
    </body>
</html>
```

### Thymeleaf
#### Thymeleaf 的入门项目
- 创建项目
- 修改 pom 文件添加坐标
- 创建存放视图的目录
    - 目录位置：src/main/resources/templates templates：该目录是安全的。意味着该目录下的内容是不允许外界直接访问的。
```xml
<project xmlns="http://maven.apache.org/POM/4.0.0"
    xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
    xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>
    <parent>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-parent</artifactId>
        <version>1.5.10.RELEASE</version>
    </parent>
    <groupId>com.bjsxt</groupId>
    <artifactId>spring-boot-view-thymeleaf</artifactId>
    <version>0.0.1-SNAPSHOT</version>

    <properties>
        <java.version>1.7</java.version>
        <thymeleaf.version>3.0.2.RELEASE</thymeleaf.version>
        <thymeleaf-layout-dialect.version>2.0.4</thymeleaf-layout-dialect.version>
    </properties>

    <dependencies>
        <!-- springBoot的启动器 -->
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-web</artifactId>
        </dependency>
        <!-- springBoot的启动器 -->
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-thymeleaf</artifactId>
        </dependency>
    </dependencies>
</project>
```

#### Thymeleaf 的基本使用
- Thymeleaf特点：Thymelaef 是通过他特定语法对 html 的标记做渲染。
- 异常(元素类型"meta"必须由匹配的结束标记终止)
    - 解决1：让 html 的标记按照严禁的语法去编写。
    - 解决2：Thymeleaf.jar:更新为 3.0 以上的版本；thymeleaf-layout-dialect.jar：更新为 2.0 以上的版本

#### Thymeleaf 语法详解
- 变量输出
    - th:text 在页面中输出值
    - th:value 可以将一个值放入到 input 标签的 value 中
- 字符串操作
    - Thymeleaf 内置对象 
    - 注意语法： 
        - 调用内置对象一定要用# 
        - 大部分的内置对象都以 s 结尾 strings、numbers、dates 
- 字符串常用函数
    - ${#strings.isEmpty(key)} 判断字符串是否为空，如果为空返回 true，否则返回 false
    - ${#strings.contains(msg,'T')} 判断字符串是否包含指定的子串，如果包含返回 true，否则返回 false
    - ${#strings.startsWith(msg,'a')} 判断当前字符串是否以子串开头，如果是返回 true，否则返回 false
    - ${#strings.endsWith(msg,'a')} 判断当前字符串是否以子串结尾，如果是返回 true，否则返回 false
    - ${#strings.length(msg)} 返回字符串的长度
    - ${#strings.indexOf(msg,'h')} 查找子串的位置，并返回该子串的下标，如果没找到则返回-1
    - ${#strings.substring(msg,13)} 
    - ${#strings.substring(msg,13,15)} 截取子串，用户与 jdkString 类下 SubString 方法相同
    - ${#strings.toUpperCase(msg)} 
    - ${#strings.toLowerCase(msg)} 字符串转大小写。
- 日期格式化处理
    - ${#dates.format(key)} 格式化日期，默认的以浏览器默认语言为格式化标准
    - ${#dates.format(key,'yyy/MM/dd')}按照自定义的格式做日期转换
    - ${#dates.year(key)} year：取年
    - ${#dates.month(key)}  Month：取月 
    - ${#dates.day(key)} Day：取日
- 条件判断
    - th:if
    - th:switch
    ```html
    <span th:if="${sex} == '男'">
        性别：男
    </span>
    <span th:if="${sex} == '女'">
        性别：女
    </span>
    <hr/>
    <div th:switch="${id}">
        <span th:case="1">ID为1</span>
        <span th:case="2">ID为2</span>
        <span th:case="3">ID为3</span>
    </div>
    ```
- 迭代遍历
    - th:each
        - 状态变量属性(u是item var是状态变量)
            - index:当前迭代器的索引 从 0 开始 
            - count:当前迭代对象的计数 从 1 开始 
            - size:被迭代对象的长度 
            - even/odd:布尔值，当前循环是否是偶数/奇数
            - first:布尔值，当前循环的是否是第一条，如果是返回 true 否则返回 false 
            - last:布尔值，当前循环的是否是最后一条，如果是则返回 true 否则返回 false
    ```html
    <table border="1">
        <tr th:each="u,var : ${list}">
            <td th:text="${u.userid}"></td>
            <td th:text="${u.username}"></td>
            <td th:text="${u.userage}"></td>
            <td th:text="${var.index}"></td>
            <td th:text="${var.count}"></td>
            <td th:text="${var.size}"></td>
            <td th:text="${var.even}"></td>
            <td th:text="${var.odd}"></td>
            <td th:text="${var.first}"></td>
            <td th:text="${var.last}"></td>
        </tr>
    </table>
    ```
    - th:each 迭代 Map
    ```html
    <table border="1">
        <tr th:each="maps : ${map}">
            <td th:each="entry:${maps}" th:text="${entry.value.userid}" ></td>
            <td th:each="entry:${maps}" th:text="${entry.value.username}"></td>
            <td th:each="entry:${maps}" th:text="${entry.value.userage}"></td>
        </tr>
    </table>
    ```
- 域对象操作
    - HttpServletRequest
    - HttpSession
    - ServletContext
    ```html
<body>
    Request:<span th:text="${#httpServletRequest.getAttribute('req')}"></span><br/>
    Session:<span th:text="${session.sess}"></span><br/>
    Application:<span th:text="${application.app}"></span>
</body>
    ```
- URL 表达式
    - th:href 
    - th:src
    - url 表达式语法 基本语法：@{}
    - URL 类型
        - 绝对路径 <a th:href="@{http://www.baidu.com}">绝对路径</a><br/>
        - 相对路径
            - 相对于当前项目的根,相对于项目的上下文的相对路径 <a th:href="@{/show}">相对路径</a>
            - 相对于服务器路径的根 <a th:href="@{~/project2/resourcename}">相对于服务器的根</a>
    - 在 url 中实现参数传递 <a th:href="@{/show(id=1,name=zhagnsan)}">相对路径-传参</a>
    - 在 url 中通过 restful 风格进行参数传递 <a th:href="@{/path/{id}/show(id=1,name=zhagnsan)}">相对路径-传参-restful</a>

## SpringBoot整合持久层
通过使用 SpringBoot+SpringMVC+MyBatis 整合实现一 个对数据库中的 users 表的 CRUD 的操作

### 项目配置
- 修改 pom 文件
- 添加 application.properties 全局配置文件
```xml
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
    xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>
    <parent>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-parent</artifactId>
        <version>1.5.10.RELEASE</version>
    </parent>
    <groupId>com.bjsxt</groupId>
    <artifactId>12-spring-boot-springmvc-mybatis</artifactId>
    <version>0.0.1-SNAPSHOT</version>

    <properties>
        <java.version>1.7</java.version>
        <thymeleaf.version>3.0.2.RELEASE</thymeleaf.version>
        <thymeleaf-layout-dialect.version>2.0.4</thymeleaf-layout-dialect.version>
    </properties>

    <dependencies>
        <!-- springBoot的启动器 -->
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-web</artifactId>
        </dependency>
        <!-- web启动器 -->
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-thymeleaf</artifactId>
        </dependency>
        <!-- Mybatis启动器 -->
        <dependency>
            <groupId>org.mybatis.spring.boot</groupId>
            <artifactId>mybatis-spring-boot-starter</artifactId>
            <version>1.1.1</version>
        </dependency>
        <!-- mysql数据库驱动 -->
        <dependency>
            <groupId>mysql</groupId>
            <artifactId>mysql-connector-java</artifactId>
        </dependency>
        <!-- druid数据库连接池 -->
        <dependency>
            <groupId>com.alibaba</groupId>
            <artifactId>druid</artifactId>
            <version>1.0.9</version>
        </dependency>
    </dependencies>
</project>
```
```
spring.datasource.driverClassName=com.mysql.jdbc.Driver
spring.datasource.url=jdbc:mysql://localhost:3306/ssm
spring.datasource.username=root
spring.datasource.password=1234

spring.datasource.type=com.alibaba.druid.pool.DruidDataSource

mybatis.type-aliases-package=com.bjsxt.pojo
```
- 启动类
```java
@SpringBootApplication
@MapperScan("com.bjsxt.mapper") //@MapperScan 用户扫描MyBatis的Mapper接口
public class App {

    public static void main(String[] args) {
        SpringApplication.run(App.class, args);
    }
}

```

# SpringBoot高级

## 服务端表单数据校验

SpringBoot 对表单数据校验的技术特点
- SpringBoot 中使用了 Hibernate-validate 校验框架

SpringBoot 表单数据校验步骤
- 在实体类中添加校验规则
```java
public class Users {
    @NotBlank(message="用户名不能为空") //非空校验
    @Length(min=2,max=6,message="最小长度为2位，最大长度为6位")
    private String name;
    @NotEmpty
    private String password;
    @Min(value=15)
    private Integer age;
    @Email
    private String email;
}
```
- 在 Controller 中开启校验
- 在页面中获取提示信息
```html
    <form th:action="@{/save}" method="post">
        用户姓名：<input type="text" name="name"/><font color="red" th:errors="${aa.name}"></font><br/>
        用户密码：<input type="password" name="password" /><font color="red" th:errors="${aa.password}"></font><br/>
        用户年龄：<input type="text" name="age" /><font color="red" th:errors="${aa.age}"></font><br/>
        用户邮箱：<input type="text" name="email" /><font color="red" th:errors="${aa.email}"></font><br/>
        <input type="submit" value="OK"/>
    </form>
```

-  解决数据校验时的异常问题
    - 解决异常的方法，在跳转页面的方法中注入一个对象，来解决问题。要求参数对象的 变量名必须是对象的类名的全称首字母小写
    ```java
    /**
     * 解决异常的方式。可以在跳转页面的方法中注入一个 Uesrs 对象。 
     * 注意：由于 springmvc 会将该对象放入到 Model 中传递。key 的名称会使用 该对象的驼峰式的命名规则来作为 key。
     * 参数的变量名需要与对象的名称相同。将首字母小写。  
     * @param users 
     * @return 
    */ 
    @RequestMapping("/addUser") 
    public String showPage( Users users){ 
        return "add"; 
    }

    /**
     * 完成用户添加
     *@Valid 开启对Users对象的数据校验
     *BindingResult:封装了校验的结果
     */
    @RequestMapping("/save")
    public String saveUser(@ModelAttribute("aa") @Valid Users users,BindingResult result){
        if(result.hasErrors()){
            return "add";
        }
        System.out.println(users);
        return "ok";
    }
    ```
- 如果参数的名称需要做改变
```java
@Controller
public class UsersController {
    /**
     * 如果想为传递的对象更改名称，可以使用@ModelAttribute("aa")这表示当前传递的对象的key为aa。
     * 那么我们在页面中获取该对象的key也需要修改为aa
     * @param users
     * @return
     */
    @RequestMapping("/addUser")
    public String showPage(@ModelAttribute("aa") Users users){
        return "add";
    }
    
    /**
     * 完成用户添加
     *@Valid 开启对Users对象的数据校验
     *BindingResult:封装了校验的结果
     */
    @RequestMapping("/save")
    public String saveUser(@ModelAttribute("aa") @Valid Users users,BindingResult result){
        if(result.hasErrors()){
            return "add";
        }
        System.out.println(users);
        return "ok";
    }   
}
```
- 其他校验规则
    - @NotBlank: 判断字符串是否为 null 或者是空串(去掉首尾空格)。
    - @NotEmpty: 判断字符串是否 null 或者是空串。 
    - @Length: 判断字符的长度(最大或者最小)
    - @Min: 判断数值最小值
    - @Max: 判断数值最大值 
    - @Email: 判断邮箱是否合法

## 异常处理

### 自定义错误页面
- SpringBoot 默认的处理异常的机制： SpringBoot 默认的已经提供了一套处理异常的机制。 一旦程序中出现了异常SpringBoot会像/error的url发送请求。在springBoot中提供了一个叫BasicExceptionController来处理/error请求，然后跳转到默认显示异常的页面来展示异常信息。
- 如果我们需要将所有的异常同一跳转到自定义的错误页面，需要再src/main/resources/templates目录下创建error.html页面。注意：名称必须叫 error 


### @ExceptionHandle 注解处理异常
```java
    /**
     * java.lang.ArithmeticException
     * 该方法需要返回一个ModelAndView：目的是可以让我们封装异常信息以及视图的指定
     * 参数Exception e:会将产生异常对象注入到方法中
     */
    @ExceptionHandler(value={java.lang.ArithmeticException.class})
    public ModelAndView arithmeticExceptionHandler(Exception e){
        ModelAndView mv = new ModelAndView();
        mv.addObject("error", e.toString());
        mv.setViewName("error1");
        return mv;
    }

```

### @ControllerAdvice+@ExceptionHandler 注解处理异常
- 需要创建一个能够处理异常的全局异常类。在该类上需要添加@ControllerAdvice 注解
```java
@ControllerAdvice
public class GlobalException {
    /**
     * java.lang.ArithmeticException
     * 该方法需要返回一个ModelAndView：目的是可以让我们封装异常信息以及视图的指定
     * 参数Exception e:会将产生异常对象注入到方法中
     */
    @ExceptionHandler(value={java.lang.ArithmeticException.class})
    public ModelAndView arithmeticExceptionHandler(Exception e){
        ModelAndView mv = new ModelAndView();
        mv.addObject("error", e.toString());
        mv.setViewName("error1");
        return mv;
    }
}
```

### 配置 SimpleMappingExceptionResolver 处理异常
- 在全局异常类中添加一个方法完成异常的同一处理
- 跳转页面无法传输参数
```java
@Configuration
public class GlobalException {
    /**
     * 该方法必须要有返回值。返回值类型必须是：SimpleMappingExceptionResolver
     */
    @Bean
    public SimpleMappingExceptionResolver getSimpleMappingExceptionResolver(){
        SimpleMappingExceptionResolver resolver = new SimpleMappingExceptionResolver();
        Properties mappings = new Properties();
        /**
         * 参数一：异常的类型，注意必须是异常类型的全名
         * 参数二：视图名称
         */
        mappings.put("java.lang.ArithmeticException", "error1");
        mappings.put("java.lang.NullPointerException","error2");
        
        //设置异常与视图映射信息的
        resolver.setExceptionMappings(mappings);
        return resolver;
    }
}
```

### 自定义 HandlerExceptionResolver 类处理异常
- 需要再全局异常处理类中实现HandlerExceptionResolver 接口
```java
@Configuration
public class GlobalException implements HandlerExceptionResolver {

    @Override
    public ModelAndView resolveException(HttpServletRequest request, HttpServletResponse response, Object handler, Exception ex) {
        ModelAndView mv = new ModelAndView();
        //判断不同异常类型，做不同视图跳转
        if(ex instanceof ArithmeticException){
            mv.setViewName("error1");
        }
        
        if(ex instanceof NullPointerException){
            mv.setViewName("error2");
        }
        mv.addObject("error", ex.toString());
        
        return mv;
    }
}
```

##  SpringBoot 整合 Junit 单元测试
- 创建项目，修改pom.xml，添加 junit 环境的 jar 包 
```xml
    <dependency>
        <groupId>org.springframework.boot</groupId>
       <artifactId>spring-boot-starter-test</artifactId>
    </dependency>
```
- 编写业务代码
    - DAO
    - 业务层
    - 启动类
- 使用 SpringBoot 整合 Junit 做单元测试
    - 编写测试类
    ```java
/**
 * SpringBoot测试类
 *@RunWith:启动器 
 *SpringJUnit4ClassRunner.class：让junit与spring环境进行整合
 *
 *@SpringBootTest(classes={App.class}) 1,当前类为springBoot的测试类
 *@SpringBootTest(classes={App.class}) 2,加载SpringBoot启动类。启动springBoot
 *
 *junit与spring整合 @Contextconfiguartion("classpath:applicationContext.xml")
 */
@RunWith(SpringJUnit4ClassRunner.class) 
@SpringBootTest(classes={App.class})
public class UserServiceTest {
    @Autowired
    private UserServiceImpl userServiceImpl;
    @Test
    public void testAddUser(){
        this.userServiceImpl.addUser();
    }
}

    ```

## SpringBoot热部署
### SpringLoader 插件
- 方式一：以 maven 插件方式使用 SpringLoader
    - 在 pom 文件中添加插件配置
    - 使用 maven 的命令起来启动 spring-boot:run
    - 注意：这种方式的缺点是 Springloader 热部署程序是在系统后台以进程的形式来运行。需要手动关闭该进程
    ```xml
    <!-- springloader插件 -->
    <build>
        <plugins>
            <plugin>
                <groupId>org.springframework.boot</groupId>
                <artifactId>spring-boot-maven-plugin</artifactId>
                <dependencies>
                    <dependency>
                        <groupId>org.springframework</groupId>
                        <artifactId>springloaded</artifactId>
                        <version>1.2.5.RELEASE</version>
                    </dependency>
                </dependencies>
            </plugin>
        </plugins>
    </build>
    ```
- 方式二：在项目中直接使用 jar 包的方式
    - 添加 springloader 的 jar 包
    - 启动方式
        - Run Configuration
        - 启动命令： -javaagent:.\lib\springloaded-1.2.5.RELEASE.jar -noverify
- SpringLoader 缺陷：就是 Java 代码做部署处理。但是对页面无能为力。

### DevTools 工具
-  SpringLoader 与 DevTools 的区别：
    - SpringLoader 在部署项目时使用的是热部署的方式。 
    - DevTools 在部署项目时使用的是重新部署的方式
- 修改项目的 pom 文件添加 devtools 的依赖
    ```xml
    <!-- DevTools的坐标 -->
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-devtools</artifactId>
        <optional>true</optional>
    </dependency>
    ```

##  SpringBoot 缓存技术
###  SpringBoot 整合 Ehcache




###  SpringBoot 整合 SpringDataRedis








