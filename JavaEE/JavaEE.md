# JavaEE

### Spring

1. **Bean容器：通过容器统一管理对象的生命周期**

   1. 减少对象创建、销毁的步骤，提高了效率：不用每次都在方法中创建对象，退出方法后对象又很快变为可回收状态
   2. 可以统一管理对象之间的依赖关系

   Bean配置项：

   	1. scope：singleton(单例)和prototype(原型)，每次从容器中获取bean对象时，是否获取同一个
    	2. 静态工厂方法：factory-method
    	3. 实例工厂方法：factory-bean+factory-method

2. **IOC：Inversion of Control 控制反转**

   1. bean对象从程序自行管理的方式，转变为Spring容器管理，控制权发生了反转

      优势：容器统一管理bean对象，对象解耦

3. **DI：Dependency InJection 依赖注入**

   * bean对象设置属性的依赖关系。
   * 注入方式：
     1. 属性注入
     2. 构造方法注入

4. **Sring生命周期**

   ![image-20200627223559683](C:\Users\apple\AppData\Roaming\Typora\typora-user-images\image-20200627223559683.png)

5. **Bean的定义**

   1. XML配置方式
   2. 注解方式：所有定义Bean的注解，需要扫描，容器才会实例化对象(SpringBoot中会自动扫描启动类所在的包路径，之下的Bean注解都可以扫描并注册到容器中)
      1. @Component：可以使用此注解描述Spring中的Bean,但它是一个泛化的概念，仅仅表示个组件，并且可以作用在任何层次，使用时只需将该注解标注在相应的类上即可
      2. @Repository：用于将数据访问层(DAO层)的类标识为Spring中的Bean,其功能与@Component相同
      3. @Service：通常作用于业务层(Service层)，用于将业务层的类标识为Spring中的Bean,其功能与@Component相同
      4. @Controller：通常作用在控制层，用于将控制层的类标识为Spring中的Bean,其功能与@Component相同
      5. @Configuration：设置配置类
      6. @Bean：使用在方法上，表示注册一个名称为方法名的Bean对象到容器中

   * 使用在类上：
     1. 如果能扫描到该类，将注册到容器中
     2. 只会实例化为一个对象
   * 使用在方法上：
     * 根据需要可以注册多个Bean

6. **Bean的使用：装配、注入**

   1. @Autowired：Spring的注解
   2. @Resource：jdk的注解，是一个规范，Spring是实现了这个规范
   3. @Bean所在的方法，以方法参数的方式注入

   如果同一个类型Bean注册了多个对象到容器中，装配时，指定Bean的名称：

   	1. @Autowired+@Qualifier
    	2. @Resource(name="bean的名称")

7. **AOP**

   ![image-20200627230129906](C:\Users\apple\AppData\Roaming\Typora\typora-user-images\image-20200627230129906.png)

   1. 使用场景：
      1. 统计每次http请求的执行时间
      2. 每次http请求中，记录http请求数据到日志文件
      3. 处理Session
      4. 用户访问权限的控制
      5. 数据库事务的处理
   2. 使用AOP的好处：
      * 统一处理，不用在每个方法中加入代码，不会侵入业务代码
   3. AOP的原理：
      * 在编译期及运行期，通过织入字节码的方式，生成原有类型的代理类
   4. AOP的类型：
      1. 静态代理
      2. 动态代理
   5. AOP的实现方式：
      1. JDK的方式：被代理类必须实现接口。基于InvocationHandler+Proxy生成代理类
      2. CGLIB的方式：被代理类不需要实现接口
      3. Aspectj的方式



### SpringMVC

* 请求URL+网络抓包

* @Controller：类上使用，表示定义处理客户端请求的Bean

* ResController：和@Controller作用一样，但是所有的方法都是默认有@ResponseBody的效果

* 请求相关：

  * 统一的拦截器：@Configuration+实现WebMvcConfigurer接口，处理的路径映射，只要匹配路劲的请求，统一会调用拦截器方法

  * @RequedtMappiing：可以使用在类上、方法上，表示客户端请求的配置(包括URI，请求方法，数据类型)

    1. @RequestParam获取请求数据：

       ![image-20200627232123643](C:\Users\apple\AppData\Roaming\Typora\typora-user-images\image-20200627232123643.png)

    2. @RequestBody：请求数据类型Content-Type=application/json

    3. 直接注入HttpServletRequest和HttpServletResponse

    4. @PathVariable：获取请求路径中的变量

* 响应相关：

  * @ResponseBody：返回的Content-Type响应头尾application/json
  * 不带@ResponseBody注解，需要返回字符串，表示静态资源路径
  * ![image-20200627232428425](C:\Users\apple\AppData\Roaming\Typora\typora-user-images\image-20200627232428425.png)
  * 统一的异常处理：配置@ControllerAdvice+@ExceptionHandler统一拦截在Controller请求方法中抛出异常

* SpringMVC的流程：

  * ![image-20200627232520996](C:\Users\apple\AppData\Roaming\Typora\typora-user-images\image-20200627232520996.png)

### SpringBoot初识

1. 默认使用src/main/resource/application.properties作为项目的配置文件
2. main启动：启动类必须要在某个包下
3. 启动类所在的包，会自动扫描(所以要定义Bean,写在启动类之下的包里边)
4. SpringBoot怎么自动化配置的？
   1. Spring的SPI机制：SpringBoot提供了加载依赖包META-INF文件夹中，很多配置文件的方式，来实现化配置类，及做其他的启动时初始化操作