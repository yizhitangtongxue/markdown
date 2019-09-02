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
2. Bean 的 id 属性和 name 属性作用一样，用来作为 Bean 的名称，id 和 name 属性在 IOC 容器中必须是唯一的
3. Bean 的名称中如果有特殊字符，就需要使用 name 属性
4. Bean 的 class 属性，用于设置一个类的完全路径名称，主要作用是 IOC 容器生成类的实例
5. 配置 Bean，将对象的创建权交给 Spring，实现控制反转
6. Bean 的作用域默认是 singleton，即 Bean 以单例模式存在，一个 IOC 容器中只有一个 Bean 实例
    > 通过配置 bean 标签的 scope 属性的值为 prototype，可以让我们每次在 getBean 的时候都重新生成一个新的对象
    > 多例模式一般用在 Spring 整合 struct2 的时候，struct2 的 action 是多例的，struct2 要交给 Spring 管理，这时 bean 标签的 scope 值就要设置成 prototype

## Bean 的生命周期

1. Spring 帮我们实例化对象的时候，一共经历了 11 个过程，其中第 5 和第 8 步最重要
    > 1. 实例化类
    > 2. 封装属性
    > 3. 如果 Bean 实现 BeanNameAware 执行 setBeanName | 设置 Bean 的名称
    > 4. 如果 Bean 实现 BeanFactoryName 或者 ApplicationContextAware 设置工厂 setBeanFactory 或者上下文对象 setApplicationContext | 了解工厂的信息
    > 5. 如果存在类实现 BeanPostProcessor(后处理类)，执行 postProcessBeforeinitialization | 初始化前方法
    > 6. 如果 Bean 实现 InitializingBean，执行 afterPropertiesSet | 属性设置后执行的方法
    > 7. 调用 ```<bean init-method="init">```, 指定初始化方法
    > 8. 如果存在类实现 BeanPostProcessor(处理 Bean)，执行 postProcessAfterInitialization | 初始化后方法，至此对象初始化完成
    > 9. 执行业务逻辑方法，权限校验应该在 8 执行
    > 10. 如果 Bean 实现 DisposableBean，执行 destory 方法 | 执行 Spring 的销毁方法
    > 11. 调用 ```<bean destory-method="customerDestory">```, 指定销毁方法
2. Spring 初始化 bean 或者销毁 bean 时，可以调用 bean 的两个生命周期方法
    > 1. init-method 当 bean 被载入到容器时调用
    > 2. destory-method 当 bean 从容器中删除的时候调用 (scope 为 singleton 时有效)
3. 想调用 destory-method，需要使用 ClassPathXmlApplicationContext.close() 方法

## IOC 和 DI

1. IOC Inverse of Control 即控制反转
    > 将原本在程序中手动创建对象的控制权，交给了 Spring 框架管理
    > 简单的说，就是创建对象的控制权被反转到了 Spring 框架
2. DI Dependency Injection 即依赖注入
    > 依赖注入的前提是控制反转
    > 在 Spring 创建整个对象的过程中，将对象所依赖的属性注入进去
3. [IOC 和 DI 基础](https://www.iteye.com/blog/jinnianshilongnian-1413846)

## 几种属性的依赖注入方式

### 1. 构造方法

1. 构造方法属性依赖注入，无需为属性设置 set 方法

2. 构造方法属性依赖注入，需要 constructor-arg 标签，name 属性为属性的名称，value 属性为属性的值

```xml
    <bean id="user" class="com.imooc.ioc.demo4.User">
        <constructor-arg name="name" value="张三"/>
        <constructor-arg name="age" value="23"/>
    </bean>
```

### 2. set 方法 比较习惯此方法

1. 在配置文件中，通过 property 标签注入属性

2. 需要在类中为属性设置 set 方法，没有 set 方法，在 Spring 配置文件中会报错

3. 通过在 Spring 配置文件中，可以设置类的各种类型的属性，比如对象类型
    > 1. 当类的属性为对象类型时，需要先为此对象配置 bean
    > 2. 当类的属性为对象类型时，Spring 配置中的 property 属性的 name 值为类的名称，使用 ref 代表类的 beanid
    > 3. 当类的属性为对象类型时，需要为该对象配置 set 方法

```xml
    <bean id="person" class="com.imooc.ioc.demo4.Person">
        <property name="name" value="李四" />
        <property name="age" value="32" />
    </bean>

    <bean id="cat" class="com.imooc.ioc.demo4.Cat">
        <property name="name" value="ketty"/>
    </bean>
```

```java
package com.imooc.ioc.demo4;

public class Person {
    private String name;
    private Integer age;
    private Cat cat;

    public void setCat(Cat cat) {
        this.cat = cat;
    }

    public void setName(String name) {
        this.name = name;
    }


    public void setAge(Integer age) {
        this.age = age;
    }


    @Override
    public String toString() {
        return "Person{" +
                "name='" + name + '\'' +
                ", age=" + age +
                '}';
    }
}
```

### 3. p 命名空间注入属性

1. 首先需要在 Spring 的配置文件中写入 ```xmlns:p="http://www.springframework.org/schema/p"```

2. 使用 p: 属性名称 = 属性值注入属性

3. 属性类型为对象的话，应该使用 p:beanid-ref="beanid"

```xml
    <!--p 命名空间的方式注入属性 -->
    <bean id="person" class="com.imooc.ioc.demo4.Person" p:name="李四" p:age="32" p:cat-ref="cat" />

    <bean id="cat" class="com.imooc.ioc.demo4.Cat" p:name="ketty"/>
```

### 4. SpEL Spring Expression Language 注入属性

1. #{'使用字符串'}

2. #{beanId} 使用另一个 Bean

3. #{beanId.context.toUpperCase()} 使用指定属性，并使用方法

4. #{T(java.lang.Math).PI} 使用静态字段或者方法

### 5. 复杂类型属性注入

> 主要用于 Spring 整合其他框架

1. 数组类型的属性注入

2. List 集合类型的属性注入

3. Set 集合类型的属性注入

4. Map 集合类型的属性注入

5. Properties 类型的属性注入

```xml
 <!-- 复杂类型的属性注入 -->
    <bean id="collectionBean" class="com.imooc.ioc.demo5.CollectionBean">
        <!-- 数组类型的属性注入 -->
        <property name="arrs">
            <list>
                <value>aaa</value>
                <value>bbb</value>
                <value>ccc</value>
            </list>
        </property>
        <!-- 数组类型的属性注入 -->
        <property name="list">
            <list>
                <value>111</value>
                <value>222</value>
                <value>333</value>
            </list>
        </property>
        <!-- Set 类型的属性注入 -->
        <property name="set">
            <set>
                <value>ddd</value>
                <value>eee</value>
                <value>fff</value>
            </set>
        </property>
        <!-- Map 类型的属性注入 -->
        <property name="map">
            <map>
                <entry key="aaa" value="111" />
                <entry key="bbb" value="222" />
                <entry key="ccc" value="333" />
            </map>
        </property>
        <!-- Properties 类型的属性注入 -->
        <property name="properties">
            <props>
                <prop key="username">root</prop>
                <prop key="password">1234</prop>
            </props>
        </property>
    </bean>
```

## 使用注解定义 Bean

1. Spring2.5 引入注解定义 Bean
    > 1. 使用注解定义 Bean，首先需要在 pom.xml 中引入 spring-aop
    > 2. 在 pom.xml 内定义如下

    ```xml
    <?xml version="1.0" encoding="UTF-8"?>
    <beans xmlns="http://www.springframework.org/schema/beans"
        xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
        xsi:schemaLocation="
            http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">
        <!-- 开启注解扫描 -->
        <!-- 注解扫描的作用就是去每个包的类上面去找有没有某个注解，如果有注解，就交给 Spring 管理 -->
        <context:component-scan base-package="com.imooc.demo1" />
    </beans>
    ```

2. @Component 描述 Spring 框架中的 Bean

3. 除了 @Component 之外，还有三个基本功能和 @Component 等效的注解:
    > 1. @Repository 用于对 Dao 实现类进行标记
    > 2. @Service 用于对 Service 实现类进行标记
    > 3. @Controller 用于对 Controller 实现类进行标记

## 控制反转和依赖注入的简单演示

```xml
<!-- 将对象的创建权交给了 Spring -->
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
2. 项目特别复杂时会使用静态或者实例工厂的方式

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
    <!-- 先配置工厂的实例 -->
    <bean id="bean3Factory" class="com.imooc.ioc.demo2.Bean3Factory" />
    <!-- 再配置类的实例 -->
    <bean id="bean3" factory-bean="bean3Factory" factory-method="craeteBean3" />
```

```java
    // 配置实例工厂
    /**
     * Bean3 的实例工厂
     */
    public class Bean3Factory {
        public Bean3 createBean3() {
            System.out.println("Bean3Factory 中的 createBean3 方法已经执行了...");
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
            // 通过 Spring 工厂获得类的实例
            Bean3 bean3 = (Bean3) applicationContext.getBean("bean3");
        }
    }
```
