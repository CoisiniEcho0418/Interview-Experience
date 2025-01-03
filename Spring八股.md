# Spring八股

------

## IOC

### Spring的IOC介绍一下

**IOC**：Inversion Of Control，即控制反转，是一种设计思想。在传统的 Java SE 程序设计中，我们直接在对象内部通过 new 的方式来创建对象，是程序主动创建依赖对象；

![图片](https://mmbiz.qpic.cn/sz_mmbiz_png/J0g14CUwaZfQFwefcVKVTArNRnzbZ8CBWQibV45QdvgG3GzkOLsNuO3E9JibT4ZxkiaOl8bDjIZIZnroJLcAA3PHg/640?wx_fmt=png&from=appmsg&wxfrom=5&wx_lazy=1&wx_co=1)

而在Spring程序设计中，IOC 是有专门的容器去控制对象。

![图片](https://mmbiz.qpic.cn/sz_mmbiz_png/J0g14CUwaZfQFwefcVKVTArNRnzbZ8CBVHIoUBO41Vy2JhjNwYo8icbCqT4W2z2PNF8SCicfoZ6jibSnQjdLqhqjA/640?wx_fmt=png&from=appmsg&wxfrom=5&wx_lazy=1&wx_co=1)

**所谓控制**就是对象的创建、初始化、销毁。

- 创建对象：原来是 new 一个，现在是由 Spring 容器创建。
- 初始化对象：原来是对象自己通过构造器或者 setter 方法给依赖的对象赋值，现在是由 Spring 容器自动注入。
- 销毁对象：原来是直接给对象赋值 null 或做一些销毁操作，现在是 Spring 容器管理生命周期负责销毁对象。

总结：IOC 解决了繁琐的对象生命周期的操作，解耦了我们的代码。

**所谓反转**：其实是反转的控制权，前面提到是由 Spring 来控制对象的生命周期，那么对象的控制就完全脱离了我们的控制，控制权交给了 Spring 。这个反转是指：我们由对象的控制者变成了 IOC 的被动控制者。



### IOC源码

[Spring IOC 容器源码分析](https://javadoop.com/post/spring-ioc)



## AOP

### Spring的aop介绍一下

Spring AOP是Spring框架中的一个重要模块，用于实现面向切面编程。

我们知道，Java 就是一门面向对象编程的语言，在 OOP 中最小的单元就是“Class 对象”，但是在 AOP 中最小的单元是“切面”。一个“切面”可以包含很多种类型和对象，对它们进行模块化管理，例如事务管理。

在面向切面编程的思想里面，把功能分为两种

- **核心业务**：登陆、注册、增、删、改、查、都叫核心业务
- **周边功能**：日志、事务管理这些次要的为周边业务

在面向切面编程中，核心业务功能和周边功能是分别独立进行开发，两者不是耦合的，然后把切面功能和核心业务功能 "编织" 在一起，这就叫AOP。

AOP能够将那些与业务无关，**却为业务模块所共同调用的逻辑或责任（例如事务处理、日志管理、权限控制等）封装起来**，便于**减少系统的重复代码**，**降低模块间的耦合度**，并**有利于未来的可拓展性和可维护性**。



### Spring AOP的原理

[SpringAOP原理分析-CSDN博客](https://blog.csdn.net/weixin_40160543/article/details/92010760?ops_request_misc=%7B%22request%5Fid%22%3A%224A74AD3D-EB64-4082-98D9-DF9F15CA231C%22%2C%22scm%22%3A%2220140713.130102334.pc%5Fall.%22%7D&request_id=4A74AD3D-EB64-4082-98D9-DF9F15CA231C&biz_id=0&utm_medium=distribute.pc_search_result.none-task-blog-2~all~first_rank_ecpm_v1~rank_v31_ecpm-19-92010760-null-null.142^v100^pc_search_result_base2&utm_term=AOP原理&spm=1018.2226.3001.4187)

[代理和AOP_运行时增强和编译时增强-CSDN博客](https://blog.csdn.net/weixin_41987908/article/details/135079686?ops_request_misc=&request_id=&biz_id=102&utm_term=编译时增强、运行时增强&utm_medium=distribute.pc_search_result.none-task-blog-2~all~sobaiduweb~default-0-135079686.142^v100^pc_search_result_base2&spm=1018.2226.3001.4187)



## MVC

### Spring MVC的工作流程描述一下

![图片](https://mmbiz.qpic.cn/sz_mmbiz_jpg/J0g14CUwaZfQFwefcVKVTArNRnzbZ8CBSNEH93VxLOnMgY7kTfMWJPoicFAJLWuHnGvO1vx08A3xAYqLVy8BAPQ/640?wx_fmt=jpeg&from=appmsg&wxfrom=5&wx_lazy=1&wx_co=1)

Spring MVC的工作流程如下：

1. 用户发送请求至前端控制器DispatcherServlet
2. DispatcherServlet收到请求调用处理器映射器HandlerMapping。
3. 处理器映射器根据请求url找到具体的处理器，生成处理器执行链HandlerExecutionChain(包括处理器对象和处理器拦截器)一并返回给DispatcherServlet。
4. DispatcherServlet根据处理器Handler获取处理器适配器HandlerAdapter执行HandlerAdapter处理一系列的操作，如：参数封装，数据格式转换，数据验证等操作
5. 执行处理器Handler(Controller，也叫页面控制器)。
6. Handler执行完成返回ModelAndView
7. HandlerAdapter将Handler执行结果ModelAndView返回到DispatcherServlet
8. DispatcherServlet将ModelAndView传给ViewReslover视图解析器
9. ViewReslover解析后返回具体View
10. DispatcherServlet对View进行渲染视图（即将模型数据model填充至视图中）。
11. DispatcherServlet响应用户。



## Bean

### Spring bean是否是线程安全的？如何保证bean的线程安全性？

[面试：Spring 中的bean 是线程安全的吗？-腾讯云开发者社区-腾讯云 (tencent.com)](https://cloud.tencent.com/developer/article/1743283)

[Spring的bean是怎么保证线程安全的_springbean如何保证线程安全-CSDN博客](https://blog.csdn.net/mqq2502513332/article/details/124343634)



## Spring事务

[Spring 事务详解 | JavaGuide](https://javaguide.cn/system-design/framework/spring/spring-transaction.html)

[太难了~面试官让我结合案例讲讲自己对Spring事务传播行为的理解。](https://mp.weixin.qq.com/s?__biz=Mzg2OTA0Njk0OA==&mid=2247486668&idx=2&sn=0381e8c836442f46bdc5367170234abb&chksm=cea24307f9d5ca11c96943b3ccfa1fc70dc97dd87d9c540388581f8fe6d805ff548dff5f6b5b&token=1776990505&lang=zh_CN#rd)



## Spring设计模式

[Spring 中的设计模式详解 | JavaGuide](https://javaguide.cn/system-design/framework/spring/spring-design-patterns-summary.html)
