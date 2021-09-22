# Java框架

## 1. Spring有哪些主要模块？

1、Spring core：核心容器

核心容器提供spring框架的基本功能。Spring以bean的方式组织和管理Java应用中的各个组件及其关系。Spring使用BeanFactory来产生和管理Bean，它是工厂模式的实现。BeanFactory使用控制反转(IoC)模式将应用的配置和依赖性规范与实际的应用程序代码分开。BeanFactory使用依赖注入的方式提供给组件依赖。主要实现控制反转IoC和依赖注入DI、Bean配置以及加载。

2、Spring AOP：Spring面向切面编程

通过配置管理特性，Spring AOP 模块直接将面向方面的编程功能集成到了Spring框架中。所以，可以很容易地使 Spring框架管理的任何对象支持 AOP。Spring AOP 模块为基于 Spring 的应用程序中的对象提供了事务管理服务。通过使用 Spring AOP，不用依赖 EJB 组件，就可以将声明性事务管理集成到应用程序中。AOP把一个业务流程分成几部分，例如权限检查、业务处理、日志记录，每个部分单独处理，然后把它们组装成完整的业务流程。每个部分被称为切面或关注点。

AOP的实现原理为动态代理技术，一共有两种代理模式：

（1）ProxyFactoryBean代理工厂对象

Spring内置代理类，引入一个中间层，能够创建不同类型的对象，利用它可以实现任何形式的AOP。

（2）TransactionProxyFactoryBean事务代理工厂对象

常用在数据库编程上，Spring利用TransactionProxyFactoryBean对事务进行管理，在指定方法前利用AOP连接数据库并开启事务，然后在指定方法返回后利用AOP提交事务并断开数据库。

3、Spring context：Spring上下文

Spring上下文是一个配置文件，向Spring框架提供上下文信息。Spring上下文包括企业服务，如JNDI、EJB、电子邮件、国际化、校验和调度功能。提供框架式Bean访问方式，其他程序可以通过Context访问Spring的Bean资源。

4、Spring DAO

DAO模块主要目的是将持久层相关问题与一般的的业务规则和工作流隔离开来。Spring 中的DAO提供一致的方式访问数据库，不管采用何种持久化技术，Spring都提供一致的编程模型。Spring还对不同的持久层技术提供一致的DAO方式的异常层次结构。Spring的DAO模块对JDBC进行了再封装，隐藏了Connection、Statement、ResultSet等JDBC API，使DAO模块直接继承JdbcDaoSupport类。

5、Spring ORM（Object Relation Mapper）对象关系映射模块

Spring 与所有的主要的ORM框架都集成的很好，包括hibernate、JDO实现、TopLink和IBatis SQL Map等。Spring为所有的这些框架提供了模板之类的辅助类，达成了一致的编程风格。
Spring的ORM模块对ORM框架如Hibernate等进行了封装，Spring能够管理、维护Hibernate，使用时可直接继承HibernateDaoSupport类，该类内置一个HibernateTemplate。Hibernate的配置也转移到Spring配置文件中。

（注：ORM是通过使用描述对象和数据库之间映射的元数据，ORM框架采用元数据来描述对象--关系映射细节，元数据一般采用xml格式，并且存放在专门的对象--映射文件中）

6、Spring Web模块

Web模块建立在应用程序上下文模块之上，为基于Web的应用程序提供了上下文。Web层使用Web层框架，可选的，可以是Spring自己的MVC框架，或者提供的Web框架，如Struts、Webwork、tapestry和jsf。

Web模块用于整合Web框架，将Web框架也纳入Spring的管理之中。如Spring提供继承方式与代理方式整合Struts，继承方式不需要更改任何配置文件，只把Action继承自ActionSupport即可，但会对Spring产生依赖。代理方式需要在struts-config.xml中配置<controller>，由Spring全盘代理，因此可以使用Spring的各种资源、拦截器等。

7、Spring MVC

MVC框架是一个全功能的构建Web应用程序的MVC实现。通过策略接口，MVC框架变成为高度可配置的。Spring的MVC框架提供清晰的角色划分：控制器、验证器、命令对象、表单对象和模型对象、分发器、处理器映射和视图解析器。Spring支持多种视图技术。

Spring MVC 的工作流程：

（1） 客户端发送请求，请求到达 DispatcherServlet 主控制器。
（2） DispatcherServlet 控制器调用 HandlerMapping 处理。
（3） HandlerMapping 负责维护请求和 Controller 组件对应关系。 HandlerMapping 根据请求调用对应的 Controller 组件处理。
（4） 执行 Controller 组件的业务处理，需要访问数据库，可以调用 DAO 等组件。
（5）Controller 业务方法处理完毕后，会返回一个 ModelAndView 对象。该组件封装了模型数据和视图标识。
（6）Servlet 主控制器调用 ViewResolver 组件，根据 ModelAndView 信息处理。定位视图资源，生成视图响应信息。
（7）控制器将响应信息给用户输出。 

## 2. 说一下Spring MVC运行流程？

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

