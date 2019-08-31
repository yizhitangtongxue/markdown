# Spring 学习笔记

## 介绍

1. Spring 是一个开源框架
2. Spring 是为了简化企业级应用开发而生的
3. Spring 是 JavaSE/EE 的一站式框架
4. Spring 解决了业务逻辑层和其他各层的松耦合问题，他将面向接口的编程思想贯穿整个应用系统
5. Spring 兴起于 2003 年，由 Rod Johnson 创建

## 优点

1. 方便解耦，简化开发
    > Spring 是一个大工厂，可以将所有的对象创建和依赖关系维护，交给 Spring 管理
2. AOP 编程的支持
    > Spring 提供面向切面编程，可以方便的实现对程序进行权限拦截，运行监控等功能
3. 支持申明式事务
    > 只需要通过配置就可以完成对事务的管理，无需手动编程
4. 方便程序测试
    > Spring 对 Junit4 支持，可以通过注解方便测试 Spring 程序
5. 方便集成各种优秀框架
    > Spring 内部提供了对各种优秀框架的支持，比如 Mybatis 等
6. 降低 JavaEE API 的使用难度
    > Spring 内部封装了一些在 JAVAEE 中非常难用的 API，比如 JDBC、JavaMail 等

## 学习时碰到的一些坑以及注意事项

1. idea 要使用 2019 1.4 的，之后的版本在编辑 pom.xml 文件时不会自动提示 dependency
2. src/main / 下的 java 文件夹是用来存放 java 代码的，resources 文件夹是用来存放配置文件的
3. 在创建完成项目之后，如果 src/main / 下没有 java 和 resources，则需要手动创建这两个文件夹，并在 java 文件夹下右键 **Mark Directory As *Sources Root***，在 resources 文件夹下右键 **Mark Directory As *ReSources Root***

## Spring 的基础包

1. spring-core
2. spring-beans
3. spring-context
4. spring-expression
5. 其他的一些包名如果不知道的话，可以去 spring 的官网下载一个 spring 框架，解压完之后，文件夹里的 lib 下的 jar 文件就是 spring 的所有包了。或者在编辑 pom.xml 时输入 spring 会自动提示

## Bean

1. Bean 的两种写法
    > 1. ```<bean id=""class=""></bean>```
    > 2. ```<bean id=""class=""/>```
2. Bean 的 id 属性和 name 属性作用一样，用来作为Bean的名称，id和name属性在IOC容器中必须是唯一的
3. Bean 的名称中如果有特殊字符，就需要使用name属性
4. Bean 的 class 属性，用于设置一个类的完全路径名称，主要作用是IOC容器生成类的实例
5. 配置 Bean，将对象的创建权交给 Spring，实现控制反转
6. Bean的作用域默认是singleton，即Bean以单例模式存在，一个IOC容器中只有一个Bean实例
    > 通过配置bean标签的scope属性的值为prototype，可以让我们每次在getBean的时候都重新生成一个新的对象
    > 多例模式一般用在Spring整合struct2的时候，struct2的action是多例的，struct2要交给Spring管理，这时bean标签的scope值就要设置成prototype

## Bean的生命周期

1. Spring初始化bean或者销毁bean时，可以调用bean的两个生命周期方法
    > 1. init-method 当bean被载入到容器时调用
    > 2. destory-method 当bean从容器中删除的时候调用(scope为singleton时有效)
2. 想调用destory-method，需要使用ClassPathXmlApplicationContext.close()方法

## IOC 和 DI

1. IOC Inverse of Control 即控制反转
    > 将原本在程序中手动创建对象的控制权，交给了 Spring 框架管理
    > 简单的说，就是创建对象的控制权被反转到了 Spring 框架
2. DI Dependency Injection 即依赖注入
    > 依赖注入的前提是控制反转
    > 在 Spring 创建整个对象的过程中，将对象所依赖的属性注入进去
3. [IOC 和 DI 基础](https://www.iteye.com/blog/jinnianshilongnian-1413846)

## 控制反转和依赖注入的简单演示

```xml
<!-- 将类的创建权交给了 Spring -->
<bean id="userService" class="com.github.demo1.ClassImpl"> <!-- 此处就是控制反转 -->
    <!-- 设置属性 -->
    <property name="属性名称" value="属性值"/> <!-- 此处就是依赖注入 -->
</bean>
```

1. 通过在配置文件中配置 bean 将对象的创建权交给 Spring，这就是控制反转

2. 在 bean 标签中配置类的属性，这就是依赖注入

3. 想实现依赖注入，必须先有 set 方法

### 传统方式开发与 Spring 方式开发的区别

传统方式，需要手动创建对象，并设置 sayHello 方法需要的属性

```java
    @Test
    /**
     * 传统方式开发
     */
    public void demo1() {
        // UserService userService = new UserServiceImpl();
        UserServiceImpl userService = new UserServiceImpl();

        // 设置属性
        userService.setName("张三");

        userService.sayHello();
    }
```

Spring 方式，对象的创建权交给了 Spring，
在加载 applicationContext 配置文件时，就将所有的单例模式的类实例化成对象了
然后通过 getBean 获得这个对象

```java
    @Test
    /**
     * Spring 的方式实现
     */
    public void demo2() {
        // 读取配置文件，创建 Spring 工厂
        ApplicationContext applicationContext = new ClassPathXmlApplicationContext("applicationContext.xml");
        // 通过工厂获得类的实例
        UserService userService = (UserService) applicationContext.getBean("userService");
        userService.sayHello();
    }
```

## 三种实例化 Spring Bean 的方式

1. 一般来说用的都是无参数构造方法的方式
2，项目特别复杂时会使用静态或者实例工厂的方式

### 1. 无参数构造方法的方式

```xml
    <!-- 配置 Bean-->
    <!--    1. 无参数构造方法的方式 -->
    <bean id="bean1" class="com.imooc.ioc.demo2.Bean1"/>
```

```java
    // 配置无参数构造方法
    /**
     * Bean 的实例化的三种方式：采用无参数的构造方法的方式
     */
    public class Bean1 {
        public Bean1() {
            System.out.println("Bean1 被实例化了...");
        }
    }

    public class SpringDemo1 {
        @Test
        /**
         * 1. 无参数构造方法的方式
         */
        public void demo1() {
            // 读取配置文件，创建工厂
            ApplicationContext applicationContext = new ClassPathXmlApplicationContext("applicationContext.xml");
            // 通过 Spring 工厂获得类的实例
            Bean1 bean1 = (Bean1) applicationContext.getBean("bean1");
        }
    }
```

### 2. 静态工厂的方式 (简单工厂)

需要注意的是 Bean 配置中多了一项 factory-method，用来指定工厂中创建对象的方法

```xml
    <!-- 配置 Bean-->
    <!--    2. 静态工厂的方式 -->
    <bean id="bean2" class="com.imooc.ioc.demo2.Bean2Factory" factory-method="createBean2"/>
```

```java
    // 配置静态工厂
    /**
     * Bean2 的静态工厂
     */
    public class Bean2Factory {
        public static Bean2 createBean2() {
            System.out.println("Bean2Factory 的 createBean2 方法已经执行了...");
            return new Bean2();
        }
    }

    public class SpringDemo1 {
        @Test
        /**
         * 2. 静态工厂的方式
         */
        public void demo2() {
            // 读取配置文件，创建工厂
            ApplicationContext applicationContext = new ClassPathXmlApplicationContext("applicationContext.xml");
            // 通过 Spring 工厂获得类的实例
            Bean2 bean2 = (Bean2) applicationContext.getBean("bean2");
        }
    }
```

### 3. 实例工厂的方式

1. 静态工厂和实例工厂区别就是静态工厂用来创建对象的方法是静态的，而实例工厂不是静态的
2. 配置 bean 的时候，需要先配置工厂的实例，然后再配置类的实例

```xml
    <!-- 配置 Bean-->
    <!--    3. 实例工厂的方式 -->
    <!--先配置工厂的实例-->
    <bean id="bean3Factory" class="com.imooc.ioc.demo2.Bean3Factory" />
    <!--再配置类的实例-->
    <bean id="bean3" factory-bean="bean3Factory" factory-method="craeteBean3" />
```

```java
    // 配置实例工厂
    /**
     * Bean3的实例工厂
     */
    public class Bean3Factory {
        public Bean3 createBean3() {
            System.out.println("Bean3Factory中的createBean3方法已经执行了...");
            return new Bean3();
        }
    }

    public class SpringDemo1 {
        @Test
        /**
         * 3. 实例工厂的方式
         */
        public void demo3() {
            // 读取配置文件，创建工厂
            ApplicationContext applicationContext = new ClassPathXmlApplicationContext("applicationContext.xml");
            // 通过Spring工厂获得类的实例
            Bean3 bean3 = (Bean3) applicationContext.getBean("bean3");
        }
    }
```
 