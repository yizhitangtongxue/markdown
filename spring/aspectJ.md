# Aspect

## 基本配置

```xml
    <!--applicationContext.xml-->
    <?xml version="1.0" encoding="UTF-8"?>
    <beans xmlns="http://www.springframework.org/schema/beans"
        xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
        xmlns:aop="http://www.springframework.org/schema/aop" xsi:schemaLocation="
            http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd
            http://www.springframework.org/schema/aop http://www.springframework.org/schema/aop/spring-aop.xsd"> <!-- bean definitions here -->
        <!-- 开启 AspectJ 注解开发，自动代理 -->
        <aop:aspectj-autoproxy/>
        <!-- 目标类 -->
        <bean id="productDao" class="com.imooc.aspectJ.demo1.ProductDao"/>
        <!-- 切面类 -->
        <bean class="com.imooc.aspectJ.demo1.MyAspectAnno"/>
    </beans>
```

```java
    package com.imooc.aspectJ.demo1;

    import org.aspectj.lang.annotation.Aspect;
    import org.aspectj.lang.annotation.Before;

    /**
    * 切面类
    */
    @Aspect
    public class MyAspectAnno {
        @Before(value = "execution(* com.imooc.aspectJ.demo1.ProductDao.*(..))")
        public void before() {
            System.out.println("前置通知...=>");
        }
    }

    // Demo
    package com.imooc.aspectJ.demo1;

    import org.junit.Test;
    import org.junit.runner.RunWith;
    import org.springframework.test.context.ContextConfiguration;
    import org.springframework.test.context.junit4.SpringJUnit4ClassRunner;

    import javax.annotation.Resource;

    @RunWith(SpringJUnit4ClassRunner.class)
    @ContextConfiguration("classpath:applicationContext.xml")
    public class SpringDemo1 {

        @Resource(name="productDao")
        private ProductDao productDao;

        @Test
        public void demo1() {
            productDao.save();
            productDao.update();
            productDao.delete();
            productDao.findAll();
            productDao.findOne();
        }
    }

```

## @Aspect

1. 表示一个切面

### @Before(value="execution(* *(..))")

1. 表示前置通知

2. 可以在方法中传入 JoinPoint 对象，用来获得切点信息

    ```java
        @Aspect
        public class MyAspectAnno {
            @Before(value = "execution(* com.imooc.aspectJ.demo1.ProductDao.save(..))")
            public void before(JoinPoint joinPoint) {
                // 通过 JoinPoint 对象获得切入点信息
                System.out.println("前置通知...=>"+joinPoint);
            }
        }

    ```

### @AfterReturning

1. 表示后置通知

2. 通过 returning 属性，可以定义方法的返回值，作为参数

    ```java
        @AfterReturning(value = "execution(* com.imooc.aspectJ.demo1.ProductDao.update(..)))",returning = "result")
        public void afterReturning(Object result) {
            // 通过 returning 获得方法的返回值
            System.out.println("后置通知...=>"+result);
        }
    ```

### @Around

1. 表示环绕通知

2. @Around 在目标方法执行之前和之后都会执行一个增强的操作

3. around 方法是有返回值的，它的返回值就是目标代理方法执行的返回值

4. 通过 ProceedingJoinPoint 可以调用拦截目标方法执行

    ```java
        @Around(value = "execution(* com.imooc.aspectJ.demo1.ProductDao.delete(..))")
        public Object around(ProceedingJoinPoint proceedingJoinPoint) throws Throwable {
            System.out.println("环绕前通知...");
            // 如果不调用 ProceedingJoinPoint 的 preceed 方法，那么目标方法就被拦截了，不会执行了
            Object obj = proceedingJoinPoint.proceed();// 执行目标方法
            System.out.println("环绕后通知...");
            return obj;
        }
    ```

### @AfterThrowing

1. 表示异常抛出通知

2. 可以通过设置 throwing 属性，设置发生异常对象的参数，可以得到异常的信息

    ```java
        @AfterThrowing(value = "execution(* com.imooc.aspectJ.demo1.ProductDao.findOne(..))",throwing = "e")
        public void afterThrowing(Throwable e) {
            // 通过设置 throwing 属性，获得方法的异常信息
            System.out.println("异常抛出通知...=>"+e.getMessage());
        }
    ```

### @After

1. 表示最终通知

2. 无论是否出现异常，最终通知总是会被执行

    ```java
        @After(value = "execution(* com.imooc.aspectJ.demo1.ProductDao.findAll(..))")
        public void after() {
            System.out.println("最终通知...=>");
        }
    ```
