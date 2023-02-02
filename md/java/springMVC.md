# MVC设计模式
## 设计模式、框架、架构的区别
1. 框架：框架通常是代码重用，可以用代码表示，能够直接执行和复用
2. 设计模式：通常是设计重用，设计模式只有实例化之后才能用代码表示
3. 架构：介于两者之间，部分代码重用，部分设计重用，有时分析也可重用
3. 设计模式是比框架更小的元素，一个框架中往往含有一个或多个设计模式
4. 框架针对某一特定应用领域，模式却可适用于各种应用，框架偏重`实现`，模式偏重`思想`
6. 设计模式和框架都是软件层面的，架构还包括硬件层面的，比如部署系统所需的物理器件
## 什么是MVC设计模式
MVC(Model-View-Controller) 设计模式是一种软件设计思想，即通过C将 M 和 V 的实现代码分离，使同一个程序可以有不同的表现形式
* V(视图层):格式化数据并把它们呈现给用户，包括数据展示、用户交互、数据验证、界面设计等
* C(控制层)：接收并转发请求，对请求进行处理后，指定视图并将结果响应到视图层
* M(数据模型层):程序的`主体和核心`，它负责数据处理、业务逻辑和实现数据存取等
## MVC设计模式的优缺点
> 优点

* 多视图共享一个模型，大大提高了代码的可重用性
* MVC 三个模块相互独立，松耦合架构
* 控制器提高了应用程序的灵活性和可配置性
* 有利于软件工程化管理
> 缺点

* 原理复杂
* 增加了系统结构和实现的复杂性
* 视图对模型数据的低效率访问
## 常见的MVC框架
### Struts1.X
### Webwork2 / Struts2
### springMVC
# springMVC概述
springMVC 是 一个基于MVC 设计模式的轻量级 Web 开发框架,本质上相当于` Servlet`
## springMVC的优点
* 清晰地角色划分，Model、View 和 Controller各司其职，各负其责
* 灵活的配置功能，可以把类当作 Bean 通过 XML 进行配置。
* 丰富的控制器接口和实现类，可以用 Spring 提供的控制器实现类，也可以自己实现控制器接口
* 真正做到与 View 层的实现无关，不强制 JSP，可根据需要使用 Velocity、FreeMarker 等技术
* 国际化支持
* 面向接口编程
* 与 Spring 框架无缝集成
# springMVC原理
# springMVC使用
## springMVC配置
springMVC 是基于`Servlet` 的，`DispatcherServlet`是整个springMVC 框架的核心，主要负责`截获请求`并将其分派给相应的处理器处理。所以配置springMVC，首先要定义DispatcherServlet。跟所有 Servlet 一样，用户`必须在web.xml`中进行配置。
> web.xml
```XML
<?xml version="1.0" encoding="UTF-8"?>
<web-app xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
    xmlns="http://java.sun.com/xml/ns/javaee" xmlns:web="http://java.sun.com/xml/ns/javaee/web-app_2_5.xsd"
    xsi:schemaLocation="http://java.sun.com/xml/ns/javaee http://java.sun.com/xml/ns/javaee/web-app_3_0.xsd"
    version="3.0">
    <display-name>springMVC</display-name>
    <!-- 部署 DispatcherServlet -->
    <servlet>
        <servlet-name>springmvc</servlet-name>
        <servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>
        <!-- 指定 springMVC配置文件的位置 -->
        <init-param>
        	<param-name>contextConfigLocation</param-name>
        	<param-value>classpath:springmvc-servlet.xml</param-value>
    	</init-param>
        <!-- 表示容器再启动时立即加载servlet -->
        <load-on-startup>1</load-on-startup>
    </servlet>
    <servlet-mapping>
        <servlet-name>springmvc</servlet-name>
        <!-- 处理所有URL -->
        <url-pattern>/</url-pattern>
    </servlet-mapping>
</web-app>
```
> springmvc-servlet.xml
```XML
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="
        http://www.springframework.org/schema/beans
        http://www.springframework.org/schema/beans/spring-beans.xsd">
    <!-- LoginController控制器类，映射到"/login" -->
    <bean name="/login"
          class="net.biancheng.controller.LoginController"/>
    <!-- LoginController控制器类，映射到"/register" -->
    <bean name="/register"
          class="net.biancheng.controller.RegisterController"/>
</beans>
```
> controller(login.java)
> controller(register.java)
```java
public class LoginController implements Controller {
    public ModelAndView handleRequest(HttpServletRequest arg0,
            HttpServletResponse arg1) throws Exception {
        return new ModelAndView("/WEB-INF/jsp/register.jsp");
    }
}
```