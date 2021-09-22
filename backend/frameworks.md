# Java框架

## 1. Spring有哪些主要模块？

1. Spring Core：

 Core封装包是框架的最基础部分，提供IOC和依赖注入特性。这里的基础概念是BeanFactory，它提供对Factory模式的经典实现来消除对程序性单例模式的需要，并真正地允许你从程序逻辑中分离出依赖关系和配置。

2.Spring Context: 

构建于Core封装包基础上的 Context封装包，提供了一种框架式的对象访问方法，有些象JNDI注册器。Context封装包的特性得自于Beans封装包，并添加了对国际化（I18N）的支持（例如资源绑定），事件传播，资源装载的方式和Context的透明创建，比如说通过Servlet容器。

3．Spring DAO:  

DAO (Data Access Object)提供了JDBC的抽象层，它可消除冗长的JDBC编码和解析数据库厂商特有的错误代码。 并且，JDBC封装包还提供了一种比编程性更好的声明性事务管理方法，不仅仅是实现了特定接口，而且对所有的POJOs（plain old Java objects）都适用。

4.Spring ORM: 

ORM 封装包提供了常用的“对象/关系”映射APIs的集成层。 其中包括JPA、JDO、Hibernate 和 iBatis 。利用ORM封装包，可以混合使用所有Spring提供的特性进行“对象/关系”映射，如前边提到的简单声明性事务管理。

5.Spring AOP: 

Spring的 AOP 封装包提供了符合AOP Alliance规范的面向方面的编程实现，让你可以定义，例如方法拦截器（method-interceptors）和切点（pointcuts），从逻辑上讲，从而减弱代码的功能耦合，清晰的被分离开。而且，利用source-level的元数据功能，还可以将各种行为信息合并到你的代码中。

6.Spring Web: 

Spring中的 Web 包提供了基础的针对Web开发的集成特性，例如多方文件上传，利用Servlet listeners进行IOC容器初始化和针对Web的ApplicationContext。当与WebWork或Struts一起使用Spring时，这个包使Spring可与其他框架结合。

7.Spring Web MVC: 

Spring中的MVC封装包提供了Web应用的Model-View-Controller（MVC）实现。Spring的MVC框架并不是仅仅提供一种传统的实现，它提供了一种清晰的分离模型，在领域模型代码和Web Form之间。并且，还可以借助Spring框架的其他特性。

## 2. 说一下Spring MVC运行流程？

参考博客：https://www.jianshu.com/p/8a20c547e245

1.用户发送请求至前端控制器DispatcherServlet

2.DispatcherServlet收到请求调用处理器映射器HandlerMapping。

3.处理器映射器根据请求url找到具体的处理器，生成处理器执行链HandlerExecutionChain(包括处理器对象和处理器拦截器)一并返回给DispatcherServlet。

4.DispatcherServlet根据处理器Handler获取处理器适配器HandlerAdapter执行HandlerAdapter处理一系列的操作，如：参数封装，数据格式转换，数据验证等操作

5.执行处理器Handler(Controller，也叫页面控制器)。

6.Handler执行完成返回ModelAndView

7.HandlerAdapter将Handler执行结果ModelAndView返回到DispatcherServlet

8.DispatcherServlet将ModelAndView传给ViewReslover视图解析器

9.ViewReslover解析后返回具体View

10.DispatcherServlet对View进行渲染视图（即将模型数据model填充至视图中）。

11.DispatcherServlet响应用户。


## 3. jpa和hibernate有什么区别？

## 4. Spring中的bean是线程安全的吗？

- Spring中的Bean：https://www.awaimai.com/2596.html
- https://www.jianshu.com/p/4db5f16f83a5
- https://cloud.tencent.com/developer/article/1743283

Spring中的Bean理论上不是线程安全的，但是实际使用时默认是线程安全的。

在 Spring 中，构成应用程序主干并由Spring IoC容器管理的对象称Bean。Bean是一个由Spring IoC容器实例化、组装和管理的对象。在 Spring 中，类的实例化、依赖的实例化、依赖的传入都交由 Spring Bean 容器控制， 而不是用new方式实例化对象、通过非构造函数方法传入依赖等常规方式。容器本身并没有提供Bean的线程安全策略，因此可以说Spring容器中的Bean本身不具备线程安全的特性。

然而，Spring中的Bean默认是单例模式的，框架并没有对bean进行多线程的封装处理。实际上大部分时间Bean是无状态的（比如Dao） 所以说在某种程度上来说Bean其实是安全的。如果Bean是有状态的 那就需要开发人员自己来进行线程安全的保证，最简单的办法就是改变bean的作用域 把  singleton 改为 protopyte， 这样每次请求Bean就相当于是 new Bean() 这样就可以保证线程的安全了。（有状态就是有数据存储功能，无状态就是不会保存数据）

## 5. Spring Boot配置文件有哪几种类型？它们有什么区别？

## 6. Spring Cloud断路器的作用是什么？

