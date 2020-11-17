# Spring

## 1、Spring

### 1.1、简介

spring官网： https://spring.io/projects/spring-framework#overview

官方下载： https://repo.spring.io/release/org/springframework/spring/

GitHub： https://github.com/spring-projects/spring-framework

Spring Web MVC： [spring-webmvc最新版](https://mvnrepository.com/artifact/org.springframework/spring-webmvc/5.2.7.RELEASE)

### 1.2、优点

- Spring是一个开源的免费框架（容器）！
- Spring是一个轻量级的非入侵式的框架
- 控制反转（IOC），面向切面编程（AOP）！
- 支持事务的处理，对框架整合的支持

开源免费容器，轻量级非侵入式，控制反转，面向切面，支持事务，支持框架整合

**Spring是一个轻量级的控制反转(IOC)和面向切面(AOP)编程的框架**

## 2、IOC理论推导

1. UserDao 接口

   ```java
   public interface UserDao {
      public void getUser();
   }
   123
   1234
   ```

2. UserDaoImpl 实现类

   ```java
   public class UserDaoImpl implements UserDao {
      @Override
      public void getUser() {
          System.out.println("获取用户数据");
     }
   }
   123456
   1234567
   ```

3. UserService 业务接口

   ```java
   public interface UserService {
      public void getUser();
   }
   123
   1234
   ```

4. UserServiceImpl 业务实现类

   ```java
   public class UserServiceImpl implements UserService {
      private UserDao userDao = new UserDaoImpl();
   
      @Override
      public void getUser() {
          userDao.getUser();
     }
   }
   12345678
   123456789
   ```

5. 测试一下

   ```java
   @Test
   public void test(){
      UserService service = new UserServiceImpl();
      service.getUser();
   }
   12345
   123456
   ```

把Userdao的实现类增加一个 .

```java
public class UserDaoMySqlImpl implements UserDao {
   @Override
   public void getUser() {
       System.out.println("MySql获取用户数据");
  }
}
123456
1234567
```

紧接着我们要去使用MySql的话 , 我们就需要去service实现类里面修改对应的实现

```java
public class UserServiceImpl implements UserService {
   private UserDao userDao = new UserDaoMySqlImpl();

   @Override
   public void getUser() {
       userDao.getUser();
  }
}
12345678
123456789
```

**代码量十分大，修改一次的成本十分昂贵！**

我们使用一个Set接口实现，已经发生了革命性的变化！

```java
public class UserServiceImpl implements UserService {
   private UserDao userDao;
	// 利用set实现
   public void setUserDao(UserDao userDao) {
       this.userDao = userDao;
  }

   @Override
   public void getUser() {
       userDao.getUser();
  }
}
123456789101112
12345678910111213
```

- 之前，程序是主动创建对象，控制权在程序员手上！
- 使用了set注入后，程序不再具有主动性，而是变成了被动的接受对象！

这种思想，从本质上解决了问题，我们程序员不用再去管对象的创建了。系统的耦合性大大降低，可以专注在业务的实现上！这是IOC的原型！

### IOC本质

⭐️**控制反转IoC(Inversion of Control)，是一种设计思想，DI(依赖注入)是实现IoC的一种方法**，也有人认为DI只是IoC的另一种说法。没有IoC的程序中 , 我们使用面向对象编程 , 对象的创建与对象间的依赖关系完全硬编码在程序中，对象的创建由程序自己控制，控制反转后将对象的创建转移给第三方，个人认为所谓控制反转就是：获得依赖对象的方式反转了。

![在这里插入图片描述](https://img-blog.csdnimg.cn/20200628180055895.png#pic_center)

采用XML方式配置Bean的时候，Bean的定义信息是和实现分离的，而采用注解的方式可以把两者合为一体，Bean的定义信息直接以注解的形式定义在实现类中，从而达到了零配置的目的。

⭐️**控制反转是一种通过描述（XML或注解）并通过第三方去生产或获取特定对象的方式。在Spring中实现控制反转的是IoC容器，其实现方法是依赖注入（Dependency Injection,DI）。**

## 3、HelloSpring

在父模块中导入jar包

```xml
<!-- https://mvnrepository.com/artifact/org.springframework/spring-webmvc -->
<dependency>
	<groupId>org.springframework</groupId>
	<artifactId>spring-webmvc</artifactId>
	<version>5.2.7.RELEASE</version>
</dependency>
123456
1234567
```

pojo的Hello.java

```java
package pojo;

public class Hello {

	private String str;
	
	public String getStr() {
		return str;
	}

	public void setStr(String str) {
		this.str = str;
	}
	
	@Override
	public String toString() {
		return "Holle [str=" + str + "]";
	}
}
12345678910111213141516171819
```

在resource里面的xml配置

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
        https://www.springframework.org/schema/beans/spring-beans.xsd">


    <!--在Spring中创建对象，在Spring这些都称为bean
    	类型 变量名 = new 类型();
    	Holle holle = new Holle();
    	
    	bean = 对象(holle)
    	id = 变量名(holle)
    	class = new的对象(new Holle();)
    	property 相当于给对象中的属性设值,让str="Spring"
    -->
    
    <bean id="hello" class="pojo.Hello">
        <property name="str" value="Spring"/>
    </bean>
</beans>
123456789101112131415161718192021
```

测试类MyTest

```java
package holle1;

import org.springframework.context.ApplicationContext;
import org.springframework.context.support.ClassPathXmlApplicationContext;

import pojo.Hello;

public class MyTest {

	public static void main(String[] args) {
		//获取Spring的上下文对象
		ApplicationContext context = new ClassPathXmlApplicationContext("beans.xml");
		//我们的对象下能在都在spring·中管理了，我们要使用，直接取出来就可以了
		Hello holle = (Hello) context.getBean("hello");
		System.out.println(holle.toString());
	}

}
123456789101112131415161718
```

⭐️**核心用set注入，所以必须要有下面的set()方法**

```java
//Hello类
public void setStr(String str) {
		this.str = str;
	}
1234
```

⭐️**思考：**

![image-20200801165156259](https://imgconvert.csdnimg.cn/aHR0cHM6Ly9naXRlZS5jb20vd29fYmVsbC9QaWN0dXJlQmVkL3Jhdy9tYXN0ZXIvaW1hZ2UvaW1hZ2UtMjAyMDA4MDExNjUxNTYyNTkucG5n?x-oss-process=image/format,png)
IOC：对象由Spring 来创建，管理，装配！

## 4、IOC创建对象的方式

1. 使用无参构造创建对象，默认。
2. 使用有参构造（如下）

下标赋值

**index指的是有参构造中参数的下标，下标从0开始;**

```java
package com.kuang.pojo;

import lombok.ToString;
@ToString
public class User {
    private  String name;
    public User(String name){
        this.name=name;
    }
}
12345678910
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
        https://www.springframework.org/schema/beans/spring-beans.xsd">
    
    <bean id="user" class="com.kuang.pojo.User">
       <constructor-arg index="0" value="狂神" />
    </bean>
    
</beans>
1234567891011
public class MyTest {
    public static void main(String[] args) {
        ApplicationContext context=new ClassPathXmlApplicationContext("beans.xml");
        User user=(User) context.getBean("user");
        System.out.println(user);
    }
}
1234567
```

类型赋值（不建议使用）

```xml
<bean id="user" class="com.kuang.pojo.User">
        <constructor-arg type="String" value="刘烽先"/>
</bean>
123
```

**直接通过参数名（掌握）**

```xml
<bean id="user" class="pojo.User">
    <constructor-arg name="name" value="kuang"></constructor-arg>
</bean>
<!-- 比如参数名是name，则有name="具体值" -->
1234
```

注册bean之后就对象的初始化了（**类似 new 类名()**）

弹幕评论：

name方式还需要无参构造和set方法,index和type只需要有参构造

就算是new 两个对象，也是只有一个实例（**单例模式：全局唯一**）

```java
User user = (User) context.getBean("user");
User user2 = (User) context.getBean("user");
system.out.println(user == user2)//结果为true
123
```

总结：在配置文件加载的时候，容器(< bean>)中管理的对象就已经初始化了

## 5、Spring配置

### 5.1、别名

```xml
<bean id="user" class="pojo.User">
    <constructor-arg name="name" value="chen"></constructor-arg>
</bean>

<alias name="user" alias="userLove"/>
<!-- 使用时
	User user2 = (User) context.getBean("userLove");	
-->
12345678
```

### 5.2、Bean的配置(name也可以当别名)

```xml
<!--id：bean的唯一标识符，也就是相当于我们学的对象名
class：bean对象所对应的会限定名：包名+类型
name：也是别名，而且name可以同时取多个别名 -->
<bean id="user" class="pojo.User" name="u1 u2,u3;u4">
    <property name="name" value="chen"/>
</bean>
<!-- 使用时
	User user2 = (User) context.getBean("u1");	
-->
123456789
```

### 5.3、import

import一般用于团队开发使用，它可以将多个配置文件，导入合并为一个

假设，现在项目中有多个人开发，这三个人复制不同的类开发，不同的类需要注册在不同的bean中，我们可以利
用import将所有人的beans.xml合并为一个总的！

- 张三(beans.xm1)

- 李四(beans2.xm1)

- 王五(beans3.xm1)

- applicationContext.xml

  ```xml
  <import resource="beans.xm1"/>
  <import resource="beans2.xml"/>
  <import resource="beans3.xm1"/>
  123
  ```

**使用的时候，直接使用总的配置就可以了**

弹幕评论：

按照在总的xml中的导入顺序来进行创建，后导入的会重写先导入的，最终实例化的对象会是后导入xml中的那个

## 6、依赖注入（DI）

### 6.1、构造器注入

第4点有提到

### 6.2、set方式注入【重点】

依赖注入：set注入！

- 依赖：bean对象的创建依赖于容器

- 注入：bean对象中的所有属性，由容器来注入

  【环境搭建】

  1. 复杂类型

     Address类

  2. 真实测试对象

     Student类

  3. beans.xml

  4. 测试

     MyTest3

  Student类

```java
package com.kuang.pojo;

import lombok.Data;
import lombok.Getter;
import lombok.Setter;
import lombok.ToString;
import java.util.*;
import java.util.Map;
import java.util.Properties;
import java.util.Set;
@Setter
@Getter
public class Student {//别忘了写get和set方法（用lombok注解也行）
    private String name;
    private Address address;

    private String[] books;
    private List<String> hobbies;

    private Map<String, String> card;
    private Set<String> game;

    private Properties infor;
    private String wife;

    @Override
    public String toString() {
        return "Student{" +"\n"+
                "name='" + name + '\'' +"\n"+
                ", address=" + address.toString() +"\n"+
                ", books=" + Arrays.toString(books) +"\n"+
                ", hobbies=" + hobbies +"\n"+
                ", card=" + card +"\n"+
                ", game=" + game +"\n"+
                ", infor=" + infor +"\n"+
                ", wife='" + wife + '\'' +"\n"+
                '}';
    }
}

12345678910111213141516171819202122232425262728293031323334353637383940
```

Address类

```java
package com.kuang.pojo;

public class Address {

    private String address;

    public String getAddress() {
        return address;
    }

    public void setAddress(String address) {
        this.address = address;
    }
    @Override
    public String toString() {
        return "Address{" +
                "address='" + address + '\'' +
                '}';
    }
}

123456789101112131415161718192021
```

beans.xml

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
        https://www.springframework.org/schema/beans/spring-beans.xsd">

    <bean id="address" class="com.kuang.pojo.Address">
        <property name="address" value="address你好" />
    </bean>

    <bean id="student" class="com.kuang.pojo.Student">
        <!--第一种，普通值注入 -->
        <property name="name" value="name你好" />
        <!--第二种，ref注入 -->
        <property name="address" ref="address" />

        <!--数组注入 -->
        <property name="books">
            <array>
                <value>三国</value>
                <value>西游</value>
                <value>水浒</value>
            </array>
        </property>

        <!--list列表注入 -->
        <property name="hobbies">
            <list>
                <value>唱</value>
                <value>跳</value>
                <value>rap</value>
                <value>篮球</value>
            </list>
        </property>

        <!--map键值对注入 -->
        <property name="card">
            <map>
                <entry key="username" value="root" />
                <entry key="password" value="root" />
            </map>
        </property>

        <!--set(可去重)注入 -->
        <property name="game">
            <set>
                <value>wangzhe</value>
                <value>lol</value>
                <value>galname</value>
            </set>
        </property>

        <!--空指针null注入 -->
        <property name="wife">
            <null></null>
        </property>

        <!--properties常量注入 -->
        <property name="infor">
            <props>
                <prop key="id">20200802</prop>
                <prop key="name">cbh</prop>
            </props>
        </property>
    </bean>
</beans>
123456789101112131415161718192021222324252627282930313233343536373839404142434445464748495051525354555657585960616263646566
```

MyTest

```java
public class MyTest {
    public static void main(String[] args) {
        ApplicationContext context = new ClassPathXmlApplicationContext("beans.xml");
        Student stu = (Student) context.getBean("student");
        System.out.println(stu.toString());
    }
123456
```

### 6.3、拓展注入

(p命名空间注入实质是set注入)

c命名空间，通过构造器注入

pojo增加User类

```java
package pojo;

public class User {
    private String name;
    private int id;
	public User() {
        
	}
	public User(String name, int id) {
		super();
		this.name = name;
		this.id = id;
	}
	public String getName() {
		return name;
	}
	public void setName(String name) {
		this.name = name;
	}
	public int getId() {
		return id;
	}
	public void setId(int id) {
		this.id = id;
	}
	@Override
	public String toString() {
		return "User [name=" + name + ", id=" + id + "]";
	}
}
123456789101112131415161718192021222324252627282930
```

注意： beans 里面加上这下面两行

使用p和c命名空间需要导入xml约束

xmlns:p=“http://www.springframework.org/schema/p”
xmlns:c=“http://www.springframework.org/schema/c”

UserBean.xml

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:p="http://www.springframework.org/schema/p"
       xmlns:c="http://www.springframework.org/schema/c"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
        https://www.springframework.org/schema/beans/spring-beans.xsd">

    <!--p命名空间注入/set注入，可以直接注入属性的值-》property-->
    <bean id="user" class="com.kuang.pojo.User" p:name="cxk" p:id="20" >
    </bean>

    <!--c命名空间，通过构造器注入，需要写入有参和无参构造方法-》construct-args-->
    <bean id="user2" class="com.kuang.pojo.User" c:name="cbh" c:id="22"></bean>
</beans>
123456789101112131415
```

测试

```java
public void test01(){
        ApplicationContext context = new ClassPathXmlApplicationContext("UserBean.xml");
        User user = context.getBean("user",User.class);//确定class对象，就不用再强转了
        System.out.println(user.toString());
    }
12345
```

### 6.4、Bean作用域

![image-20200802143401165](https://imgconvert.csdnimg.cn/aHR0cHM6Ly9naXRlZS5jb20vd29fYmVsbC9QaWN0dXJlQmVkL3Jhdy9tYXN0ZXIvaW1hZ2UvaW1hZ2UtMjAyMDA4MDIxNDM0MDExNjUucG5n?x-oss-process=image/format,png)

![image-20200802143342586](https://imgconvert.csdnimg.cn/aHR0cHM6Ly9naXRlZS5jb20vd29fYmVsbC9QaWN0dXJlQmVkL3Jhdy9tYXN0ZXIvaW1hZ2UvaW1hZ2UtMjAyMDA4MDIxNDMzNDI1ODYucG5n?x-oss-process=image/format,png)

1. 单例模式（默认）

   ```xml
   <bean id="user2" class="pojo.User" c:name="cxk" c:age="19" scope="singleton"></bean>
   1
   ```

![image-20200802143802005](https://imgconvert.csdnimg.cn/aHR0cHM6Ly9naXRlZS5jb20vd29fYmVsbC9QaWN0dXJlQmVkL3Jhdy9tYXN0ZXIvaW1hZ2UvaW1hZ2UtMjAyMDA4MDIxNDM4MDIwMDUucG5n?x-oss-process=image/format,png)
弹幕评论：单例模式是把对象放在pool中，需要再取出来，使用的都是同一个对象实例

1. 原型模式: 每次从容器中get的时候，都产生一个新对象！

   ```xml
   <bean id="user2" class="pojo.User" c:name="cxk" c:age="19" scope="prototype"></bean>
   1
   12
   ```

![image-20200802143826227](https://imgconvert.csdnimg.cn/aHR0cHM6Ly9naXRlZS5jb20vd29fYmVsbC9QaWN0dXJlQmVkL3Jhdy9tYXN0ZXIvaW1hZ2UvaW1hZ2UtMjAyMDA4MDIxNDM4MjYyMjcucG5n?x-oss-process=image/format,png)

1. 其余的request、session、application这些只能在web开放中使用！

## 7、Bean的自动装配

- 自动装配是Spring满足bean依赖的一种方式
- Spring会在上下文自动寻找，并自动给bean装配属性

在Spring中有三种装配的方式

1. 在xml中显示配置

2. 在java中显示配置

3. 隐式的自动装配bean 【重要】

4. 环境搭建：一个人有两个宠物

5. byType自动装配：byType会自动查找，和自己对象set方法参数的类型相同的bean

   保证所有的class唯一(类为全局唯一)

6. byName自动装配：byName会自动查找，和自己对象set对应的值对应的id

   保证所有id唯一，并且和set注入的值一致

```xml
<!-- 找不到id和多个相同class -->
<bean id="cat1" class="pojo.Cat"/>
<bean id="cat2" class="pojo.Cat"/>
找不到 id=cat，且有两个Cat
1234
```

### 7.1测试：自动装配

pojo的Cat类

```java
public class Cat {
    public void shut(){
        System.out.println("miao");
    }
}
12345
123456
```

pojo的Dog类

```java
public class Dog {

    public void shut(){
        System.out.println("wow");
    }

}
1234567
```

pojo的People类

```java
public class People {
    
    private Cat cat;
    private Dog dog;
    private String name;

    public Cat getCat() {
        return cat;
    }

    public void setCat(Cat cat) {
        this.cat = cat;
    }

    public Dog getDog() {
        return dog;
    }

    public void setDog(Dog dog) {
        this.dog = dog;
    }

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }
    @Override
    public String toString() {
        return "People{" +
                "cat=" + cat +
                ", dog=" + dog +
                ", name='" + name + '\'' +
                '}';
    }
}
1234567891011121314151617181920212223242526272829303132333435363738
```

xml配置(手动装配)

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
        https://www.springframework.org/schema/beans/spring-beans.xsd">
    <bean id="dog" class="com.kuang.pojo.Dog"/>
    <bean id="cat" class="com.kuang.pojo.Cat"/>

    <bean id="people" class="com.kuang.pojo.People">
        <property name="name" value="狂神"/>
        <property name="cat" ref="cat"/>
        <property name="dog" ref="dog"/>
    </bean>
</beans>
1234567891011121314
```

测试：

```java
public class MyTest {
    public static void main(String[] args) {
        ApplicationContext context=new ClassPathXmlApplicationContext("beans.xml");
        People p=context.getBean("people",People.class);
        p.getCat().shut();
        p.getDog().shut();
	System.out.println( p.getName());
    }
}
123456789
```

xml配置 -> byType 自动装配(-byType会在容器自动查找，和自己对象属性相同的bean例如，Dog dog; 那么就会查找pojo的Dog类，再进行自动装配)

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
        https://www.springframework.org/schema/beans/spring-beans.xsd">

    <bean id="dog" class="com.kuang.pojo.Dog"/>
    <bean id="cat" class="com.kuang.pojo.Cat"/>
    
    <!--byType会在容器自动查找，和自己对象属性相同的bean
		例如，Dog dog; 那么就会查找pojo的Dog类，再进行自动装配
	-->
    <bean id="people" class="pojo.People" autowire="byType">
        <property name="name" value="cbh"></property>
    </bean>

</beans>
1234567891011121314151617
```

xml配置 -> byName 自动装配(byname会在容器自动查找，和自己对象set方法的set后面的值对应的id
例如:setDog()，取set后面的字符作为id，则要id = dog 才可以进行自动装配)

```xml
 <bean id="dog" class="com.kuang.pojo.Dog"/>
  <bean id="cat" class="com.kuang.pojo.Cat"/>
<!--byname会在容器自动查找，和自己对象set方法的set后面的值对应的id
  例如:setDog()，取set后面的字符作为id，则要id = dog 才可以进行自动装配
  
 -->
  <bean id="people2" class="com.kuang.pojo.People" autowire="byName">
        <property name="name" value="人类"/>
   </bean>
123456789
```

弹幕评论：byName只能取到小写，大写取不到

#### 7.2.2、@Autowired+@Qualifier

**@Autowired不能唯一装配时，需要@Autowired+@Qualifier**

如果@Autowired自动装配环境比较复杂。自动装配无法通过一个注解完成的时候，可以使用@Qualifier(value = “dog”)去配合使用，指定一个唯一的id对象

```java
public class People {
    @Autowired
    private Cat cat;
    @Autowired
    @Qualifier(value = "dog")
    private Dog dog;
    private String name;
}
12345678
```

弹幕评论：

如果xml文件中同一个对象被多个bean使用，Autowired无法按类型找到，可以用@Qualifier指定id查找

#### 7.2.3、@Resource

**默认是byName方式，如果匹配不上，就会byType**

```java
public class People {
    Resource(name="cat")
    private Cat cat;
    Resource(name="dog")
    private Dog dog;
    private String name;
}
1234567
```

弹幕评论：

Autowired是byType，@Autowired+@Qualifier = byType || byName

Autowired是先byteType,如果唯一則注入，否则byName查找。resource是先byname,不符合再继续byType

#### 区别：

@Resource和@Autowired的区别：

- 都是用来自动装配的，都可以放在属性字段上
- @Autowired通过byType的方式实现，而且必须要求这个对象存在！【常用】
- @Resource默认通过byname的方式实现，如果找不到名字，则通过byType实现！如果两个都找不到的情况下，就报错！【常用】
- 执行顺序不同：@Autowired通过byType的方式实现。@Resource默认通过byname的方式实现

## 8、使用注解开发

在spring4之后，使用注解开发，必须要保证aop包的导入
![image-20200802201924490](https://imgconvert.csdnimg.cn/aHR0cHM6Ly9naXRlZS5jb20vd29fYmVsbC9QaWN0dXJlQmVkL3Jhdy9tYXN0ZXIvaW1hZ2UvaW1hZ2UtMjAyMDA4MDIyMDE5MjQ0OTAucG5n?x-oss-process=image/format,png)
使用注解需要导入contex的约束

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
    xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
    xmlns:context="http://www.springframework.org/schema/context"
    xsi:schemaLocation="http://www.springframework.org/schema/beans
        https://www.springframework.org/schema/beans/spring-beans.xsd
        http://www.springframework.org/schema/context
        https://www.springframework.org/schema/context/spring-context.xsd">

    <context:annotation-config/>

</beans>
123456789101112
12345678910111213
```

### 8.1、bean

弹幕评论：
有了< context:component-scan>，另一个< context:annotation-config/>标签可以移除掉，因为已经被包含进去了。

```xml
<!--指定要扫描的包，这个包下面的注解才会生效
	别只扫一个com.kuang.pojo包--> 
<context:component-scan base-package="com.kuang"/> 
<context:annotation-config/>
1234
//@Component 组件
//等价于<bean id="user" classs"pojo.User"/> 
@Component
public class User {  
     public String name ="秦疆";
}
1234567891011
```

### 8.2、属性如何注入@value

```java
@Component
public class User { 
    //相当于<property name="name" value="kuangshen"/> 
    @value("kuangshen") 
    public String name; 
    
    //也可以放在set方法上面
    //@value("kuangshen")
    public void setName(String name) { 
        this.name = name; 
    }
}
123456789101112
```

### 8.3、衍生的注解

@Component有几个衍生注解，会按照web开发中，mvc架构中分层。

- dao （@Repository）
- service（@Service）
- controller（@Controller）

**这四个注解的功能是一样的，都是代表将某个类注册到容器中**

### 8.4、自动装配置

@Autowired：默认是byType方式，如果匹配不上，就会byName

@Nullable：字段标记了这个注解，说明该字段可以为空

@Resource：默认是byName方式，如果匹配不上，就会byType

### 8.5、作用域@scope

```java
//原型模式prototype，单例模式singleton
//scope("prototype")相当于<bean scope="prototype"></bean>
@Component 
@scope("prototype")
public class User { 
    
    //相当于<property name="name" value="kuangshen"/> 
    @value("kuangshen") 
    public String name; 
    
    //也可以放在set方法上面
    @value("kuangshen")
    public void setName(String name) { 
        this.name = name; 
    }
}
12345678910111213141516
```

### 8.6、小结

**xml与注解：**

- xml更加万能，维护简单，适用于任何场合
- 注解，不是自己的类使用不了，维护复杂

**最佳实践：**

- xml用来管理bean
- 注解只用来完成属性的注入
- 要开启注解支持

## 9、使用Java的方式配置Spring

不使用Spring的xml配置，完全交给java来做！

Spring的一个子项目，在spring4之后，，，它成为了核心功能

![image-20200802215752868](https://imgconvert.csdnimg.cn/aHR0cHM6Ly9naXRlZS5jb20vd29fYmVsbC9QaWN0dXJlQmVkL3Jhdy9tYXN0ZXIvaW1hZ2UvaW1hZ2UtMjAyMDA4MDIyMTU3NTI4NjgucG5n?x-oss-process=image/format,png)
**实体类：pojo的User.java**

```java
//这里这个注解的意思,就是说明这个类被Spring接管了,注册到了容器中 
@component 
public class User { 
    private String name;
    
    public String getName() { 
    	return name; 
    } 
    //属性注入值
    @value("QINJIANG')  
    public void setName(String name) { 
    	this.name = name; 
    } 
    @Override 
    public String toString() { 
        return "user{" + 
        "name='" + name + '\''+ 
        '}'; 
    } 
}
1234567891011121314151617181920
123456789101112131415161718192021
```

弹幕评论：要么使用@Bean，要么使用@Component和ComponentScan，两种效果一样

**配置文件：config中的kuang.java**

@Import(KuangConfig2.class)，用@import来包含KuangConfig2.java

```java
//这个也会Spring容器托管,注册到容器中,因为他本米就是一个@Component 
// @Configuration表这是一个配置类,就像我们之前看的beans.xml，类似于<beans>标签
@Configuration 
@componentScan("com.Kuang.pojo") //开启扫描
//@Import(KuangConfig2.class)
public class KuangConfig { 
    //注册一个bean , 就相当于我们之前写的一个bean 标签 
    //这个方法的名字,就相当于bean 标签中的 id 属性 ->getUser
    //这个方法的返同值,就相当于bean 标签中的class 属性 ->User
    
    //@Bean 
    public User getUser(){ 
    	return new User(); //就是返回要注入到bean的对象! 
    } 
}
123456789101112131415
12345678910111213141516
```

弹幕评论：ComponentScan、@Component("pojo”) 这两个注解配合使用

**测试类**

```java
public class MyTest { 
    public static void main(String[ ] args) { 
    //如果完全使用了配置类方式去做,我们就只能通过 Annotationconfig 上下文来获取容器,通过配置类的class对象加载! 
    ApplicationContext context = new AnnotationConfigApplicationContext(KuangConfig.Class); //class对象
    User getUser =(User)context.getBean( "getUser"); //方法名getUser
    System.out.Println(getUser.getName()); 
    } 
}
12345678
123456789
```

**会创建两个相同对象问题的说明：**

**弹幕总结 - -> @Bean是相当于< bean>标签创建的对象，而我们之前学的@Component是通过spring自动创建的这个被注解声明的对象，所以这里相当于有两个User对象被创建了。一个是bean标签创建的（@Bean），一个是通过扫描然后使用@Component，spring自动创建的User对象，所以这里去掉@Bean这些东西，然后开启扫描。之后在User头上用@Component即可达到spring自动创建User对象了**

```java
//这个也会Spring容器托管,注册到容器中,因为他本米就是一个@Component 
// @Configuration表这是一个配置类,就像我们之前看的beans.xml，类似于<beans>标签
@Configuration 
@componentScan("com.Kuang.pojo") //开启扫描
//@Import(KuangConfig2.class)
public class KuangConfig { 
    //注册一个bean , 就相当于我们之前写的一个bean 标签 
    //这个方法的名字,就相当于bean 标签中的 id 属性 ->getUser
    //这个方法的返同值,就相当于bean 标签中的class 属性 ->User
    
    //@Bean 
    public User getUser(){ 
    	return new User(); //就是返回要注入到bean的对象! 
    } 
}
123456789101112131415
12345678910111213141516
```

弹幕评论：ComponentScan、@Component("pojo”) 这两个注解配合使用

**测试类**

```java
public class MyTest { 
    public static void main(String[ ] args) { 
    //如果完全使用了配置类方式去做,我们就只能通过 Annotationconfig 上下文来获取容器,通过配置类的class对象加载! 
    ApplicationContext context = new AnnotationConfigApplicationContext(KuangConfig.Class); //class对象
    User getUser =(User)context.getBean( "getUser"); //方法名getUser
    System.out.Println(getUser.getName()); 
    } 
}
12345678
123456789
```

**会创建两个相同对象问题的说明：**

**弹幕总结 - -> @Bean是相当于< bean>标签创建的对象，而我们之前学的@Component是通过spring自动创建的这个被注解声明的对象，所以这里相当于有两个User对象被创建了。一个是bean标签创建的（@Bean），一个是通过扫描然后使用@Component，spring自动创建的User对象，所以这里去掉@Bean这些东西，然后开启扫描。之后在User头上用@Component即可达到spring自动创建User对象了**

## 10、动态代理

代理模式是SpringAOP的底层

分类：动态代理和静态代理

![image-20200803101427846](https://imgconvert.csdnimg.cn/aHR0cHM6Ly9naXRlZS5jb20vd29fYmVsbC9QaWN0dXJlQmVkL3Jhdy9tYXN0ZXIvaW1hZ2UvaW1hZ2UtMjAyMDA4MDMxMDE0Mjc4NDYucG5n?x-oss-process=image/format,png)

### 10.1、静态代理

![image-20200803101621868](https://imgconvert.csdnimg.cn/aHR0cHM6Ly9naXRlZS5jb20vd29fYmVsbC9QaWN0dXJlQmVkL3Jhdy9tYXN0ZXIvaW1hZ2UvaW1hZ2UtMjAyMDA4MDMxMDE2MjE4NjgucG5n?x-oss-process=image/format,png)
代码步骤：

1、接口

```java
package pojo;
public interface Host {
	public void rent();
}
1234
12345
```

2、真实角色

```java
package pojo;
public class HostMaster implements Host{	
    
	public void rent() {
		System.out.println("房东要出租房子");
	}
}
1234567
12345678
```

3、代理角色

```java
package pojo;
public class Proxy {

	public Host host;
	
	public Proxy() {
		
	}
	
	public Proxy(Host host) {
		super();
		this.host = host;
	}
	
	public void rent() {
		seeHouse();
		host.rent();
		fee();
		sign();
	}
	//看房
	public void seeHouse() {
		System.out.println("看房子");
	}
	//收费
	public void fee() {
		System.out.println("收中介费");
	}
	//合同
	public void sign() {
		System.out.println("签合同");
	}		
}
123456789101112131415161718192021222324252627282930313233
```

4、客户端访问代理角色

```java
package holle4_proxy;

import pojo.Host;
import pojo.HostMaster;
import pojo.Proxy;

public class My {

	public static void main(String[] args) {
		//房东要出租房子
		Host host = new HostMaster();
		//中介帮房东出租房子，但也收取一定费用（增加一些房东不做的操作）
		Proxy proxy = new Proxy(host);
		//看不到房东，但通过代理，还是租到了房子
		proxy.rent();
		
	}
}
123456789101112131415161718
```

![image-20200803105229478](https://imgconvert.csdnimg.cn/aHR0cHM6Ly9naXRlZS5jb20vd29fYmVsbC9QaWN0dXJlQmVkL3Jhdy9tYXN0ZXIvaW1hZ2UvaW1hZ2UtMjAyMDA4MDMxMDUyMjk0NzgucG5n?x-oss-process=image/format,png)
代码翻倍：几十个真实角色就得写几十个代理

AOP横向开发

![image-20200803111539621](https://imgconvert.csdnimg.cn/aHR0cHM6Ly9naXRlZS5jb20vd29fYmVsbC9QaWN0dXJlQmVkL3Jhdy9tYXN0ZXIvaW1hZ2UvaW1hZ2UtMjAyMDA4MDMxMTE1Mzk2MjEucG5n?x-oss-process=image/format,png)

## 11、AOP

### 11.1、什么是AOP

![image-20200803134502169](https://imgconvert.csdnimg.cn/aHR0cHM6Ly9naXRlZS5jb20vd29fYmVsbC9QaWN0dXJlQmVkL3Jhdy9tYXN0ZXIvaW1hZ2UvaW1hZ2UtMjAyMDA4MDMxMzQ1MDIxNjklMjAtJTIwQU9QLnBuZw?x-oss-process=image/format,png)

### 11.2、AOP在Spring中的使用

提供声明式事务，允许用户自定义切面

- 横切关注点：跨越应用程序多个模块的方法或功能。即是，与我们业务逻辑无关的，但是我们需要关注的部分，就是横切关注点。如日志，安全，缓存，事务等等…
- 切面(Aspect)：横切关注点 被模块化的特殊对象。即，它是一个类。（Log类）
- 通知(Advice)：切面必须要完成的工作。即，它是类中的一个方法。（Log类中的方法）
- 目标(Target)：被通知对象。（生成的代理类)
- 代理(Proxy)：向目标对象应用通知之后创建的对象。（生成的代理类）
- 切入点(PointCut)：切面通知执行的”地点”的定义。（最后两点：在哪个地方执行，比如：method.invoke()）
- 连接点(JointPoint)：与切入点匹配的执行点。

![image-20200803154043909](https://imgconvert.csdnimg.cn/aHR0cHM6Ly9naXRlZS5jb20vd29fYmVsbC9QaWN0dXJlQmVkL3Jhdy9tYXN0ZXIvaW1hZ2UvaW1hZ2UtMjAyMDA4MDMxNTQwNDM5MDkucG5n?x-oss-process=image/format,png)
SpringAOP中，通过Advice定义横切逻辑，Spring中支持5种类型的Advice:

![image-20200803135937435](https://imgconvert.csdnimg.cn/aHR0cHM6Ly9naXRlZS5jb20vd29fYmVsbC9QaWN0dXJlQmVkL3Jhdy9tYXN0ZXIvaW1hZ2UvaW1hZ2UtMjAyMDA4MDMxMzU5Mzc0MzUucG5n?x-oss-process=image/format,png)
**即AOP在不改变原有代码的情况下，去增加新的功能。**（代理）

### 11.3、使用Spring实现AOP

导入jar包

```xml
<!-- https://mvnrepository.com/artifact/org.aspectj/aspectjweaver -->
<dependency>
    <groupId>org.aspectj</groupId>
    <artifactId>aspectjweaver</artifactId>
    <version>1.9.4</version>
</dependency>
123456
1234567
```

#### 11.3.1、方法一：使用原生spring接口

springAPI接口实现

applicationContext.xml

```xml
<?xml version = "1.0" encoding = "UTF-8"?>
<beans xmlns = "http://www.springframework.org/schema/beans"
       xmlns:xsi = "http://www.w3.org/2001/XMLSchema-instance"
       xmlns:aop = "http://www.springframework.org/schema/aop"
       xsi:schemaLocation = "http://www.springframework.org/schema/beans
        http://www.springframework.org/schema/beans/spring-beans-3.0.xsd
        http://www.springframework.org/schema/aop
        http://www.springframework.org/schema/aop/spring-aop-3.0.xsd ">

    <!-- bean definition & AOP specific configuration -->

    <!--注册bean-->
    <bean id="userService" class="com.kuang.service.UserServiceImpl"/>
    <bean id="log" class="com.kuang.log.Log" />
    <bean id="afterlog" class="com.kuang.log.AfterLog" />
    <!--方式一，使用原生的spring API接口-->
    <!--注册aop-->
    <!--配置aop需要导入aop的约束-->

    <aop:config>
        <!--aop的切入点:expression表达式,execution:要执行的位置-->
        <aop:pointcut id="pointcut" expression="execution(* com.kuang.service.UserServiceImpl.*(..))"/>
        <!--执行环绕增强-->
        <aop:advisor advice-ref="log" pointcut-ref="pointcut" />
        <aop:advisor advice-ref="afterlog" pointcut-ref="pointcut"/>
    </aop:config>
</beans>
123456789101112131415161718192021222324252627
```

execution(返回类型，类名，方法名(参数)) -> execution(* com.service.*,*(…))

UserService.java

```java
package com.kuang.service;

public interface UserService {
    public void add();
    public void update();
    public void delete();
    public void select();
}
12345678
```

UserService 的实现类 UserServiceImp.java

```java
package com.kuang.service;

public class UserServiceImpl implements UserService {

    public void add() {
        System.out.println("增加了一个用户");
    }

    public void update() {
        System.out.println("更新了一个用户");
    }

    public void delete() {
        System.out.println("删除了一个用户");
    }

    public void select() {
        System.out.println("查询了一个用户");
    }
}
1234567891011121314151617181920
```

前置Log.java

```java
package com.kuang.log;

import org.springframework.aop.MethodBeforeAdvice;

import java.lang.reflect.Method;

public class Log implements MethodBeforeAdvice {
    //method：要执行的目标对象的方法
    //args：参数
    //target：目标对象
    public void before(Method method, Object[] args, Object target) throws Throwable {
        System.out.println(target.getClass().getName()+"的"+method.getName()+"被执行了");
    }
}
1234567891011121314
```

后置AfterLog.java

```java
package com.kuang.log;

import org.springframework.aop.AfterReturningAdvice;

import java.lang.reflect.Method;

public class AfterLog implements AfterReturningAdvice {

    //returnVaule: 返回值
    public void afterReturning(Object returnValue, Method method, Object[] args, Object target) throws Throwable {
        System.out.println("执行了"+method.getName()+"方法，返回结果为"+returnValue);
    }
}
12345678910111213
```

测试类MyTest

```java
import com.kuang.service.UserService;
import org.springframework.context.ApplicationContext;
import org.springframework.context.support.ClassPathXmlApplicationContext;

public class MyTest {
    public static void main(String[] args) {
        ApplicationContext context=new ClassPathXmlApplicationContext("application.xml");
        //动态代理代理的是接口
        UserService userService= (UserService) context.getBean("userService");
        userService.add();


    }
}
1234567891011121314
```

#### 11.3.2、方法二：自定义类实现AOP

application.xml

```xml
<?xml version = "1.0" encoding = "UTF-8"?>
<beans xmlns = "http://www.springframework.org/schema/beans"
       xmlns:xsi = "http://www.w3.org/2001/XMLSchema-instance"
       xmlns:aop = "http://www.springframework.org/schema/aop"
       xsi:schemaLocation = "http://www.springframework.org/schema/beans
        http://www.springframework.org/schema/beans/spring-beans-3.0.xsd
        http://www.springframework.org/schema/aop
        http://www.springframework.org/schema/aop/spring-aop-3.0.xsd ">

    <!-- bean definition & AOP specific configuration -->

    <!--注册bean-->
    <bean id="userService" class="com.kuang.service.UserServiceImpl"/>
    <bean id="log" class="com.kuang.log.Log" />
    <bean id="afterlog" class="com.kuang.log.AfterLog" />
    <!--方式一，使用原生的spring API接口-->
    <!--注册aop-->
    <!--配置aop需要导入aop的约束-->

    <!--<aop:config>
        &lt;!&ndash;aop的切入点:expression表达式,execution:要执行的位置&ndash;&gt;
        <aop:pointcut id="pointcut" expression="execution(* com.kuang.service.UserServiceImpl.*(..))"/>
        &lt;!&ndash;执行环绕增强&ndash;&gt;
        <aop:advisor advice-ref="log" pointcut-ref="pointcut" />
        <aop:advisor advice-ref="afterlog" pointcut-ref="pointcut"/>
    </aop:config>-->
    <!--方式二,自定义切面-->
    <bean id="diy" class="com.kuang.diy.DIYPoint"/>
    <aop:config>
        <!--自定义切面，ref:要引用的类-->
        <aop:aspect ref="diy">
            <aop:pointcut id="point" expression="execution(* com.kuang.service.UserServiceImpl.*(..))"/>
            <!--通知-->
            <aop:before method="before" pointcut-ref="point"/>
            <aop:after method="after" pointcut-ref="point"/>
        </aop:aspect>
    </aop:config>
</beans>
1234567891011121314151617181920212223242526272829303132333435363738
```

DIYPoint.java

```
package com.kuang.diy;

public class DIYPoint {
    public void before(){
        System.out.println("====方法执行前======");
    }
    public void after(){
        System.out.println("====方法执行后======");
    }
}

1234567891011
```

#### 11.3.3、方法三：使用注解实现

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:aop="http://www.springframework.org/schema/aop"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
		https://www.springframework.org/schema/beans/spring-beans.xsd
		http://www.springframework.org/schema/aop
		https://www.springframework.org/schema/aop/spring-aop.xsd">
	
    <!-- 注册 -->
    <bean id="userservice" class="service.UserServiceImpl"/>
    <!--方式三，使用注解实现-->
    <bean id="diyAnnotation" class="diy.DiyAnnotation"></bean>
    
    <!-- 开启自动代理 
		实现方式：默认JDK (proxy-targer-class="fasle")
    			 cgbin (proxy-targer-class="true")-->
	<aop:aspectj-autoproxy/>
    
</beans>
1234567891011121314151617181920
```

DiyAnnotation.java

```java
package com.kuang.diy;

import org.aspectj.lang.ProceedingJoinPoint;
import org.aspectj.lang.annotation.After;
import org.aspectj.lang.annotation.Around;
import org.aspectj.lang.annotation.Aspect;
import org.aspectj.lang.annotation.Before;

//使用注解aop
@Aspect //标注这个类是一个切面
public class AnnotationPointCut {
    @Before("execution(* com.kuang.service.UserServiceImpl.*(..))")
    public void before(){
        System.out.println("======方法执行前2=======");
    }
    @After("execution(* com.kuang.service.UserServiceImpl.*(..))")
    public void after(){
        System.out.println("======方法执行后2=======");
    }
    //在环绕增强中，我们可以给地暖管一个参数，代表我们要获取切入的点
    @Around("execution(* com.kuang.service.UserServiceImpl.*(..))")
    public void arrount(ProceedingJoinPoint jp) throws Throwable{
        System.out.println("环绕前");
        Object proceed = jp.proceed();
        System.out.println("环绕后");
    }
}


1234567891011121314151617181920212223242526272829
```

测试

```java
public class MyTest {
    public static void main(String[] args) {
        ApplicationContext context = new ClassPathXmlApplicationContext("applicationContext.xml");
        //注意:动态代理代理的是接口
        UserService userService = (UserService) context.getBean("userservice");
        userService.add();
    }
}
12345678
```

输出结果：

![image-20200803175642064](https://imgconvert.csdnimg.cn/aHR0cHM6Ly9naXRlZS5jb20vd29fYmVsbC9QaWN0dXJlQmVkL3Jhdy9tYXN0ZXIvaW1hZ2UvaW1hZ2UtMjAyMDA4MDMxNzU2NDIwNjQucG5n?x-oss-process=image/format,png)

## 12、整合mybatis

mybatis-spring官网：https://mybatis.org/spring/zh/

**mybatis的配置流程：**

1. 编写实体类
2. 编写核心配置文件
3. 编写接口
4. 编写Mapper.xmi
5. 测试

### 12.1、mybatis-spring-方式一

1. 编写数据源配置
2. sqISessionFactory
3. sqISessionTemplate（相当于sqISession）
4. 需要给接口加实现类【new】
5. 将自己写的实现类，注入到Spring中
6. 测试！

先导入jar包

```xml
<dependencies>

    <!-- https://mvnrepository.com/artifact/org.springframework/spring-webmvc -->
    <dependency>
        <groupId>org.springframework</groupId>
        <artifactId>spring-webmvc</artifactId>
        <version>5.2.7.RELEASE</version>
    </dependency>

    <dependency>
        <groupId>org.aspectj</groupId>
        <artifactId>aspectjweaver</artifactId>
        <version>1.9.4</version>
    </dependency>

    <!-- https://mvnrepository.com/artifact/org.springframework/spring-jdbc -->
    <dependency>
        <groupId>org.springframework</groupId>
        <artifactId>spring-jdbc</artifactId>
        <version>5.2.7.RELEASE</version>
    </dependency>

    <dependency>
        <groupId>org.mybatis</groupId>
        <artifactId>mybatis</artifactId>
        <version>3.5.2</version>
    </dependency>

    <dependency>
        <groupId>org.mybatis</groupId>
        <artifactId>mybatis-spring</artifactId>
        <version>2.0.4</version>
    </dependency>

    <dependency>
        <groupId>mysql</groupId>
        <artifactId>mysql-connector-java</artifactId>
        <version>8.0.12</version>
    </dependency>

    <dependency>
        <groupId>org.projectlombok</groupId>
        <artifactId>lombok</artifactId>
        <version>1.18.12</version>
    </dependency>

</dependencies>
	
<!--在build中配置resources，来防止资源导出失败的问题-->
<!-- Maven解决静态资源过滤问题 -->
<build>
<resources>
    <resource>
        <directory>src/main/java</directory>
        <includes>
            <include>**/*.properties</include>
            <include>**/*.xml</include>
        </includes>
        <filtering>false</filtering>
    </resource>
    <resource>
        <directory>src/main/resources</directory>
        <includes>
            <include>**/*.properties</include>
            <include>**/*.xml</include>
        </includes>
        <filtering>false</filtering>
    </resource>
</resources>
</build>
```

![文件路径](https://imgconvert.csdnimg.cn/aHR0cHM6Ly9naXRlZS5jb20vd29fYmVsbC9QaWN0dXJlQmVkL3Jhdy9tYXN0ZXIvaW1hZ2UvaW1hZ2UtMjAyMDA4MDQxMjMyMTA1NjAucG5n?x-oss-process=image/format,png)
**编写顺序：**
**User -> UserMapper -> UserMapper.xml -> spring-dao.xml -> UserServiceImpl -> applicationContext.xml -> MyTest6**

**代码步骤：**

pojo实体类 User

```java
package pojo;
import lombok.Data;
@Data
public class User {
	 private int id;
    private String name;
    private String pwd;
}
12345678
```

mapper目录下的 UserMapper、UserMapperImpl、UserMapper.xml

接口UserMapper

```java
package mapper;
import java.util.List;
import pojo.User;
public interface UserMapper {
	public List<User> getUser();
}
123456
```

UserMapperImpl

```java
package mapper;
import java.util.List;
import org.mybatis.spring.SqlSessionTemplate;
import pojo.User;

public class UserMapperImpl implements UserMapper{
	
	//我们的所有操作，在原来都使用sqlSession来执行，现在都使用SqlSessionTemplate；
	private SqlSessionTemplate sqlSessionTemplate;

	public void setSqlSessionTemplate(SqlSessionTemplate sqlSessionTemplate) {
		this.sqlSessionTemplate = sqlSessionTemplate;
	}

	public List<User> getUser() {
		UserMapper mapper = sqlSessionTemplate.getMapper(UserMapper.class);
		return mapper.getUser();
	}
}
```

UserMapper.xml （狂神给面子才留下来的）

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper
        PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-mapper.dtd">

<!--namespace=绑定一个指定的Dao/Mapper接口-->
<mapper namespace="com.kuang.mapper.UserMapper">
    <select id="getUserList" resultType="com.kuang.pojo.User">
    select * from mybatis.user
  </select>
</mapper>
```

resource目录下的 mybatis-config.xml、spring-dao.xml、applicationContext.xml

mybatis-config.xml

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE configuration
        PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-config.dtd">
<!--configuration核心配置文件-->
<configuration>

    <!-- 配置mybatis的log实现为STDOUT_LOGGING -->
    <settings>
        <setting name="logImpl" value="STDOUT_LOGGING"/>
    </settings>

</configuration>
```

spring-dao.xml

```xml
<?xml version = "1.0" encoding = "UTF-8"?>
<beans xmlns = "http://www.springframework.org/schema/beans"
       xmlns:xsi = "http://www.w3.org/2001/XMLSchema-instance"
       xmlns:aop = "http://www.springframework.org/schema/aop"
       xsi:schemaLocation = "http://www.springframework.org/schema/beans
        http://www.springframework.org/schema/beans/spring-beans-3.0.xsd
        http://www.springframework.org/schema/aop
        http://www.springframework.org/schema/aop/spring-aop-3.0.xsd ">
    <!--DataSource:使用Spring的数帮源替换Mybatis的配置 其他数据源：c3p0、dbcp、druid
		这使用Spring提供的JDBC: org.springframework.jdbc.datasource -->
    <!--data source -->
    <bean id="datasource" class="org.springframework.jdbc.datasource.DriverManagerDataSource">
        <property name="driverClassName" value="com.mysql.cj.jdbc.Driver" />
        <property name="url" value="jdbc:mysql://127.0.0.1:3306/mybatis?useSSL=false&amp;useUnicode=true&amp;characterEncoding=utf-8&amp;serverTimezone=Asia/Shanghai"/>
        <property name="username" value="root" />
        <property name="password" value="root" />
    </bean>
    <!--sqlSessionFactory-->
    <bean id="sqlSessionFactory" class="org.mybatis.spring.SqlSessionFactoryBean">
        <property name="dataSource" ref="datasource" />
        <!--绑定 mybatis 配置文件-->
        <property name="configLocation" value="classpath:mybatis-config.xml"/>
        <property name="mapperLocations" value="classpath:com/kuang/mapper/*.xml"/>
    </bean>
    <!-- sqlSessionTemplate 就是之前使用的：sqlsession -->
    <bean id="sqlSession" class="org.mybatis.spring.SqlSessionTemplate">
        <!-- 只能使用构造器注入sqlSessionFactory 原因：它没有set方法-->
        <constructor-arg index="0" ref="sqlSessionFactory"/>
    </bean>

</beans>
```

applicationContext.xml

```xml
<?xml version = "1.0" encoding = "UTF-8"?>
<beans xmlns = "http://www.springframework.org/schema/beans"
       xmlns:xsi = "http://www.w3.org/2001/XMLSchema-instance"
       xmlns:aop = "http://www.springframework.org/schema/aop"
       xsi:schemaLocation = "http://www.springframework.org/schema/beans
        http://www.springframework.org/schema/beans/spring-beans-3.0.xsd
        http://www.springframework.org/schema/aop
        http://www.springframework.org/schema/aop/spring-aop-3.0.xsd ">

        <!-- 导入spring-dao.xml -->
        <import resource="spring-dao.xml"/>

    <bean id="userMapper" class="com.kuang.mapper.UserMapperImpl">
        <property name="sqlSession" ref="sqlSession"></property>
    </bean>
</beans>
```

测试类

```java
public class MyTest {
    @Test
    public void test01(){
        ApplicationContext context=new ClassPathXmlApplicationContext("applicationContext.xml");
        UserMapper userMapper=context.getBean("userMapper",UserMapper.class);
        for (User user:userMapper.getUserList()){
            System.out.println(user);
        }
    }
}
12345678910
```

### 12.2、mybatis-spring-方式二

UserServiceImpl2

```java
package com.kuang.mapper;

import com.kuang.pojo.User;
import org.apache.ibatis.session.SqlSession;
import org.mybatis.spring.support.SqlSessionDaoSupport;

import java.util.List;
import java.util.Map;

public class UserMapperImpl2 extends SqlSessionDaoSupport implements UserMapper {

    public List<User> getUserList() {
        SqlSession sqlSession=getSqlSession();
        UserMapper mapper=sqlSession.getMapper(UserMapper.class);
        return mapper.getUserList();
    }

    public void addUser2(Map<String, Object> map) {

    }

    public List<User> getUserLike(String value) {
        return null;
    }

    public List<User> getUserLimit(Map<String, Integer> map) {
        return null;
    }
}
```

spring-dao.xml

```xml
<?xml version = "1.0" encoding = "UTF-8"?>
<beans xmlns = "http://www.springframework.org/schema/beans"
       xmlns:xsi = "http://www.w3.org/2001/XMLSchema-instance"
       xmlns:aop = "http://www.springframework.org/schema/aop"
       xsi:schemaLocation = "http://www.springframework.org/schema/beans
        http://www.springframework.org/schema/beans/spring-beans-3.0.xsd
        http://www.springframework.org/schema/aop
        http://www.springframework.org/schema/aop/spring-aop-3.0.xsd ">
    <!--DataSource:使用Spring的数帮源替换Mybatis的配置 其他数据源：c3p0、dbcp、druid
		这使用Spring提供的JDBC: org.springframework.jdbc.datasource -->
    <!--data source -->
    <bean id="datasource" class="org.springframework.jdbc.datasource.DriverManagerDataSource">
        <property name="driverClassName" value="com.mysql.cj.jdbc.Driver" />
        <property name="url" value="jdbc:mysql://127.0.0.1:3306/mybatis?useSSL=false&amp;useUnicode=true&amp;characterEncoding=utf-8&amp;serverTimezone=Asia/Shanghai"/>
        <property name="username" value="root" />
        <property name="password" value="root" />
    </bean>
    <!--sqlSessionFactory-->
    <bean id="sqlSessionFactory" class="org.mybatis.spring.SqlSessionFactoryBean">
        <property name="dataSource" ref="datasource" />
        <!--绑定 mybatis 配置文件-->
        <property name="configLocation" value="classpath:mybatis-config.xml"/>
        <property name="mapperLocations" value="classpath:com/kuang/mapper/*.xml"/>
    </bean>
    <!-- sqlSessionTemplate 就是之前使用的：sqlsession -->
    <bean id="sqlSession" class="org.mybatis.spring.SqlSessionTemplate">
        <!-- 只能使用构造器注入sqlSessionFactory 原因：它没有set方法-->
        <constructor-arg index="0" ref="sqlSessionFactory"/>
    </bean>

</beans>
```

applicationContext.xml

```xml
<?xml version = "1.0" encoding = "UTF-8"?>
<beans xmlns = "http://www.springframework.org/schema/beans"
       xmlns:xsi = "http://www.w3.org/2001/XMLSchema-instance"
       xmlns:aop = "http://www.springframework.org/schema/aop"
       xsi:schemaLocation = "http://www.springframework.org/schema/beans
        http://www.springframework.org/schema/beans/spring-beans-3.0.xsd
        http://www.springframework.org/schema/aop
        http://www.springframework.org/schema/aop/spring-aop-3.0.xsd ">

        <!-- 导入spring-dao.xml -->
        <import resource="spring-dao.xml"/>

    <bean id="userMapper" class="com.kuang.mapper.UserMapperImpl">
        <property name="sqlSession" ref="sqlSession"></property>
    </bean>
    <bean id="userMapper2" class="com.kuang.mapper.UserMapperImpl2">
        <property name="sqlSessionFactory" ref="sqlSessionFactory"/>
    </bean>
</beans>
```

测试

```java
public class MyTest {
    @Test
    public void test01(){
        ApplicationContext context=new ClassPathXmlApplicationContext("applicationContext.xml");
        UserMapper userMapper=context.getBean("userMapper2",UserMapper.class);
        for (User user:userMapper.getUserList()){
            System.out.println(user);
        }
    }
}
12345678910
```

## 13. 声明式事务

- 把一组业务当成一个业务来做；要么都成功，要么都失败！
- 事务在项目开发中，十分的重要，涉及到数据的一致性问题
- 确保完整性和一致性

事务的ACID原则：
1、原子性
2、隔离性
3、一致性
4、持久性

ACID参考文章：https://www.cnblogs.com/malaikuangren/archive/2012/04/06/2434760.html

Spring中的事务管理

- 声明式事务：AOP
- 编程式事务：需要再代码中，进行事务管理

**声明式事务**

先导入jar包

```xml
<dependencies>

    <!-- https://mvnrepository.com/artifact/org.springframework/spring-webmvc -->
    <dependency>
        <groupId>org.springframework</groupId>
        <artifactId>spring-webmvc</artifactId>
        <version>5.2.7.RELEASE</version>
    </dependency>

    <dependency>
        <groupId>org.aspectj</groupId>
        <artifactId>aspectjweaver</artifactId>
        <version>1.9.4</version>
    </dependency>

    <!-- https://mvnrepository.com/artifact/org.springframework/spring-jdbc -->
    <dependency>
        <groupId>org.springframework</groupId>
        <artifactId>spring-jdbc</artifactId>
        <version>5.2.7.RELEASE</version>
    </dependency>

    <dependency>
        <groupId>org.mybatis</groupId>
        <artifactId>mybatis</artifactId>
        <version>3.5.2</version>
    </dependency>

    <dependency>
        <groupId>org.mybatis</groupId>
        <artifactId>mybatis-spring</artifactId>
        <version>2.0.4</version>
    </dependency>

    <dependency>
        <groupId>mysql</groupId>
        <artifactId>mysql-connector-java</artifactId>
        <version>8.0.12</version>
    </dependency>

    <dependency>
        <groupId>org.projectlombok</groupId>
        <artifactId>lombok</artifactId>
        <version>1.18.12</version>
    </dependency>

</dependencies>
	
<!--在build中配置resources，来防止资源导出失败的问题-->
<!-- Maven解决静态资源过滤问题 -->
<build>
<resources>
    <resource>
        <directory>src/main/java</directory>
        <includes>
            <include>**/*.properties</include>
            <include>**/*.xml</include>
        </includes>
        <filtering>false</filtering>
    </resource>
    <resource>
        <directory>src/main/resources</directory>
        <includes>
            <include>**/*.properties</include>
            <include>**/*.xml</include>
        </includes>
        <filtering>false</filtering>
    </resource>
</resources>
</build>
```

**代码步骤：**

pojo实体类 User

```java
@Data
@ToString
@AllArgsConstructor
@NoArgsConstructor
public class User {
    private int id;
    private String name;
    private String pwd;
}
123456789
```

mapper目录下的 UserMapper、UserMapperImpl、UserMapper.xml

接口UserMapper

```java
public interface UserMapper {
    public List<User> getUser();

    public int insertUser(User user);

    public int delUser(@Param("id") int id);
}
1234567
```

UserMapper.xml

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper
        PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-mapper.dtd">

<!--namespace=绑定一个指定的Dao/Mapper接口-->

<!-- 绑定接口 -->
<mapper namespace="com.kuang.mapper.UserMapper">
    <select id="getUser" resultType="com.kuang.pojo.User">
		select * from user
	</select>

    <insert id="insertUser"  parameterType="com.kuang.pojo.User" >
		insert into mybatis.User (id,name,pwd) values (#{id},#{name},#{pwd})
	</insert>

    <delete id="delUser" parameterType="_int">
        delete AAAfrom mybatis.user where id = #{id}
        <!-- deleteAAAAA是故意写错的 -->
    </delete>

</mapper>
1234567891011121314151617181920212223
```

UserMapperImpl( extends SqlSessionDaoSupport)

```java
public class UserMapperImpl extends SqlSessionDaoSupport implements UserMapper {

    public List<User> getUser() {
        User user = new User(8,"你好","wwww");

        //insertUser(user);
        //delUser(7);
        SqlSession sqlSession = getSqlSession();
        UserMapper mapper = sqlSession.getMapper(UserMapper.class);
        mapper.insertUser(user);
        mapper.delUser(9);
        return mapper.getUser();
        //或者return  getSqlSession().getMapper(UserMapper.class).getUser();
    }
    //插入
    public int insertUser(User user) {
        return getSqlSession().getMapper(UserMapper.class).insertUser(user);
    }
    //删除
    public int delUser(int id) {
        return getSqlSession().getMapper(UserMapper.class).delUser(id);
    }
}

12345678910111213141516171819202122232425
```

resource目录下的 mybatis-config.xml、spring-dao.xml、applicationContext.xml

mybatis-config.xml

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE configuration
        PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-config.dtd">
<!--configuration核心配置文件-->
<configuration>

    <!-- 配置mybatis的log实现为STDOUT_LOGGING -->
    <settings>
        <setting name="logImpl" value="STDOUT_LOGGING"/>
    </settings>

</configuration>

1234567891011121314
```

spring-dao.xml

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:aop="http://www.springframework.org/schema/aop"
       xmlns:tx="http://www.springframework.org/schema/tx"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
        https://www.springframework.org/schema/beans/spring-beans.xsd
        http://www.springframework.org/schema/tx
        https://www.springframework.org/schema/tx/spring-tx.xsd
        http://www.springframework.org/schema/aop
        https://www.springframework.org/schema/aop/spring-aop.xsd">
    <!--DataSource:使用Spring的数帮源替换Mybatis的配置 其他数据源：c3p0、dbcp、druid
		这使用Spring提供的JDBC: org.springframework.jdbc.datasource -->
    <!--data source -->
    <bean id="datasource" class="org.springframework.jdbc.datasource.DriverManagerDataSource">
        <property name="driverClassName" value="com.mysql.cj.jdbc.Driver" />
        <property name="url" value="jdbc:mysql://localhost:3306/mybatis?useSSL=false&amp;useUnicode=true&amp;characterEncoding=utf-8&amp;serverTimezone=Asia/Shanghai"/>
        <property name="username" value="root" />
        <property name="password" value="root" />
    </bean>
    <!--sqlSessionFactory-->
    <bean id="sqlSessionFactory" class="org.mybatis.spring.SqlSessionFactoryBean">
        <property name="dataSource" ref="datasource" />
        <!--绑定 mybatis 配置文件-->
        <property name="configLocation" value="classpath:mybatis-config.xml"/>
        <property name="mapperLocations" value="classpath:com/kuang/mapper/*.xml"/>
    </bean>
    <!-- sqlSessionTemplate 就是之前使用的：sqlsession -->
    <bean id="sqlSession" class="org.mybatis.spring.SqlSessionTemplate">
        <!-- 只能使用构造器注入sqlSessionFactory 原因：它没有set方法-->
        <constructor-arg index="0" ref="sqlSessionFactory"/>
    </bean>
    <!--配置声明式事务-->
    <bean id="transactionManager" class="org.springframework.jdbc.datasource.DataSourceTransactionManager">
        <constructor-arg ref="datasource" />
    </bean>
    <!--结合aop实现事务织入-->
    <!--配置事务的通知类-->
    <tx:advice id="txAdvice" transaction-manager="transactionManager" >
        <!--给哪些方法配置事务-->
        <!--新东西：配置事务的传播特性 propagation-->
        <tx:attributes>
            <tx:method name="insert"/>
            <tx:method name="del" propagation="REQUIRED"/>
            <tx:method name="getUser" propagation="REQUIRED"/>
            <tx:method name="*"/>

        </tx:attributes>
    </tx:advice>
    <!--配置事务切入-->
    <aop:config>
        <aop:pointcut id="txPointCut" expression="execution(* com.kuang.mapper.*.*(..))"/>
        <aop:advisor advice-ref="txAdvice" pointcut-ref="txPointCut"/>
    </aop:config>
</beans>
12345678910111213141516171819202122232425262728293031323334353637383940414243444546474849505152535455
```

###### 其中spring七个事物传播属性：

PROPAGATION_REQUIRED – 支持当前事务，如果当前没有事务，就新建一个事务。这是最常见的选择。
　PROPAGATION_SUPPORTS – 支持当前事务，如果当前没有事务，就以非事务方式执行。
　PROPAGATION_MANDATORY – 支持当前事务，如果当前没有事务，就抛出异常。
　PROPAGATION_REQUIRES_NEW – 新建事务，如果当前存在事务，把当前事务挂起。
　PROPAGATION_NOT_SUPPORTED – 以非事务方式执行操作，如果当前存在事务，就把当前事务挂起。
　PROPAGATION_NEVER – 以非事务方式执行，如果当前存在事务，则抛出异常。
　PROPAGATION_NESTED – 如果当前存在事务，则在嵌套事务内执行。如果当前没有事务，
则进行与PROPAGATION_REQUIRED类似的操作。

applicationContext.xml

```xml
<?xml version = "1.0" encoding = "UTF-8"?>
<beans xmlns = "http://www.springframework.org/schema/beans"
       xmlns:xsi = "http://www.w3.org/2001/XMLSchema-instance"
       xmlns:aop = "http://www.springframework.org/schema/aop"
       xsi:schemaLocation = "http://www.springframework.org/schema/beans
        http://www.springframework.org/schema/beans/spring-beans-3.0.xsd
        http://www.springframework.org/schema/aop
        http://www.springframework.org/schema/aop/spring-aop-3.0.xsd ">

        <!-- 导入spring-dao.xml -->
        <import resource="spring-dao.xml"/>

    <bean id="userMapper" class="com.kuang.mapper.UserMapperImpl">
        <property name="sqlSessionFactory" ref="sqlSessionFactory"></property>
    </bean>

</beans>
1234567891011121314151617
```

测试类

```java
public class MyTest {

    @Test
    public void test02(){
        ApplicationContext context = new ClassPathXmlApplicationContext("applicationContext.xml");

        UserMapper userMapper = (UserMapper) context.getBean("userMapper");

        for (User user : userMapper.getUser()) {
            System.out.println(user);
        }
    }
}

123456789101112131415
```

anager=“transactionManager” >


tx:attributes
<tx:method name=“insert”/>
<tx:method name=“del” propagation=“REQUIRED”/>
<tx:method name=“getUser” propagation=“REQUIRED”/>
<tx:method name="*"/>

```
    </tx:attributes>
</tx:advice>
<!--配置事务切入-->
<aop:config>
    <aop:pointcut id="txPointCut" expression="execution(* com.kuang.mapper.*.*(..))"/>
    <aop:advisor advice-ref="txAdvice" pointcut-ref="txPointCut"/>
</aop:config>
1234567
```

\```

###### 其中spring七个事物传播属性：

PROPAGATION_REQUIRED – 支持当前事务，如果当前没有事务，就新建一个事务。这是最常见的选择。
　PROPAGATION_SUPPORTS – 支持当前事务，如果当前没有事务，就以非事务方式执行。
　PROPAGATION_MANDATORY – 支持当前事务，如果当前没有事务，就抛出异常。
　PROPAGATION_REQUIRES_NEW – 新建事务，如果当前存在事务，把当前事务挂起。
　PROPAGATION_NOT_SUPPORTED – 以非事务方式执行操作，如果当前存在事务，就把当前事务挂起。
　PROPAGATION_NEVER – 以非事务方式执行，如果当前存在事务，则抛出异常。
　PROPAGATION_NESTED – 如果当前存在事务，则在嵌套事务内执行。如果当前没有事务，
则进行与PROPAGATION_REQUIRED类似的操作。

applicationContext.xml

```xml
<?xml version = "1.0" encoding = "UTF-8"?>
<beans xmlns = "http://www.springframework.org/schema/beans"
       xmlns:xsi = "http://www.w3.org/2001/XMLSchema-instance"
       xmlns:aop = "http://www.springframework.org/schema/aop"
       xsi:schemaLocation = "http://www.springframework.org/schema/beans
        http://www.springframework.org/schema/beans/spring-beans-3.0.xsd
        http://www.springframework.org/schema/aop
        http://www.springframework.org/schema/aop/spring-aop-3.0.xsd ">

        <!-- 导入spring-dao.xml -->
        <import resource="spring-dao.xml"/>

    <bean id="userMapper" class="com.kuang.mapper.UserMapperImpl">
        <property name="sqlSessionFactory" ref="sqlSessionFactory"></property>
    </bean>

</beans>
1234567891011121314151617
```

测试类

```java
public class MyTest {

    @Test
    public void test02(){
        ApplicationContext context = new ClassPathXmlApplicationContext("applicationContext.xml");

        UserMapper userMapper = (UserMapper) context.getBean("userMapper");

        for (User user : userMapper.getUser()) {
            System.out.println(user);
        }
    }
}
```