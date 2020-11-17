# 手把手教你搭建Spring开发环境(IDEA)

首先打开IDEA,HAHAHA

1. 创建Maven项目，我一般习惯用使用maven-archetype-webapp骨架。（为啥使用骨架： *maven骨架的使用*能够帮我们快速的构建结构一致的项目,省时省力。）

   ![image-20200918093209083](C:\Users\apple\AppData\Roaming\Typora\typora-user-images\image-20200918093209083.png)

2. 改下项目名称和GroupId

   ![image-20200918093351817](C:\Users\apple\AppData\Roaming\Typora\typora-user-images\image-20200918093351817.png)

3. 在pom.xml中添加依赖（记得刷新maven）

   ```xml
    <dependencies>
   <!--    单元测试的依赖-->
       <dependency>
         <groupId>junit</groupId>
         <artifactId>junit</artifactId>
         <version>4.11</version>
         <scope>test</scope>
       </dependency>
   <!--    spring的依赖-->
       <dependency>
         <groupId>org.springframework</groupId>
         <artifactId>spring-context</artifactId>
         <version>5.0.2.RELEASE</version>
       </dependency>
   <!--    使用Lombok-->
       <dependency>
         <groupId>org.projectlombok</groupId>
         <artifactId>lombok</artifactId>
         <version>1.16.20</version>
         <scope>provided</scope>
       </dependency>
     </dependencies>
   ```

4. 完成目录结构

   * 一般你右击new一个Directory的时候，缺失的都会显示出来，你直接添加就可以了

   ![image-20200918093548420](C:\Users\apple\AppData\Roaming\Typora\typora-user-images\image-20200918093548420.png)

5. 然后需要添加spring的配置文件了。当然是再resoures下啦，我一般创建的文件名为bean.xml（看你自己喽，我是为了方便记忆）

   * 基本约束

     ```xml
     <?xml version="1.0" encoding="UTF-8"?>
     <beans xmlns="http://www.springframework.org/schema/beans"
            xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
            xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">
     
     </beans>
     ```

   * 所有约束

     ```xml
     <beans xmlns="http://www.springframework.org/schema/beans"
            xmlns:mvc="http://www.springframework.org/schema/mvc"
            xmlns:context="http://www.springframework.org/schema/context"
            xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
            xsi:schemaLocation="
             http://www.springframework.org/schema/beans
             http://www.springframework.org/schema/beans/spring-beans.xsd
             http://www.springframework.org/schema/mvc
             http://www.springframework.org/schema/mvc/spring-mvc.xsd
             http://www.springframework.org/schema/context
             http://www.springframework.org/schema/context/spring-context.xsd">
     </beans>
     ```

6. Let`s go来使用一下

   1. 创建一个实体类

      ```java
      import lombok.Getter;
      import lombok.Setter;
      import lombok.ToString;
      
      @Getter
      @Setter
      @ToString
      public class Student {
          private Integer id;
          private String name;
          private Integer age;
          private Integer score;
      
      }
      ```

   2. 创建对象交给spring来管理吧（在我们刚才的配置文件中添加）

      ```xml
      <?xml version="1.0" encoding="UTF-8"?>
      <beans xmlns="http://www.springframework.org/schema/beans"
             xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
             xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">
      
          <bean id="student" class="model.Student">
              <property name="id" value="1"/>
              <property name="name" value="mxh"/>
              <property name="age" value="21"/>
              <property name="score" value="85"/>
          </bean>
      </beans>
      ```

   3. 使用一哈看看效果

      ```java
      import org.junit.Test;
      import org.springframework.context.ApplicationContext;
      import org.springframework.context.support.ClassPathXmlApplicationContext;
      
      public class SpringTest {
      
          @Test
          public void f(){
              //1.获取核心容器
              ApplicationContext ac = new ClassPathXmlApplicationContext("bean.xml");
              //2.根据id获取对象
              Student s = (Student) ac.getBean("student");
              //3.处理对象
              System.out.println(s);
              System.out.println("test1 "+s.hashCode());
          }
      
          @Test
          public void f1(){
              //1.获取核心容器
              ApplicationContext ac = new ClassPathXmlApplicationContext("bean.xml");
              //2.根据id获取对象
              Student s = (Student) ac.getBean("student");
              //3.处理对象
              System.out.println(s);
              System.out.println("test2 "+s.hashCode());
          }
      }
      ```

      ![image-20200918095720577](C:\Users\apple\AppData\Roaming\Typora\typora-user-images\image-20200918095720577.png)

      ![image-20200918095736107](C:\Users\apple\AppData\Roaming\Typora\typora-user-images\image-20200918095736107.png)

我们可以看出是同一个对象，都来自于spring容器。