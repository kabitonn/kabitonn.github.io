---
layout: post
title: SpringMVC
categories: Java
description: SpringMVC
keywords: SpringMVC
---


# SpringMVC简介

SpringMVC 中重要组件
- DispatcherServlet: 前端控制器,接收所有请求(如果配置/不包含jsp)
- HandlerMapping: 解析请求格式的.判断希望要执行哪个具体的方法
- HandlerAdapter: 负责调用具体的方法
- ViewResovler:视图解析器.解析结果,准备跳转到具体的物理视图
- 运行原理：DispatcherServlet -> HandlerMapping -> HandlerAdapter -> Controller -> ViewResovler

Spring 容器和 SpringMVC 容器的关系
```java
    /**
     * Initialize and publish the WebApplicationContext for this servlet.
     * <p>Delegates to {@link #createWebApplicationContext} for actual creation
     * of the context. Can be overridden in subclasses.
     * @return the WebApplicationContext instance
     * @see #FrameworkServlet(WebApplicationContext)
     * @see #setContextClass
     * @see #setContextConfigLocation
     */
    protected WebApplicationContext initWebApplicationContext() {
        WebApplicationContext rootContext =
                WebApplicationContextUtils.getWebApplicationContext(getServletContext());
        WebApplicationContext wac = null;

        if (this.webApplicationContext != null) {
            // A context instance was injected at construction time -> use it
            wac = this.webApplicationContext;
            if (wac instanceof ConfigurableWebApplicationContext) {
                ConfigurableWebApplicationContext cwac = (ConfigurableWebApplicationContext) wac;
                if (!cwac.isActive()) {
                    // The context has not yet been refreshed -> provide services such as
                    // setting the parent context, setting the application context id, etc
                    if (cwac.getParent() == null) {
                        // The context instance was injected without an explicit parent -> set
                        // the root application context (if any; may be null) as the parent
                        cwac.setParent(rootContext);
                    }
                    configureAndRefreshWebApplicationContext(cwac);
                }
            }
        }
```
- Spring 容器和 SpringMVC 容器是父子容器
    - SpringMVC 容器中能够调用 Spring 容器的所有内容


SpringMVC 环境搭建注解方式
- 导入 jar
- 在 web.xml 中配置前端控制器 DispatcherServlet
    - 如果不配置 `<init-param>` 会在/WEB-INF/`<servlet-name>`-servlet.xml 
- 在 src 下新建 springmvc.xml
    - 引入 xmlns:mvc 命名空间
- 编写控制器类

	```xml
	<!-- web.xml -->
	<?xml version="1.0" encoding="UTF-8"?>
	<web-app version="3.0" xmlns="http://java.sun.com/xml/ns/javaee"
		xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
		xsi:schemaLocation="http://java.sun.com/xml/ns/javaee                       
		http://java.sun.com/xml/ns/javaee/web-app_3_0.xsd">

		<servlet>
			<servlet-name>springmvc</servlet-name>
			<servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>
			<!-- 修改配置文件路径和名称 -->
			<init-param>
				<param-name>contextConfigLocation</param-name>
				<param-value>classpath:springmvc.xml</param-value>
			</init-param>
			<!-- 自启动 -->
			<load-on-startup>1</load-on-startup>
		</servlet>
		<servlet-mapping>
			<servlet-name>springmvc</servlet-name>
			<url-pattern>/</url-pattern>
		</servlet-mapping>
	</web-app>

	<!-- springmvc.xml -->
	<?xml version="1.0" encoding="UTF-8"?>
	<beans xmlns="http://www.springframework.org/schema/beans"
		xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" 
		xmlns:mvc="http://www.springframework.org/schema/mvc"
		xmlns:context="http://www.springframework.org/schema/context"
		xsi:schemaLocation="http://www.springframework.org/schema/beans
			http://www.springframework.org/schema/beans/spring-beans.xsd
			http://www.springframework.org/schema/context
			http://www.springframework.org/schema/context/spring-context.xsd
			http://www.springframework.org/schema/mvc
			http://www.springframework.org/schema/mvc/spring-mvc.xsd">
		<!-- 扫描注解 -->
		<context:component-scan base-package="com.bjsxt.controller"></context:component-scan>
		<!-- 注解驱动 -->
		<!-- org.springframework.web.servlet.mvc.annotation.DefaultAnnotationHandlerMapping -->
		<!-- org.springframework.web.servlet.mvc.annotation.AnnotationMethodHandlerAdapter -->
		<mvc:annotation-driven></mvc:annotation-driven>
		<!-- 静态资源 -->
		<mvc:resources location="/js/" mapping="/js/**"></mvc:resources>
		<mvc:resources location="/css/" mapping="/css/**"></mvc:resources>
		<mvc:resources location="/images/" mapping="/images/**"></mvc:resources>
	</beans>
	```

字符编码过滤器
- 在 web.xml 中配置 Filter
	```xml
		<!-- 字符编码过滤器 -->
		<filter>
			<filter-name>encoding</filter-name>
			<filter-class>org.springframework.web.filter.CharacterEncodingFilter</filter-class>
			<init-param>
				<param-name>encoding</param-name>
				<param-value>utf-8</param-value>
			</init-param>
		</filter>
		<filter-mapping>
			<filter-name>encoding</filter-name>
			<url-pattern>/*</url-pattern>
		</filter-mapping>
	```

传参
- 把内容写到方法(HandlerMethod)参数中,SpringMVC 只要有这个内容,注入内容.
- 基本数据类型参数
    - 默认保证参数名称和请求中传递的参数名相同
    - 如果请求参数名和方法参数名不对应使用@RequestParam(value="")赋值 
    - 如果方法参数是基本数据类型(不是封装类)可以通过@RequestParam(defaultValue="") 设置默认值，防止没有参数时500 
    - 如果强制要求必须有某个参数 @RequestParam(required=true)
- HandlerMethod 中参数是对象类型
    - 请求参数名和对象中属性名对应(get/set 方法) 
- 请求参数中包含多个同名参数的获取方式
    - 复选框传递的参数就是多个同名参数 @RequestParam("name")List<String> list
- 请求参数中对象.属性格式
    - jsp代码 `<input type="text" name="peo.name"/> <input type="text" name="peo.age"/>`
    - 新建一个类，对象名和参数中点前面名称对应 public class Demo { private People peo;}
    - 控制器 @RequestMapping("demo6") public String demo6(Demo demo){}
- 在请求参数中传递集合对象类型参数
    - jsp 中格式 
    ```jsp
    <input type="text" name="peo[0].name"/> 
    <input type="text" name="peo[0].age"/> 
    <input type="text" name="peo[1].name"/> 
    <input type="text" name="peo[1].age"/>
    ```
    - public class Demo { private List<People> peo;}
    - 控制器
- restful 传值方式
    - 简化 jsp 中参数编写格式
    - 在 jsp 中设定特定的格式 `<a href="demo8/123/abc">`跳转</a>
    - 控制器中
        - 在@RequestMapping 中一定要和请求格式对应
        - {名称} 中名称自定义名称
        - @PathVariable 获取@RequestMapping 中内容,默认按照方法参数名称去寻找
       
	   ```java
        @RequestMapping("demo8/{age1}/{name}") 
        public String demo8(@PathVariable String name, @PathVariable("age1") int age){ 
            System.out.println(name +" "+age); 
        }
        ```

跳转方式
- 默认跳转方式请求转发.
- 设置返回值字符串内容
    - 添加 redirect:资源路径 重定向
    - 添加 forward:资源路径 或省略 forward: 转发

视图解析器
- SpringMVC 会提供默认视图解析器
- 程序员自定义视图解析器
	
	```xml
		<bean id="viewResolver" class="org.springframework.web.servlet.view.InternalResourceViewResolver">
			<property name="prefix" value="/"></property>
			<property name="suffix" value=".jsp"></property>
		</bean>
	```
- 如果希望不执行自定义视图解析器,在方法返回值前面添加forward:或 redirect:


@ResponseBody
- 在方法上只有@RequestMapping 时,无论方法返回值是什么认为需要跳转
- 在方法上添加@ResponseBody(恒不跳转)
    - 如果返回值满足 key-value 形式(对象或 map)
        - 把响应头设置为 application/json;charset=utf-8
        - 把转换后的内容输出流的形式响应给客户端.
    - 如果返回值不满足 key-value,例如返回值为 String
        - 把响应头设置为 text/html
        - 把方法返回值以流的形式直接输出
        - 如果返回值包含中文,出现中文乱码
            - produces 表示响应头中 Content-Type 取值 @RequestMapping(produces="text/html; charset=utf-8")
- 底层使用Jackson进行json转换,在项目中一定要导入jackson的jar,spring4.1.6 对 jackson 不支持较高版本,jackson2.7 无效.


JSP九大内置对象四大作用域
- 九大内置对象

名称|类型|含义|获取方式
|-|-|-|-|
request    |HttpSevletRequest   |封装所有请求信息|方法参数
response   |HttpServletResponse |封装所有响应信息|方法参数
session    |HttpSession         |封装所有会话信息|req.getSession()
application|ServletContext      |所有信息       |getServletContext();request.getServletContext();
out        |PrintWriter         |输出对象       |response.getWriter()
exception  |Exception           |异常对象       |
page       |Object              |当前页面对象   |
pageContext|PageContext         |获取其他对象    |
config     |ServletConfig       |配置信息       |

- 四大作用域
- page 在当前页面不会重新实例化.
- request 在一次请求中同一个对象,下次请求重新实例化一个request 对象.
- session
    - 一次会话.
    - 只要客户端Cookie中传递的Jsessionid不变,Session不会重新实例化(不超过默认时间.)
    - 实际有效时间:
        - 浏览器关闭 Cookie 失效.
        - 默认时间 在时间范围内无任何交互.在 tomcat 的web.xml 中配置`<session-config> <session-timeout>30</session-timeout> </session-config>`
- application 只有在 tomcat 启动项目时菜实例化.关闭 tomcat 时销毁application

SpringMVC 作用域传值的几种方式
- 使用原生 Servlet
    - 在 HanlderMethod 参数中添加作用域对象
- 使用 Map 集合
    - 把 map 中内容放在 request 作用域中
    - spring 会对 map 集合通过 BindingAwareModelMap 进行实例化
- 使用 SpringMVC 中 Model 接口 
    - 把内容最终放入到 request 作用域中
- 使用 SpringMVC 中 ModelAndView 类 

```java
@RequestMapping("demo2") 
public String demo2(Map<String,Object> map){ 
    System.out.println(map.getClass());
    map.put("map","map 的值"); 
    return "/index.jsp";
}

@RequestMapping("demo3") 
public String demo3(Model model){
    model.addAttribute("model", "model 的值"); return "/index.jsp";
}
@RequestMapping("demo3") 
public ModelAndView demo4(){
    //参数,跳转视图 
    ModelAndView mav = new ModelAndView("/index.jsp");
    mav.addObject("mav", "mav 的值"); return mav;
}
```


文件下载
- 访问资源时相应头如果没有设置 Content-Disposition,浏览器默认按照 inline 值进行处理
    - inline 能显示就显示,不能显示就下载.
- 只需要修改相应头中 Context-Disposition="attachment;filename=文件名"
    - attachment 下载,以附件形式下载.
    - filename=值就是下载时显示的下载文件名
- 实现步骤
    - 导入 apache的两个 jar commons-fileupload.jar commons-io.jar
    - 在 jsp 中添加超链接,设置要下载文件
        - 在 springmvc 中放行静态资源 files 文件夹
    - 编写控制器方法


文件上传
- 基于 apache 的 commons-fileupload.jar 完成文件上传.
- MultipartResovler 作用:
    - 把客户端上传的文件流转换成 MutipartFile 封装类.
    - 通过 MutipartFile 封装类获取到文件流
- 表单数据类型分类,在`<form>`的 enctype 属性控制表单类型
    - 默认值 application/x-www-form-urlencoded,普通表单数据.(少量文字信息)
    - text/plain 大文字量时使用的类型.邮件,论文
    - multipart/form-data 表单中包含二进制文件内容.
- 实现步骤:
    - 导入 springmvc 包和 apache 文件上传 commons-fileupload 和 commons-io 两个 jar
    - 编写 JSP 页面 
    - 配置 springmvc.xml  MultipartResovler解析器,异常解析器
    - 编写控制器类
        - MultipartFile 对象名必须和`<input type="file"/>`的 name 属性值相同
    
	```xml
    <!-- MultipartResovler 解析器 -->
    <bean id="multipartResolver" class="org.springframework.web.multipart.commons.CommonsMultipartResolver">
        <property name="maxUploadSize" value="500"></property>
    </bean>
    <!-- 异常解析器 -->
    <bean id="exceptionResolver"
    class="org.springframework.web.servlet.handler.SimpleMappingExceptionResolver">
    <property name="exceptionMappings">
        <props> 
            <prop key="org.springframework.web.multipart.MaxUploadSizeExceededException">/error.jsp</prop>  
        </props>
    </property>
    </bean>
    ```
	
	```java
	@Controller
	public class DemoController {
		@RequestMapping("upload")
		public String upload(MultipartFile file,String name) throws IOException{
			String fileName = file.getOriginalFilename();
			String suffix = fileName.substring(fileName.lastIndexOf("."));
			//判断上传文件类型 if(suffix.equalsIgnoreCase(".png"))

			String uuid = UUID.randomUUID().toString();
			FileUtils.copyInputStreamToFile(file.getInputStream(), new File("E:/"+uuid+suffix));
			return "/index.jsp";
		}
	}
	```

自定义拦截器
- 跟过滤器比较像的技术.
- 发送请求时被拦截器拦截,在控制器的前后添加额外功能.
    - 跟 AOP 区分开AOP 在特定方法前后扩充(对 ServiceImpl)
    - 拦截器,请求的拦截.针对点是控制器方法.(对 Controller)
- SpringMVC 拦截器和 Filter 的区别
    - 拦截器只能拦截器 Controller
    - Filter 可以拦截任何请求.
- 实现自定义拦截器的步骤:
    - 新建类实现 HandlerInterceptor

	```java
	public class DemoInterceptor implements HandlerInterceptor {
		//在进入控制器之前执行
		//如果返回值为false,阻止进入控制器
		//控制代码
		@Override
		public boolean preHandle(HttpServletRequest arg0, HttpServletResponse arg1, Object arg2) throws Exception {
			System.out.println("arg2:"+arg2);
			System.out.println("preHandle");
			return true;
		}
		//控制器执行完成,进入到jsp之前执行.
		//日志记录.
		//敏感词语过滤
		@Override
		public void postHandle(HttpServletRequest arg0, HttpServletResponse arg1, Object arg2, ModelAndView arg3)
				throws Exception {

			System.out.println("往"+arg3.getViewName()+"跳转");
			System.out.println("model的值"+arg3.getModel().get("model"));
			String word = arg3.getModel().get("model").toString();
			String newWord = word.replace("祖国", "**");
			arg3.getModel().put("model", newWord);
	//      arg3.getModel().put("model", "修改后的内容");
			System.out.println("postHandle");
		}
		//jsp执行完成后执行
		//记录执行过程中出现的异常.
		//可以把异常记录到日志中
		@Override
		public void afterCompletion(HttpServletRequest arg0, HttpServletResponse arg1, Object arg2, Exception arg3)
				throws Exception {
			System.out.println("arg3:"+arg3);
			System.out.println("afterCompletion"+arg3.getMessage());
		}
	}
	```

- 在 springmvc.xml 配置拦截器需要拦截哪些控制器
	- 拦截所有控制器 
	- 拦截特定的的url 

		```xml
        <mvc:interceptors> 
            <bean class="com.bjsxt.interceptor.DemoInterceptor"></bean>
        </mvc:interceptors>
        <mvc:interceptors> 
            <mvc:interceptor> 
                <mvc:mapping path="/demo"/> <mvc:mapping path="/demo1"/> <mvc:mapping path="/demo2"/> 
                <bean class="com.bjsxt.interceptor.DemoInterceptor"></bean> 
            </mvc:interceptor> 
        </mvc:interceptors>
		```


拦截器栈
- 多个拦截器同时生效时,组成了拦截器栈
- 顺序:先进后出.
- 执行顺序和在 springmvc.xml 中配置顺序有关
- 设置先配置拦截器 A 在配置拦截器 B 执行顺序为 preHandle(A)--> preHandle(B)--> 控制器方法 -->postHandle(B)
-->postHanle(A)-->JSP-->afterCompletion(B)-->afterCompletion(A)


使用springmvc拦截器实现登陆验证
- 把页面放入到web-inf中.
    - 放入到web-inf中后必须通过控制器转发到页面.
    - springmvc拦截器拦截的是控制器,不能拦截jsp  
- 通过拦截器拦截全部控制器,需要在拦截器内部放行login控制器

SpringMVC运行原理
- 如果在web.xml中设置DispatcherServlet 的`<url-pattern>`为/时,当用户发起请求,请求一个控制器,首先会执行DispatcherServlet
- 由DispatcherServlet调用HandlerMapping的DefaultAnnotationHandlerMapping解析URL,解析后调用HandlerAdatper组件的AnnotationMethodHandlerAdapter，调用Controller中的HandlerMethod
- 当HandlerMethod执行完成后会返回View,会被 ViewResovler 进行视图解析,解析后调用 jsp 对应的.class 文件并运行,最终把运行.class 文件的结果响应给客户端



