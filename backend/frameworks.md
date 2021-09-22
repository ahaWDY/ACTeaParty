# Java框架

## 1. Spring有哪些主要模块？

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

