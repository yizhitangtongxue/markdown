# Spring MVC实现流程

1. 客户端请求被DispatcherServlet接受

2. DispatcherServlet根据HandlerMapping将请求映射到Handler

3. 生成Handler以及HandlerInterceptor

4. 结合Handler和HandlerInterceptor返回HandlerexecutionChain

5. DispatcherServlet通过HandlerAdapter执行Handler

6. 返回一个ModelAndView

7. DispatcherServlet通过ViewResolver进行解析

8. 返回填充了模型数据的View，响应给客户端

## SpringMVC的使用

1. 大部分组件由框架提供，开发者只需要通过配置进行故那脸

2. 开发者只需手动编写Handler和View

### 基于XML配置的使用

1. SpringMVC基础配置

2. XML配置Controller，HandlerMapping组件映射

3. XML配置ViewResolver组件映射

### 基于注解方式的使用

1. SpringMVC基础配置

2. Controller，HandlerMapping通过注解方式进行映射

3. XML配置ViewResolver组件映射
