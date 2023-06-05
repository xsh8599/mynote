







## springmvc概念

JavaEE[体系结构](https://so.csdn.net/so/search?q=%E4%BD%93%E7%B3%BB%E7%BB%93%E6%9E%84&spm=1001.2101.3001.7020)包括四层，从上到下分别是应用层、Web层、业务层、持久层。Struts和SpringMVC是Web层的框架，Spring是业务层的框架，Hibernate和MyBatis是持久层的框架。

SpringMVC 是一种轻量级的、基于 MVC 的 Web 层应用框架，它属于 Spring 框架的一部分。SpringMVC 说白了就是对 Servlet 进行了封装，方便大家使用。



## springmvc的特点

很多应用程序的问题在于处理业务数据的对象和显示业务数据的视图之间存在紧密耦合，通常，更新业务对象的命令都是从视图本身发起的，使视图对任何业务对象更改都有高度敏感性。而且，当多个视图依赖于同一个业务对象时是没有灵活性的。

SpringMVC是一种基于Java，实现了Web MVC设计模式，请求驱动类型的轻量级Web框架，即使用了MVC架构模式的思想，将Web层进行职责解耦。基于请求驱动指的就是使用请求-响应模型，框架的目的就是帮助我们简化开发，SpringMVC也是要简化我们日常Web开发。

特点：

- 天生与 Spring 集成
- 支持 Restful 风格开发
- 便于与其他视图技术集成，例如 theamleaf、freemarker等
- 强大的异常处理
- 对静态资源的支持

## spring组件



| 组件                                | 作用                                       |
| --------------------------------- | ---------------------------------------- |
| DispatcherServlet（是一种前端控制器，由框架提供） | 统一处理请求和响应。除此之外还是整个流程控制的中心，由 DispatcherServlet 来调用其他组件，处理用户的请求 |
| HandlerMapping（处理器映射器，由框架提供）      | 根据请求的 url、method 等信息来查找具体的 Handler(一般来讲是Controller) |
| Handler(一般来讲是Controller)          | 在 DispatcherServlet 的控制下，Handler对具体的用户请求进行处理 |
| HandlerAdapter（处理器适配器 ，由框架提供）     | 根据映射器找到的处理器 Handler 信息，按照特定的规则去执行相关的处理器 Handler。 |
| ViewResolver （视图解析器，由框架提供）        | ViewResolver 负责将处理结果生成 View 视图。 ViewResolver 首先根据逻辑视图名解析成物理图名，即具体的页面地址，再生成 View 视图对象，最后对 View 进行渲染将处理结果通过页面展示给用户。 |
| View （视图，工程师自己开发）                 | View接口的职责就是接收[model](https://so.csdn.net/so/search?q=model&spm=1001.2101.3001.7020)对象、Request对象、Response对象，并渲染输出结果给Response对象。 |

小结：

Handler 是用来干活的工具；

HandlerMapping 用于根据需要干的活找到相应的工具；

HandlerAdapter 是使用工具干活的人。详细讲解可以看这篇博客(115条消息) SpringMVC 处理器适配器详解_aFa攻防实验室的博客-CSDN博客_处理器适配器

## springmvc流程

 下面是基于Spring MVC的请求处理流程的简要描述：

1. 用户通过浏览器发起 HttpRequest 请求到前端控制器 (DispatcherServlet)。
2. DispatcherServlet 将用户请求发送给处理器映射器 (HandlerMapping)。
3. 处理器映射器 (HandlerMapping)会根据请求，找到负责处理该请求的处理器（handler），并将其封装为处理器执行链 返回 (HandlerExecutionChain) 给 DispatcherServlet
4. DispatcherServlet 会根据 处理器执行链 中的处理器，找到能够执行该处理器的处理器适配器(HandlerAdaptor)    --注，处理器适配器有多个
5. 处理器适配器 (HandlerAdaptoer) 会调用对应的具体的 Controller
6. Controller 将处理结果及要跳转的视图封装到一个对象 ModelAndView 中并将其返回给处理器适配器 (HandlerAdaptor)
7. HandlerAdaptor 直接将 ModelAndView 交给 DispatcherServlet ，至此，业务处理完毕
8. 业务处理完毕后，我们需要将处理结果展示给用户。于是DisptcherServlet 调用 ViewResolver，将 ModelAndView 中的视图名称封装为视图对象
9. ViewResolver 将封装好的视图 (View) 对象返回给 DIspatcherServlet
10. DispatcherServlet 调用视图对象，让其自己 (View) 进行渲染（将模型数据填充至视图中），形成响应对象 (HttpResponse)
11. 前端控制器 (DispatcherServlet) 响应 (HttpResponse) 给浏览器，展示在页面上。

![img](https://img-blog.csdnimg.cn/75ec98a325394e748c10d1903f4ee419.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBA6bqm55Sw6YeM55qE5a6I5pyb6ICF5ZGA,size_17,color_FFFFFF,t_70,g_se,x_16)

## springmvc简单案例

controller：

```java
package com.xsh.controller;

import org.springframework.stereotype.Controller;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.PostMapping;
import org.springframework.web.bind.annotation.RequestParam;
import org.springframework.web.servlet.ModelAndView;

@Controller
public class LoginController {

    @GetMapping("/login")
    public String showLoginForm() {
        return "login";
    }

    @PostMapping("/login")
    public ModelAndView processLogin(@RequestParam("username") String username, @RequestParam("password") String password) {
        ModelAndView modelAndView = new ModelAndView();

        // 假设用户名为 "admin"，密码为 "password" 才能成功登录
        if (username.equals("admin") && password.equals("password")) {
            modelAndView.setViewName("success");
            modelAndView.addObject("username", username);
        } else {
            modelAndView.setViewName("failure");
        }

        return modelAndView;
    }
}

```



`@Controller`：这个注解将`LoginController`类标识为一个控制器组件，告诉Spring MVC该类负责处理请求。作为handler

`@GetMapping("/login")`：这个注解指定了处理GET请求，并且映射到"/login"路径。`showLoginForm()`方法返回字符串"login"，表示要展示名为"login"的视图。

`@PostMapping("/login")`：这个注解指定了处理POST请求，并且映射到"/login"路径。`processLogin()`方法处理登录表单提交的数据，并返回一个`ModelAndView`对象。

`@RequestParam("username")` 和 `@RequestParam("password")`：这些注解将请求参数与方法的参数进行绑定。在`processLogin()`方法中，`username`和`password`参数分别绑定了请求中名为"username"和"password"的参数值。

`ModelAndView`：`ModelAndView`是Spring MVC中的一个组件，用于在控制器方法中设置视图名称和模型数据。`	ModelAndView`是一个用于封装视图名称和模型数据的类，它允许控制器方法同时设置视图名称和向视图传递数据。它是在控制器方法中创建的，并通过返回`ModelAndView`对象将视图名称和模型数据传递给DispatcherServlet。

​	在控制器方法中，可以使用`ModelAndView`的`setViewName()`方法设置视图名称，将视图的逻辑名称（例如 "success"、"failure"等）指定为字符串参数。然后，可以使用`addObject()`方法将模型数据添加到`ModelAndView`对象中，以供视图使用。模型数据可以是任何Java对象，例如字符串、列表、映射等。
​	最后，控制器方法应该返回`ModelAndView`对象，使其成为请求处理的结果。DispatcherServlet将根据`ModelAndView`中的视图名称解析实际的视图对象，并将模型数据传递给视图，以便呈现最终的响应。
​	总之，`ModelAndView`是Spring MVC中用于设置视图名称和模型数据的组件。它在控制器方法中创建，并通过返回`ModelAndView`对象将视图名称和模型数据传递给DispatcherServlet。



根据上述代码，当访问"/login"路径时，会执行`showLoginForm()`方法，返回"login"作为视图名称，表示要展示登录页面。当提交登录表单时，会执行`processLogin()`方法，根据用户名和密码的验证结果，设置不同的视图名称（"success"或"failure"）和模型数据。

springmvc-servlet.xml:

```xml
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:mvc="http://www.springframework.org/schema/mvc"
       xmlns:context="http://www.springframework.org/schema/context"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
                           http://www.springframework.org/schema/beans/spring-beans.xsd
                           http://www.springframework.org/schema/mvc
                           http://www.springframework.org/schema/mvc/spring-mvc.xsd
                           http://www.springframework.org/schema/context
                           http://www.springframework.org/schema/context/spring-context.xsd"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">

    <!-- 启用 Spring MVC 注解驱动 -->
    <mvc:annotation-driven/>

    <!-- 扫描 Controller 类 -->
    <context:component-scan base-package="com.xsh.controller"/>

    <!-- 视图解析器配置 -->
    <bean class="org.springframework.web.servlet.view.InternalResourceViewResolver">
        <property name="prefix" value="/WEB-INF/views/"/>
        <property name="suffix" value=".jsp"/>
    </bean>

</beans>

```



`<mvc:annotation-driven/>`：这个配置启用Spring MVC的注解驱动功能，允许在控制器中使用注解来处理请求。

`<context:component-scan>`：这个配置指定要扫描的基础包路径，以查找带有注解的组件。在这个例子中，它扫描位于"com.xsh.controller"包下的控制器类。

- `<bean>`：这个配置定义了一个名为`InternalResourceViewResolver`的视图解析器。
- `class="org.springframework.web.servlet.view.InternalResourceViewResolver"`：这个配置指定了视图解析器的类名，即`InternalResourceViewResolver`，它是Spring MVC提供的一个内部资源视图解析器。
- `<property>`：这个配置设置视图解析器的属性。
  - `name="prefix"`：设置前缀属性，指定视图文件的路径前缀。在这个例子中，视图文件应该位于"/WEB-INF/views/"目录下。
  - `name="suffix"`：设置后缀属性，指定视图文件的后缀名。在这个例子中，视图文件应该使用".jsp"作为后缀。



web.xml

```xml
<?xml version="1.0" encoding="UTF-8"?>
<web-app xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xmlns="http://xmlns.jcp.org/xml/ns/javaee"
         xsi:schemaLocation="http://xmlns.jcp.org/xml/ns/javaee
                             http://xmlns.jcp.org/xml/ns/javaee/web-app_3_1.xsd"
         id="WebApp_ID" version="3.1">

  <display-name>SpringMVCDemo</display-name>

  <!-- Spring MVC 配置 -->
  <servlet>
    <servlet-name>springmvc</servlet-name>
    <servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>
    <init-param>
      <param-name>contextConfigLocation</param-name>
      <param-value>/WEB-INF/springmvc-servlet.xml</param-value>
    </init-param>
    <load-on-startup>1</load-on-startup>
  </servlet>

  <servlet-mapping>
    <servlet-name>springmvc</servlet-name>
    <url-pattern>/</url-pattern>
  </servlet-mapping>

</web-app>

```



- `<servlet>`：定义一个Servlet。
- `<servlet-name>`：指定Servlet的名称，这里命名为"springmvc"。
- `<servlet-class>`：指定Servlet的类，这里使用Spring MVC的`DispatcherServlet`。
- `<init-param>`：定义Servlet的初始化参数。
  - `<param-name>`：指定初始化参数的名称。
  - `<param-value>`：指定初始化参数的值，这里是Spring MVC配置文件的路径`/WEB-INF/springmvc-servlet.xml`。
- `<load-on-startup>`：指定Servlet的加载顺序，这里设置为1，表示在Web应用程序启动时立即加载。
- `<servlet-mapping>`：将Servlet映射到URL路径。
- `<servlet-name>`：指定Servlet的名称，与上面定义的Servlet名称一致。
- `<url-pattern>`：指定URL路径，这里设置为"/"，表示将所有的请求都交给该Servlet处理。

login.jsp

```jsp
<!DOCTYPE html>
<html>
<head>
    <title>Login</title>
</head>
<body>

<h1>Login</h1>
<form action="/login" method="post">
    <label for="username">Username:</label>
    <input type="text" id="username" name="username" required><br><br>

    <label for="password">Password:</label>
    <input type="password" id="password" name="password" required><br><br>

    <input type="submit" value="Login">
</form>
</body>
</html>

```

success.jsp

```jsp
<!DOCTYPE html>
<html>
<head>
    <title>Success</title>
</head>
<body>
<h1>Login Successful</h1>
<p>Welcome, ${username}!</p>
</body>
</html>

```

failure.jsp

```jsp
<!DOCTYPE html>
<html>
<head>
    <title>Failure</title>
</head>
<body>
<h1>Login Failed</h1>
<p>Invalid username or password.</p>
</body>
</html>

```

