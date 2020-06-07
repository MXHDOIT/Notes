# Spring

#### bean容器：通过容器统一管理对象生命周期

1. 减少对象的创建和回收，提高了效率。(原因：不用每次在方法中创建对象，同时不用在每次退出方法后对象被回收)
2. 可以统一管理对象之间的依赖

* Bean配置项
  1. scope：singleton(单例)和prototype(原型)，每次从容器中获取bean对象是，是否获取同一个
  2. 静态工厂方法：factory-method
  3. 实例工厂方法：factory-bean+factory-method

#### IOC (Inversion of Control)控制反转

* bean对象从程序自行管理的方式，转换为Spring容器管理，控制权发生了反转
* 优势：容器统一管理bean对象，对象解耦

#### DI依赖注入：Dependency Injection依赖注入

* bean对象设置属性的依赖关系
* 注入方式：
  * 属性注入
  * 构造方法注入

