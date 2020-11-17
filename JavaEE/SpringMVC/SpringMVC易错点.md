# SpringMVC

### 重点知识

* SpringMVC的执行流程(P5,可以多听几遍)
* SpringMVC的使用
* SpringMVC与Spring和Mybatis的整合

### 0. MVC是什么

* M：model（JavaBean）
  * 业务逻辑
  * 保存数据的状态
* V：view（jsp）
  * 显示页面
* C：controller（servlet）
  1. 取得表单数据
  2. 调用业务逻辑
  3. 转向指定的页面

## 1、SpringMVC执行原理

**springmvc-servlet.xml:**

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
       http://www.springframework.org/schema/beans/spring-beans.xsd">

    <!--添加 处理映射器-->
    <bean class="org.springframework.web.servlet.handler.BeanNameUrlHandlerMapping"/>

    <!--添加 处理器适配器-->
    <bean class="org.springframework.web.servlet.mvc.SimpleControllerHandlerAdapter"/>

    <!--添加 视图解析器-->
    <!--视图解析器:DispatcherServlet给他的ModelAndView
        1.获取了ModelAndView的数据
        2.解析ModelAndView的视图名字
        3.拼接视图名字，找到对应的视图 /WEB-INF/jsp/hello.jsp
        4.将数据渲染到这个视图上
    -->

    <bean class="org.springframework.web.servlet.view.InternalResourceViewResolver" id="InternalResourceViewResolver">
        <!--前缀-->
        <property name="prefix" value="/WEB-INF/jsp/"/>
        <!--后缀-->
        <property name="suffix" value=".jsp"/>
    </bean>

    <!--Handler-->
    <bean id="/hello" class="com.kuang.controller.HelloController"/>

</beans>
12345678910111213141516171819202122232425262728293031
```

![在这里插入图片描述](https://img-blog.csdnimg.cn/20200707094352968.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0RERERlbmdf,size_16,color_FFFFFF,t_70)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200707094502345.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0RERERlbmdf,size_16,color_FFFFFF,t_70)

图为SpringMVC的一个较完整的流程图，实线表示SpringMVC框架提供的技术，不需要开发者实现，虚线表示需要开发者实现。

**简要分析执行流程**

> 1. DispatcherServlet表示前置控制器，是整个SpringMVC的控制中心。用户发出请求，DispatcherServlet接收请求并拦截请求。
>
> 我们假设请求的url为 : http://localhost:8080/SpringMVC/hello
>
> **如上url拆分成三部分：**
>
> http://localhost:8080服务器域名
>
> SpringMVC部署在服务器上的web站点
>
> hello表示控制器
>
> 通过分析，如上url表示为：请求位于服务器localhost:8080上的SpringMVC站点的hello控制器。
>
> 1. HandlerMapping为处理器映射。DispatcherServlet调用HandlerMapping,HandlerMapping根据请求url查找Handler。
> 2. HandlerExecution表示具体的Handler,其主要作用是根据url查找控制器，如上url被查找控制器为：hello。
> 3. HandlerExecution将解析后的信息传递给DispatcherServlet,如解析控制器映射等。
> 4. HandlerAdapter表示处理器适配器，其按照特定的规则去执行Handler。
> 5. Handler让具体的Controller执行。
> 6. Controller将具体的执行信息返回给HandlerAdapter,如ModelAndView。
> 7. HandlerAdapter将视图逻辑名或模型传递给DispatcherServlet。
> 8. DispatcherServlet调用视图解析器(ViewResolver)来解析HandlerAdapter传递的逻辑视图名。
> 9. 视图解析器将解析的逻辑视图名传给DispatcherServlet。
> 10. DispatcherServlet根据视图解析器解析的视图结果，调用具体的视图。
> 11. 最终视图呈现给用户。

**自己总结理解：**

**SpringMVC一共做了三件事情**：

1. 根据请求的url找到Controller【处理器映射 HandlerMapping】
2. 按照特定的规则去执行Controller返回ModelAndView【处理器适配器 HandlerAdapter】
3. 将Model渲染到指定的视图(解析视图)【视图解析器 ViewResolver】

**流程：**

**多总结，多改善**

1. 用户发送的所有请求，首先要经过前置控制器DispatcherServlet。
2. DispatcherServlet根据url从处理器映射HandlerMapping找到Handler(其实就是Controller)。
3. 然后DispatcherServlet找到具体的Controller类去执行，并返回结果ModelAndView,结果包括视图逻辑名和模型。
4. 而后DispatcherServlet调用视图解析器(ViewResolver)解析ModelAndView的视图名，把数据渲染到视图，返回给DispatcherServlet
5. 最后DispatcherServlet将视图呈现给用户。

## 2、SpringMVC中的 / 和 /*

> / : 只匹配所有的请求，不去匹配jsp页面
>
> /* : 匹配所有的请求，包括jsp页面, /* 会匹配 *.jsp，会出现返回 jsp视图时再次进入spring的DispatcherServlet 类，导致找不到对应的controller所以报404错。

```xml
<servlet>
    <servlet-name>springmvc</servlet-name>
    <servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>
    <!--DispatcherServlet需要绑定Spring的配置文件-->
    <init-param>
        <param-name>contextConfigLocation</param-name>
        <param-value>classpath:springmvc-servlet.xml</param-value>
    </init-param>
    <!--启动级别-->
    <load-on-startup>1</load-on-startup>
</servlet>

<servlet-mapping>
    <servlet-name>springmvc</servlet-name>
    <url-pattern>/</url-pattern>
</servlet-mapping>
12345678910111213141516
```

## 3、使用注解开发SpringMVC

**springmvc-servlet.xml配置：**

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
      xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
      xmlns:context="http://www.springframework.org/schema/context"
      xmlns:mvc="http://www.springframework.org/schema/mvc"
      xsi:schemaLocation="http://www.springframework.org/schema/beans
       http://www.springframework.org/schema/beans/spring-beans.xsd
       http://www.springframework.org/schema/context
       https://www.springframework.org/schema/context/spring-context.xsd
       http://www.springframework.org/schema/mvc
       https://www.springframework.org/schema/mvc/spring-mvc.xsd">

   <!-- 自动扫描包，让指定包下的注解生效,由IOC容器统一管理 -->
   <context:component-scan base-package="com.kuang.controller"/>
   <!-- 让Spring MVC不处理静态资源 -->
   <mvc:default-servlet-handler />
   <!--
   支持mvc注解驱动
       在spring中一般采用@RequestMapping注解来完成映射关系
       要想使@RequestMapping注解生效
       必须向上下文中注册DefaultAnnotationHandlerMapping
       和一个AnnotationMethodHandlerAdapter实例
       这两个实例分别在类级别和方法级别处理。
       而annotation-driven配置帮助我们自动完成上述两个实例的注入。
    -->
   <mvc:annotation-driven />

   <!-- 视图解析器 -->
   <bean class="org.springframework.web.servlet.view.InternalResourceViewResolver"
         id="internalResourceViewResolver">
       <!-- 前缀 -->
       <property name="prefix" value="/WEB-INF/jsp/" />
       <!-- 后缀 -->
       <property name="suffix" value=".jsp" />
   </bean>

</beans>
1234567891011121314151617181920212223242526272829303132333435363738
```

**HelloController.java**

```java
@Controller
public class HelloController {

    @RequestMapping("/hello")
    public String hello(Model model){
        model.addAttribute("msg","Hello,SpringMVCAnnocation!");
        return "hello";
    }
}
123456789
```

**使用注解开发步骤：**

1. 新建一个web项目
2. 导入相关jar包
3. 编写web.xml , 注册DispatcherServlet
4. 编写springmvc配置文件
5. 接下来就是去创建对应的控制类 , controller
6. 最后完善前端视图和controller之间的对应
7. 测试运行调试

## 4、SpringMVC结果跳转方式

**通过SpringMVC来实现转发和重定向 - 有视图解析器；**

重定向 , 不需要视图解析器 , 本质就是重新请求一个新地方嘛 , 所以注意路径问题.

可以重定向到另外一个请求实现

```java
@Controller
public class ResultSpringMVC2 {
   @RequestMapping("/rsm2/t1")
   public String test1(){
       //转发
       return "test";
  }

   @RequestMapping("/rsm2/t2")
   public String test2(){
       //重定向
       return "redirect:/index.jsp";
       //return "redirect:hello.do"; //hello.do为另一个请求/
  }
}
123456789101112131415
```

**通过SpringMVC来实现转发和重定向 - 无需视图解析器；**

测试前，需要将视图解析器注释掉

```java
@Controller
public class ResultSpringMVC {
   @RequestMapping("/rsm/t1")
   public String test1(){
       //转发
       return "/index.jsp";
  }

   @RequestMapping("/rsm/t2")
   public String test2(){
       //转发二
       return "forward:/index.jsp";
  }

   @RequestMapping("/rsm/t3")
   public String test3(){
       //重定向
       return "redirect:/index.jsp";
  }
}
1234567891011121314151617181920
```

## 5、SpringMVC解决乱码问题

**web.xml配置**

```xml
<?xml version="1.0" encoding="UTF-8"?>
<web-app xmlns="http://xmlns.jcp.org/xml/ns/javaee"
        xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
        xsi:schemaLocation="http://xmlns.jcp.org/xml/ns/javaee http://xmlns.jcp.org/xml/ns/javaee/web-app_4_0.xsd"
        version="4.0">

   <!--1.注册servlet-->
   <servlet>
       <servlet-name>SpringMVC</servlet-name>
       <servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>
       <!--通过初始化参数指定SpringMVC配置文件的位置，进行关联-->
       <init-param>
           <param-name>contextConfigLocation</param-name>
           <param-value>classpath:springmvc-servlet.xml</param-value>
       </init-param>
       <!-- 启动顺序，数字越小，启动越早 -->
       <load-on-startup>1</load-on-startup>
   </servlet>

   <!--2.所有请求都会被springmvc拦截 -->
   <servlet-mapping>
       <servlet-name>SpringMVC</servlet-name>
       <url-pattern>/</url-pattern>
   </servlet-mapping>

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
       <url-pattern>/</url-pattern>
   </filter-mapping>

</web-app>
123456789101112131415161718192021222324252627282930313233343536373839
```

## 6、整合SSM框架

### 6.1 排错思路

**问题：bean不存在**

步骤：

1. 查看这个bean注入是否成功 ok

2. Junit单元测试，看我们的代码是否能够查询出来结果 ok

3. 问题，一定不在我们的底层，是Spring出了问题

4. SpringMVC，整合的时候没有调用到我们的service层的bean；

   1. applicationContext.xml 没有注入bean;

   2. web.xml中，我们也绑定过配置文件，发现问题，我们配置的是Spring-mvc.xml，这里面确实没有 service bean;

      ```xml
      <!--DispatcherServlet-->
      <servlet>
          <servlet-name>springmvc</servlet-name>
          <servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>
          <init-param>
              <param-name>contextConfigLocation</param-name>
              <param-value>classpath:spring-mvc.xml</param-value>
          </init-param>
          <load-on-startup>1</load-on-startup>
      </servlet>
      ```





