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

## Spring 的属性注入 - 注解方式

1. 注解的方式为简单类型的属性注入值，可以没有 set 方法
    > 如果属性提供了 set 方法，注解需要写在 set 方法上

    ```java
    package com.imooc.demo1;

    import org.springframework.beans.factory.annotation.Value;
    import org.springframework.stereotype.Service;

    /**
     * Spring 的 Bean 管理的注解方式：
     * * 传统方式需要去 xml 中配置 <bean id=""class=""></bean>
     */
    @Service("userService")
    public class UserService {
        // Spring 注解的方式属性注入，没有 set 方法
        // 如果属性提供了 set 方法，注解需要写在 set 方法上
        @Value("米饭")
        private String something;
        public String sayHello(String name) {
            return "Hello" +name;
        }

        public void eat() {
            System.out.println("eat:"+something);
        }
    }

    ```

2. 使用 @Autowired 进行自动注入
    > 1. 默认按照类型进行注入，如果存在两个相同 Bean 类型相同，则按名称注入
    > 2. 注入时可以针对成员变量或者 set 方法
    > 3. 通过 @Autowired 的 required 属性，设置一定要找到匹配的 Bean

    ```java
        @Autowired
        private UserDao userDao;
    ```

3. 使用 @Qualifier 指定注入 Bean 的名称
    > 1. 使用 @Qualifier("Bean 名称") 强制指定要注入的 Bean 的名称

    ```java
        @Autowired
        @Qualifier("Bean 名称")
        private UserDao userDao;
    ```

4. 推荐使用 @Resource，相当于 @Autowired 和 @Qualifier 的组合
    > 1. @Resource 和 @Autowire 注解功能相似

    ```java
        @Resource(name="Bean 名称")
        private UserDao userDao;
    ```

## Spring 的其他注解

1. Spring 在初始化 Bean 或者销毁 Bean 时，可以调用 Bean 的两个生命周期方法

2. 当 Bean 被载入到容器的时候会调用 init 方法
    > 注解方式：@PostConstruct
    > 初始化

3. 当 Bean 从容器中删除调用 destory 方法，仅当 scope 为 singleton 有效
    > 注解方式：@PreDestory
    > 销毁

4. @Scope 注解用于指定 Bean 的作用范围
    > 使用注解配置的 Bean 和 ```<bean>``` 配置的一样，默认作用域都是 singleton

## 传统 XML 配置和注解配置混合使用

1. XML 的优势在于：结构清晰，易于阅读
    > XML 用于管理类
2. 注解的优势在于：开发便捷，属性注入方便
    > 注解用于完成属性注入
3. XML 和注解的混合开发：
    > 1. 引入 context 命名空间
    > 2. 在配置文件中添加 context:annotation-config 标签
4. context:annotation-config 标签是单独开启属性注入，只能使用属性注入
    > ```<context:component-scan base-package="com.imooc" />``` 这是开启了包扫描，包括了属性注入，可以使用各种注解 (@Serivce、@Repistory 等)

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

## Spring AOP 的学习

### AOP 基本概念

1. Aspect Oriented Programming

2. 面向切面编程，是 OOP 的延续，是 Spring 框架中的重要内容

3. 利用 AOP 可以对程序的各个部分进行隔离，从而使得业务逻辑部分之间的耦合度降低，提高程序的可重用性，同时提高了开发效率

4. AOP 采取了横向抽取机制，取代了传统纵向继承体系的重复性代码

5. 使用 AOP 可以进行：性能监视、事务管理、安全检查、缓存

6. Spring AOP 使用存 Java 实现，不需要专门的编译过程和类加载器，在运行期间通过代理方式向目标类织入增强代码

### AOP 相关术语

1. Joinpoint(连接点)：所谓连接点是指那些被拦截到的点。在 Spring 中，这些点指的是方法，因为 Spring 只支持方法类型的连接点

2. Pointcut(切入点)：所谓切入点是指我们要对哪些 Joinpoint 进行拦截的定义

3. Advice(通知 / 增强)：所谓通知是指拦截到 Joinpoint 之后所要做的事情，这就是通知
    > 通知分为：前置通知、后置通知、异常通知、最终通知、环绕通知（切面要完成的功能）

4. Introductio(引介)：是一种特殊的通知，在不修改类代码的前提下，Introduction 可以在运行期间为类动态地添加一些方法或 Field，（暂未学习）

5. Target(目标对象)：代理的目标对象，对该目标进行增强

6. Weaving(织入)：是指把增强应用到目标对象来创建新的代理对象的过程
    > Spring 采用动态代理织入
    > AspectJ 采用编译期织入和类装载期织入

7. Proxy(代理)：一个类被 AOP 织入增强后，就产生一个结果代理类

8. Aspect(切面)：是切入点和通知 (引介) 的结合

## 两个动态代理的底层实现（做个记录，应该使用 Spring 提供的更方便的配置文件形式的动态代理）

### 1. JDK 动态代理的底层实现原理

JDK 代理只能对实现接口的类产生代理

1. UserDao
    > UserDao 接口

    ```java
    package com.imooc.demo1;

    public interface UserDao {
        public void save();

        public void update();

        public void delete();

        public void find();
    }

    ```

2. UserDaoImpl
    > UserDaoImpl UserDao 接口的实现

    ```java
        package com.imooc.demo1;

        public class UserDaoImpl implements UserDao {

            @Override
            public void save() {
                System.out.println("保存用户...");
            }

            @Override
            public void update() {
                System.out.println("修改用户...");
            }

            @Override
            public void delete() {
                System.out.println("删除用户...");
            }

            @Override
            public void find() {
                System.out.println("查询用户...");
            }
        }

    ```

3. MyJdkProxy
    > 代理类

    ```java
        package com.imooc.demo1;

        import java.lang.reflect.InvocationHandler;
        import java.lang.reflect.Method;
        import java.lang.reflect.Proxy;

        public class MyJdkProxy implements InvocationHandler {

            private UserDao userDao;

            public MyJdkProxy(UserDao userDao) {
                this.userDao = userDao;
            }

            public Object createProxy() {
                Object proxy = Proxy.newProxyInstance(userDao.getClass().getClassLoader(), userDao.getClass().getInterfaces(), this);
                return proxy;
            }

            @Override
            public Object invoke(Object proxy, Method method, Object[] args) throws Throwable {
                if ("save".equals(method.getName())){
                    System.out.println("权限校验...");
                    return method.invoke(userDao,args);
                }
                return method.invoke(userDao,args);
            }
        }

    ```

4. SpringDemo1
    > 测试代码运行类

    ```java
        package com.imooc.demo1;

        import org.junit.Test;

        public class SpringDemo1 {
            @Test
            public void demo1() {
                UserDao userDao = new UserDaoImpl();

                UserDao proxy = (UserDao) new MyJdkProxy(userDao).createProxy();

                proxy.save();
                proxy.update();
                proxy.delete();
                proxy.find();
            }
        }

    ```

### 2. 使用 CGLIB 生成代理

如果类没有实现接口，可以用 CGLIB 生成代理

对于不使用接口的业务类，无法使用 JDK 动态代理

CGLIB 采用底层的字节码技术，可以为一个类创建子类，解决无接口代理问题

1. 下面是 CGLIB 生成代理的底层实现（需要配置 pom.xml，引入 Spring 的 context，beans，core，expression）

2. ProductDao

    ```java
        package com.imooc.aop.demo2;

        public class ProductDao {
            public void save() {
                System.out.println("保存商品");
            }
            public void update() {
                System.out.println("修改商品");
            }
            public void delete() {
                System.out.println("删除商品");
            }
            public void find() {
                System.out.println("查询商品");
            }
        }

    ```

3. MyCglibProxy

    ```java
        package com.imooc.aop.demo2;

        import org.springframework.cglib.proxy.Enhancer;
        import org.springframework.cglib.proxy.MethodInterceptor;
        import org.springframework.cglib.proxy.MethodProxy;

        import java.lang.reflect.Method;

        public class MyCglibProxy implements MethodInterceptor {
            private ProductDao productDao;

            public MyCglibProxy(ProductDao productDao) {
                this.productDao = productDao;
            }

            public Object createProxy() {
                // 1. 创建核心类
                Enhancer enhancer = new Enhancer();
                // 2. 设置父类
                enhancer.setSuperclass(productDao.getClass());
                // 3. 设置回调
                enhancer.setCallback(this);
                // 4. 生成代理
                Object proxy = enhancer.create();
                return proxy;
            }

            @Override
            public Object intercept(Object proxy, Method method, Object[] args, MethodProxy methodProxy) throws Throwable {
                if ("save".equals(method.getName())){
                    System.out.println("权限校验...====>");
                    return methodProxy.invokeSuper(proxy,args);
                }
                return methodProxy.invokeSuper(proxy,args);
            }
        }

    ```

4. SpringDemo2

    ```java
        package com.imooc.aop.demo2;

        import org.junit.Test;

        public class SpringDemo2 {
            @Test
            public void demo1() {
                ProductDao productDao = new ProductDao();

                ProductDao proxy = (ProductDao) new MyCglibProxy(productDao).createProxy();
                proxy.save();
                proxy.update();
                proxy.delete();
                proxy.find();
            }
        }

    ```

### 3. 代理的总结

1. Spring 在运行期间，生成动态代理对象，不需要特殊的编译器

2. Spring AOP 的底层就是通过 JDK 的动态代理或者 CGlib 动态代理技术为目标 Bean 执行横向织入
    > 1. 若目标对象实现了若干接口，Spring 使用 JDK 的 java.lang.reflect.Proxy 类代理
    > 2. 若目标对象没有实现任何接口，Spring 使用 CGlib 库生成目标对象的子类

3. 程序中应该优先使用接口创建代理，便于程序解耦维护

4. 标记为 final 的方法，不能被代理，因为无法进行覆盖（重写）
    > 1. JDK 动态代理，是针对接口生成子类，接口中方法不能使用 fianl 修饰
    > 2. CGlib 是针对目标类生产子类，因此类或方法不能使用 final 的

5. Spring 只支持方法连接点，不提供属性连接点
    > 也就是说，Spring 只支持对方法层面进行增强

## Spring AOP 增强类型

1. AOP 联盟为通知 Advice 定义了 org.aopalliance.aop.Interface.Advice

2. Spring 按照通知 Advice 在目标类方法的连接点位置，可以分为 5 类
    > 1. 前置通知 org.springframework.aop.MethodBeforeAdvice
        > 在目标方法执行前实施增强
    > 2. 后置通知 org.springframework.aop.AfterReturningAdvice
        > 在目标方法执行后实施增强
    > 3. 环绕通知 org.aopalliance.intercept.MethodInterceptor
        > 在目标方法执行前后都实施增强
    > 4. 异常抛出通知 org.springframework.aop.ThrowsAdvice
        > 在方法抛出异常后实施通知
    > 5. 引介通知 org.springframework.aop.IntroductionInterceptor
        > 在目标类中添加一些新的方法和属性 (暂未学习)

### Spring AOP 切面类型

1. Advisor：代表一般切面，Advice 本身就是一个切面，对目标类所有的方法进行拦截
    > 1. 可以使用 ```<property name="optimize" value="true"/>``` 强制使用 CGLIB 动态代理
    > ```<property name="proxyTargetClass" value="true"/>``` 是否对类代理而不是接口，设置为 true 时，使用 CGLib 代理
    > 2. 可以使用 ```<property name="proxyInterfaces" value="myBeforeAdvice"/>``` 设置需要织入目标的 Advice
    > 3. 可以使用 ```<property name="singleton"/>``` 设置返回代理是否为单例，默认单例
    > 4. 一般切面（对所有方法进行拦截）的案例：

    ```xml
    <?xml version="1.0" encoding="UTF-8"?>
    <beans xmlns="http://www.springframework.org/schema/beans"
        xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
        xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">

        <!-- 配置目标类 -->
        <bean id="studentDao" class="com.imooc.aop.demo3.StudentDaoImpl"/>

        <!-- 前置通知类型 -->
        <bean id="myBeforeAdvice" class="com.imooc.aop.demo3.MyBeforeAdvice"/>


        <!--Spring 的 AOP 产生代理对象 -->
        <bean id="studentDaoProxy" class="org.springframework.aop.framework.ProxyFactoryBean">
            <!-- 配置目标类 -->
            <property name="target" ref="studentDao"/>
            <!-- 实现的接口 -->
            <property name="proxyInterfaces" value="com.imooc.aop.demo3.StudentDao"/>
            <!-- 采用拦截的名称 -->
            <property name="interceptorNames" value="myBeforeAdvice"/>
        </bean>
    </beans>
    ```

    ```java
        // 前置通知
        package com.imooc.aop.demo3;

        import org.springframework.aop.MethodBeforeAdvice;

        import java.lang.reflect.Method;

        public class MyBeforeAdvice implements MethodBeforeAdvice {
            @Override
            public void before(Method method, Object[] args, Object target) throws Throwable {
                System.out.println("这是前置增强 / 通知 =======================");
            }
        }
    ```

    ```java
        // Dao 层
        package com.imooc.aop.demo3;

        public interface StudentDao {
            public void find();

            public void save();

            public void update();

            public void delete();
        }

    ```

    ```java
        // Dao 接口的实现
        package com.imooc.aop.demo3;

        public class StudentDaoImpl implements StudentDao {
            @Override
            public void find() {
                System.out.println("学生查询...");
            }

            @Override
            public void save() {
                System.out.println("学生保存...");

            }

            @Override
            public void update() {
                System.out.println("学生修改...");

            }

            @Override
            public void delete() {
                System.out.println("学生删除...");

            }
        }

    ```

    ```java
        // 测试类
        package com.imooc.aop.demo3;

        import org.junit.Test;
        import org.junit.runner.RunWith;
        import org.springframework.test.context.ContextConfiguration;
        import org.springframework.test.context.junit4.SpringJUnit4ClassRunner;

        import javax.annotation.Resource;

        @RunWith(SpringJUnit4ClassRunner.class)
        @ContextConfiguration("classpath:applicationContext.xml")
        public class SpringDemo3 {

            @Resource(name="studentDaoProxy")
            private StudentDao studentDao;

            @Test
            public void demo1() {
                studentDao.find();
                studentDao.save();
                studentDao.update();
                studentDao.delete();
            }
        }

    ```

2. PointcutAdvisor：代表具有切点的切面，可以指定拦截目标类哪些方法
    > 使用普通的 Advice 作为切面，将对目标类所有方法进行拦截，不够灵活，在实际开发中经常采用带有切点的切面
    > 常用的 PointcutAdvisor 实现类：
        > 1. DefaultPointcutAdvisor 最常用的切面类型，它可以通过 Pointcut 和 Advice 组合定义切面
        > 2. JdkRegexpMethodPointcut 构造正则表达式切点 (和 AspectJ 类似，是 AspectJ 的基础)
        > 3. 案例：

    ```xml
        <?xml version="1.0" encoding="UTF-8"?>
        <beans xmlns="http://www.springframework.org/schema/beans"
            xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
            xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">

            <!-- 目标类 -->
            <bean id="customerDao" class="com.imooc.aop.Demo4.CustomerDao"/>

            <!-- 通知类 环绕通知 -->
            <bean id="myAroundAdvice" class="com.imooc.aop.Demo4.MyAroundAdvice"/>

            <!-- 一般切面，使用通知作为切面，因为要对目标类的某个方法进行增强，所以需要配置一个带有切入点的切面 -->
            <bean id="myAdvisor" class="org.springframework.aop.support.RegexpMethodPointcutAdvisor">

                <!--pattern 中配置正则表达式，. 代表任意字符，* 带表任意次数 -->
                <!--<property name="pattern" value=".*save.*" />-->
                <property name="patterns" value=".*save.*,.*delete.*"/>
                <property name="advice" ref="myAroundAdvice" />
            </bean>

            <!-- 配置产生代理 -->
            <bean id="customerDaoProxy" class="org.springframework.aop.framework.ProxyFactoryBean">
                <!-- 配置目标类 -->
                <property name="target" ref="customerDao"/>
                <!-- 配置对类进行代理 -->
                <property name="proxyTargetClass" value="true"/>
                <!-- 指定要在代理的目标类插入的 Advice（通知）-->
                <property name="interceptorNames" value="myAdvisor" />
            </bean>
        </beans>
    ```

    ```java
        // Dao 层
        package com.imooc.aop.demo3;

        public interface StudentDao {
            public void find();

            public void save();

            public void update();

            public void delete();
        }

    ```

    ```java
        // Dao 层的实现
        package com.imooc.aop.demo3;

        public class StudentDaoImpl implements StudentDao {
            @Override
            public void find() {
                System.out.println("学生查询...");
            }

            @Override
            public void save() {
                System.out.println("学生保存...");

            }

            @Override
            public void update() {
                System.out.println("学生修改...");

            }

            @Override
            public void delete() {
                System.out.println("学生删除...");

            }
        }

    ```

    ```java
        // 通知类
        package com.imooc.aop.demo3;

        import org.springframework.aop.MethodBeforeAdvice;

        import java.lang.reflect.Method;

        public class MyBeforeAdvice implements MethodBeforeAdvice {
            @Override
            public void before(Method method, Object[] args, Object target) throws Throwable {
                System.out.println("这是前置增强 / 通知 =======================");
            }
        }

    ```

    ```java
        // Demo
        package com.imooc.aop.demo3;

        import org.junit.Test;
        import org.junit.runner.RunWith;
        import org.springframework.test.context.ContextConfiguration;
        import org.springframework.test.context.junit4.SpringJUnit4ClassRunner;

        import javax.annotation.Resource;

        @RunWith(SpringJUnit4ClassRunner.class)
        @ContextConfiguration("classpath:applicationContext.xml")
        public class SpringDemo3 {

            @Resource(name="studentDaoProxy")
            private StudentDao studentDao;

            @Test
            public void demo1() {
                studentDao.find();
                studentDao.save();
                studentDao.update();
                studentDao.delete();
            }
        }
    ```

3. IntroductionAdvisor：代表引介切面，针对引介通知而使用切面（暂未学习)

### 自动创建代理

前面的两个代理的案例，每个代理都是通过 ProxyFactoryBean 织入切面进行代理，在实际开发中，非常多的 Bean，每个都要配置 ProxyFactoryBean，开发维护量巨大

解决方法：自动创建代理

1. BeanNameAutoProxyCreator 根据 Bean 名称创建代理
    > 缺点是只能应用到某个类上，不能具体到方法

    ```xml
        <?xml version="1.0" encoding="UTF-8"?>
        <beans xmlns="http://www.springframework.org/schema/beans"
            xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
            xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">
            <!-- 配置目标类 -->
            <bean id="studentDao" class="com.imooc.aop.demo5.StudentDaoImpl" />
            <bean id="customerDao" class="com.imooc.aop.demo5.CustomerDao"/>

            <!-- 配置通知 / 增强 -->
            <bean id="myBeforeAdvice" class="com.imooc.aop.demo5.MyBeforeAdvice" />
            <bean id="myAroundAdvice" class="com.imooc.aop.demo5.MyAroundAdvice" />

            <bean class="org.springframework.aop.framework.autoproxy.BeanNameAutoProxyCreator">
                <!-- 配置需要增强的 Bean 的名称 -->
                <property name="beanNames" value="*Dao"/>
                <!-- 配置织入目标 (增强目标类) 的 Bean 的名称 -->
                <property name="interceptorNames" value="myBeforeAdvice"/>
            </bean>
        </beans>
    ```

    ```java
        // 通知类
        package com.imooc.aop.demo5;

        import org.springframework.aop.MethodBeforeAdvice;

        import java.lang.reflect.Method;

        public class MyBeforeAdvice implements MethodBeforeAdvice {
            @Override
            public void before(Method method, Object[] args, Object target) throws Throwable {
                System.out.println("这是前置增强 / 通知 =======================");
            }
        }
    ```

    ```java
        // CustomerDao 层（无接口）
        package com.imooc.aop.demo5;

        public class CustomerDao {
            public void find() {
                System.out.println("查询客户...");
            }
            public void save() {
                System.out.println("保存客户...");
            }
            public void update() {
                System.out.println("修改客户...");
            }
            public void delete() {
                System.out.println("删除客户...");
            }
        }

    // StudentDao 层（接口）
    package com.imooc.aop.demo5;

    public interface StudentDao {
        public void find();

        public void save();

        public void update();

        public void delete();
    }
    ```

    ```java
        // StudentDao 接口的实现类
        package com.imooc.aop.demo5;

        public class StudentDaoImpl implements StudentDao {
            @Override
            public void find() {
                System.out.println("学生查询...");
            }

            @Override
            public void save() {
                System.out.println("学生保存...");

            }

            @Override
            public void update() {
                System.out.println("学生修改...");

            }

            @Override
            public void delete() {
                System.out.println("学生删除...");

            }
        }

    ```

    ```java
        // Demo
        package com.imooc.aop.demo5;

        import org.junit.Test;
        import org.junit.runner.RunWith;
        import org.springframework.test.context.ContextConfiguration;
        import org.springframework.test.context.junit4.SpringJUnit4ClassRunner;

        import javax.annotation.Resource;

        @RunWith(SpringJUnit4ClassRunner.class)
        @ContextConfiguration("classpath:applicationContext3.xml")
        public class SpringDemo5 {
            @Resource(name = "studentDao")
            private StudentDao studentDao;
            @Resource(name = "customerDao")
            private CustomerDao customerDao;

            @Test
            public void demo1() {
                studentDao.find();
                studentDao.save();
                studentDao.update();
                studentDao.delete();

                customerDao.find();
                customerDao.save();
                customerDao.update();
                customerDao.delete();

            }
        }

    ```

2. DefaultAdvisorAutoProxyCreator 根据 Advisor 本身包含的信息创建代理
    > 其他的和 1 里面差不多

    ```xml
        <?xml version="1.0" encoding="UTF-8"?>
        <beans xmlns="http://www.springframework.org/schema/beans"
            xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
            xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">
            <!-- 配置目标类 -->
            <bean id="studentDao" class="com.imooc.aop.demo6.StudentDaoImpl" />
            <bean id="customerDao" class="com.imooc.aop.demo6.CustomerDao"/>

            <!-- 配置通知 / 增强 -->
            <bean id="myBeforeAdvice" class="com.imooc.aop.demo5.MyBeforeAdvice" />
            <bean id="myAroundAdvice" class="com.imooc.aop.demo5.MyAroundAdvice" />

            <!-- 配置切面 -->
            <bean id="myAdvisor" class="org.springframework.aop.support.RegexpMethodPointcutAdvisor">
                <property name="pattern" value="com\.imooc\.aop\.demo6\.CustomerDao\.save" />
                <property name="advice" ref="myAroundAdvice"/>
            </bean>

            <bean class="org.springframework.aop.framework.autoproxy.DefaultAdvisorAutoProxyCreator" />
        </beans>
    ```

3. AnnotationAwareAspectJAutoProxyCreator 基于 Bean 中的 ASpectJ 注解进行自动代理

## 使用 AspectJ 实现 (以后的 AOP 开发基本上就使用这种方式了)(重点)

1. 注解方式

2. XML 方式

3. 以上两种方式在开发中都会经常使用

### AspectJ 简介

1. AspectJ 是一个基于 Java 语言的 AOP 框架

2. SPring2.0 以后就集成了 AspectJ

3. @AspectJ 是 AspectJ1.5 新增功能，通过 JDK5 注解技术，允许直接在 Bean 类中定义切面

4. 新版 Spring 框架，建议使用 AspectJ 方式来开发 AOP

5. 使用 AspectJ 需要导入 SpringAOP 和 AspectJ 相关的 Jar 包

    ```xml
    <!--AspectJ注解开发，自动代理需要的依赖：pom.xml-->
    <dependencies>
        <dependency>
            <groupId>junit</groupId>
            <artifactId>junit</artifactId>
            <version>4.11</version>
        </dependency>
            <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-core</artifactId>
            <version>4.2.4.RELEASE</version>
        </dependency>
        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-context</artifactId>
            <version>4.2.4.RELEASE</version>
        </dependency>
        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-beans</artifactId>
            <version>4.2.4.RELEASE</version>
        </dependency>
        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-expression</artifactId>
            <version>4.2.4.RELEASE</version>
        </dependency>
        <dependency>
            <groupId>aopalliance</groupId>
            <artifactId>aopalliance</artifactId>
            <version>1.0</version>
        </dependency>
        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-aop</artifactId>
            <version>4.2.4.RELEASE</version>
        </dependency>
        <dependency>
            <groupId>org.aspectj</groupId>
            <artifactId>aspectjweaver</artifactId>
            <version>1.8.9</version>
        </dependency>
        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-aspects</artifactId>
            <version>4.2.4.RELEASE</version>
        </dependency>
        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-test</artifactId>
            <version>4.2.4.RELEASE</version>
        </dependency>
    </dependencies>

    <!--AspectJ注解开发，自动代理需要的配置文件：applicationContext.xml-->
    <!--可以复制spring-framework框架内的的xsd-configuration.html中aop相关的配置:the aop schema-->
    <?xml version="1.0" encoding="UTF-8"?>
        <beans xmlns="http://www.springframework.org/schema/beans"
            xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
            xmlns:aop="http://www.springframework.org/schema/aop" xsi:schemaLocation="
                http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd
                http://www.springframework.org/schema/aop http://www.springframework.org/schema/aop/spring-aop.xsd"> <!-- bean definitions here -->
        <!--开启AspectJ注解开发，自动代理-->
        <aop:aspectj-autoproxy />
        </beans>
    ```

### AspectJ不同的通知类型

1. @Before前置通知，相当于BeforeAdvice

2. @AfterRetunning后置通知，相当于AfterRetunningAdvice

3. @Around环绕通知，相当于MethodInterceptor

4. @AfterThrowing异常抛出通知，相当于ThrowAdvice

5. @After最终final通知，不论是否异常，该通知都会执行

6. @DeclareParents引介通知，相当于IntroductionInterceptor(暂未学习)

### 通过execution函数，可以定义切点的方法切入

#### 语法

1. execution(<访问修饰符 - 可选>?<返回类型><方法名><(参数)><异常 - 可选>)

#### 案例

1. 匹配所有类public方法 (..)代表任意参数
    > excution(public * *(..))

2. 匹配指定包下所有类方法，不包含子包
    > excution(* com.imooc.dao.*(..))

3. 匹配指定包下所有类方法，包含子包(包含子孙包下所有类)
    > excution(* com.imooc.dao..*(..))

4. 匹配指定类所有方法
    > excution(* com.imooc.service.UserService.*(..))

5. 匹配实现特定接口所有类方法(+代表子类)
    > excution(* com.imooc.dao.GenericDao+.*(..))

6. 匹配所有save开头的方法
    > excution(* save* (..))
