# AOP概念
  AOP（Aspect Oriented Programming），即面向切面编程，可以说是OOP（Object Oriented Programming，面向对象编程）的补充和完善。OOP引入封装、继承、多态等概念来建立一种对象层次结构，用于模拟公共行为的一个集合。不过OOP允许开发者定义纵向的关系，但并不适合定义横向的关系，例如日志功能。日志代码往往横向地散布在所有对象层次中，而与它对应的对象的核心功能毫无关系对于其他类型的代码，如安全性、异常处理和透明的持续性也都是如此，这种散布在各处的无关的代码被称为横切（cross cutting），在OOP设计中，它导致了大量代码的重复，而不利于各个模块的重用。

  AOP技术恰恰相反，它利用一种称为"横切"的技术，剖解开封装的对象内部，并将那些影响了多个类的公共行为封装到一个可重用模块，并将其命名为"Aspect"，即切面。所谓"切面"，简单说就是那些与业务无关，却为业务模块所共同调用的逻辑或责任封装起来，便于减少系统的重复代码，降低模块之间的耦合度，并有利于未来的可操作性和可维护性。

  使用"横切"技术，AOP把软件系统分为两个部分：核心关注点和横切关注点。业务处理的主要流程是核心关注点，与之关系不大的部分是横切关注点。横切关注点的一个特点是，他们经常发生在核心关注点的多处，而各处基本相似，比如权限认证、日志、事物。AOP的作用在于分离系统中的各种关注点，将核心关注点和横切关注点分离开来。

# AOP核心概念
Aspect（切面）： Aspect 声明类似于 Java 中的类声明，在 Aspect 中会包含着一些 Pointcut 以及相应的 Advice。

Joint point（连接点）：表示在程序中明确定义的点，典型的包括方法调用，对类成员的访问以及异常处理程序块的执行等等，它自身还可以嵌套其它 joint point。

Pointcut（切点）：表示一组 joint point，这些 joint point 或是通过逻辑关系组合起来，或是通过通配、正则表达式等方式集中起来，它定义了相应的 Advice 将要发生的地方。

Advice（增强）：Advice 定义了在 Pointcut 里面定义的程序点具体要做的操作，它通过 before、after 和 around 来区别是在每个 joint point 之前、之后还是代替执行的代码。

Target（目标对象）：织入 Advice 的目标对象.。

Weaving（织入）：将 Aspect 和其他对象连接起来, 并创建 Adviced object 的过程

# 举例理解

  下面我以一个简单的例子来比喻一下 AOP 中 Aspect, Joint point, Pointcut 与 Advice之间的关系.

  让我们来假设一下, 从前有一个叫爪哇的小县城, 在一个月黑风高的晚上, 这个县城中发生了命案. 作案的凶手十分狡猾, 现场没有留下什么有价值的线索. 不过万幸的是, 刚从隔壁回来的老王恰好在这时候无意中发现了凶手行凶的过程, 但是由于天色已晚, 加上凶手蒙着面, 老王并没有看清凶手的面目, 只知道凶手是个男性, 身高约七尺五寸. 爪哇县的县令根据老王的描述, 对守门的士兵下命令说: 凡是发现有身高七尺五寸的男性, 都要抓过来审问. 士兵当然不敢违背县令的命令, 只好把进出城的所有符合条件的人都抓了起来.

来让我们看一下上面的一个小故事和 AOP 到底有什么对应关系.
首先我们知道, 在 Spring AOP 中 Joint point 指代的是所有方法的执行点, 而 point cut 是一个描述信息, 它修饰的是 Joint point, 通过 point cut, 我们就可以确定哪些 Joint point 可以被织入 Advice. 对应到我们在上面举的例子, 我们可以做一个简单的类比, Joint point 就相当于 爪哇的小县城里的百姓,pointcut 就相当于 老王所做的指控, 即凶手是个男性, 身高约七尺五寸, 而 Advice 则是施加在符合老王所描述的嫌疑人的动作: 抓过来审问.
为什么可以这样类比呢?

Joint point ： 爪哇的小县城里的百姓: 因为根据定义, Joint point 是所有可能被织入 Advice 的候选的点, 在 Spring AOP中, 则可以认为所有方法执行点都是 Joint point. 而在我们上面的例子中, 命案发生在小县城中, 按理说在此县城中的所有人都有可能是嫌疑人.

Pointcut ：男性, 身高约七尺五寸: 我们知道, 所有的方法(joint point) 都可以织入 Advice, 但是我们并不希望在所有方法上都织入 Advice, 而 Pointcut 的作用就是提供一组规则来匹配joinpoint, 给满足规则的 joinpoint 添加 Advice. 同理, 对于县令来说, 他再昏庸, 也知道不能把县城中的所有百姓都抓起来审问, 而是根据凶手是个男性, 身高约七尺五寸, 把符合条件的人抓起来. 在这里 凶手是个男性, 身高约七尺五寸 就是一个修饰谓语, 它限定了凶手的范围, 满足此修饰规则的百姓都是嫌疑人, 都需要抓起来审问.

Advice ：抓过来审问, Advice 是一个动作, 即一段 Java 代码, 这段 Java 代码是作用于 point cut 所限定的那些 Joint point 上的. 同理, 对比到我们的例子中, 抓过来审问 这个动作就是对作用于那些满足 男性, 身高约七尺五寸 的爪哇的小县城里的百姓.

Aspect:：Aspect 是 point cut 与 Advice 的组合, 因此在这里我们就可以类比: “根据老王的线索, 凡是发现有身高七尺五寸的男性, 都要抓过来审问” 这一整个动作可以被认为是一个 Aspect.



# 代码案例

我们首先建立配置类Config，在此类开启AspectJ注解@EnableAspectJAutoProxy

**注意**：在最新版本的Spring框架中，`@EnableAspectJAutoProxy`注解不是必需的。当你使用`<aop:aspectj-autoproxy>`配置或者Spring Boot时，它会自动启用AspectJ自动代理。

Spring框架会根据以下条件自动启用AspectJ自动代理：

1. 在类路径下存在AspectJ织入器（例如，AspectJ的相关依赖已经被引入）。
2. Spring上下文中存在至少一个`@Aspect`注解的切面类。

因此，如果你的项目满足这些条件，你无需显式地使用`@EnableAspectJAutoProxy`注解。Spring框架会自动探测并启用AspectJ自动代理。

Config类：

```java
package com.xsh.springaop;

import org.springframework.context.annotation.ComponentScan;
import org.springframework.context.annotation.Configuration;
import org.springframework.context.annotation.EnableAspectJAutoProxy;
@ComponentScan
@Configuration
//开启AspectJ注解
@EnableAspectJAutoProxy
public class Config {
}

```



------

下列代码是一个简单的AOP切面示例，用于在目标方法执行之前和执行之后打印日志。

1. `@Aspect`注解表示这是一个切面类，用于声明切面的功能。
2. `@Component`注解表示这个切面类是一个Spring组件，将被Spring容器管理。
3. `@Before("execution(* com.xsh.springaop.MyServer.fun1(..))")`注解表示这个方法将在目标方法执行之前执行。它使用了切点表达式来指定切入的连接点。在本例中，切点表达式`execution(* com.xsh.springaop.MyServer.fun1(..))`表示匹配`com.xsh.springaop.MyServer`类中的`fun1`方法，并且方法参数任意。
4. `@After("execution(* com.xsh.springaop.MyServer.fun2(..))")`注解表示这个方法将在目标方法执行之后执行。切点表达式与上述相似，匹配`com.xsh.springaop.MyServer`类中的`fun2`方法。
5. `beforeMethodExecution()`方法是前置通知方法，它在目标方法执行之前被调用，打印了一条日志信息。
6. `afterAdvice()`方法是后置通知方法，它在目标方法执行之后被调用，打印了一条日志信息。

通过这种方式，可以在不修改原有业务逻辑的情况下，通过AOP切面对目标方法进行增强操作，例如记录日志、性能监控、事务管理等。在示例中，切面类`LoggingAspect`将在目标方法执行前后输出日志信息。

LoggingAspect:

```java
package com.xsh.springaop;

import org.aspectj.lang.annotation.After;
import org.aspectj.lang.annotation.Aspect;
import org.aspectj.lang.annotation.Before;
import org.springframework.stereotype.Component;



@Aspect
@Component
public class LoggingAspect {

    @Before("execution(* com.xsh.springaop.MyServer.fun1(..))")
    public void beforeMethodExecution() {
        System.out.println("Before executing method");
    }

    @After("execution(* com.xsh.springaop.MyServer.fun2(..))")
    public void afterAdvice() {
        System.out.println("After executing method");
    }




}

```

------



下列代码是有2个方法，分别对应切面类中的所切方法。

MyServer:

```java
package com.xsh.springaop;


import org.springframework.stereotype.Service;

@Service
public class MyServer {


    public void fun1() {
        System.out.println("我是方法1111");
    }

    public void fun2(){
        System.out.println("我是方法2222");
    }

}

```





------

下列代码是一个Spring Boot的应用程序启动类。它实现了`CommandLineRunner`接口，用于在应用程序启动后执行一些初始化任务。

1. `@Component`注解表示这个类是一个Spring组件，将被Spring容器管理。
2. `AppRunner`类实现了`CommandLineRunner`接口，它定义了一个`run`方法，在应用程序启动后会被自动调用。
3. 在`AppRunner`类中，使用`@Autowired`注解将`MyServer`类的实例自动注入进来，即将`MyServer`对象注入到`myServer`字段中。
4. 在`run`方法中，调用了`myServer`对象的`fun1()`和`fun2()`方法，即执行了目标方法。

通过这种方式，当应用程序启动后，`AppRunner`类的`run`方法将会被自动调用，从而触发执行`MyServer`类中的目标方法`fun1()`和`fun2()`。这样可以方便地测试和验证切面是否生效，是否正确地增强了目标方法的功能。

AppRunner:

```JAVA
package com.xsh.springaop;

import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.boot.CommandLineRunner;
import org.springframework.stereotype.Component;

@Component
public class AppRunner implements CommandLineRunner {

    @Autowired
    private MyServer myServer;

    @Override
    public void run(String... args) throws Exception {
        myServer.fun1();
        myServer.fun2();
    }
}


```

