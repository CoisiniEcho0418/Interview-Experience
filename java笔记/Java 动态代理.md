# Java 动态代理

------

## [JDK动态代理的理解与实现](https://blog.csdn.net/weixin_43973404/article/details/113094312) [1](https://blog.csdn.net/weixin_43973404/article/details/113094312)[2](https://cloud.tencent.com/developer/article/1336135)[3](https://blog.csdn.net/belongtocode/article/details/136991185)



**JDK动态代理**是Java反射机制的一个应用，允许在运行时动态创建代理类并增强目标对象的功能。这种代理模式主要用于实现AOP（面向切面编程），在不修改目标类代码的情况下，为目标对象提供一种代理以控制对这个对象的访问。

#### 动态代理的基本概念

动态代理与静态代理的区别在于，静态代理的代理类是在编译时就确定下来的，而动态代理的代理类是在运行时创建的。JDK动态代理必须基于接口实现，如果目标类没有实现接口，则需要使用其他代理机制，如CGLIB。

#### 实现JDK动态代理的步骤

**定义接口**：首先需要定义一个或多个接口，这些接口声明了代理类需要实现的方法。**创建目标类**：实现上述接口的目标类，这是代理类将要增强的对象。**实现InvocationHandler**：创建一个实现了*`InvocationHandler`*接口的类，在*`invoke`*方法中定义代理逻辑。**创建代理对象**：使用*Proxy*类的*`newProxyInstance`*方法动态创建代理对象，这个方法需要接口的类加载器、接口数组和*`InvocationHandler`*实例作为参数。

```java
// 创建InvocationHandler实现类

public class MyInvocationHandler implements InvocationHandler {

private Object target;


public MyInvocationHandler(Object target) {

this.target = target;

}


@Override

public Object invoke(Object proxy, Method method, Object[] args) throws Throwable {

// 在目标方法执行前后可以添加增强代码

System.out.println("Before method " + method.getName());

Object result = method.invoke(target, args);

System.out.println("After method " + method.getName());

return result;

}

}


// 使用Proxy创建代理对象

ServiceInterface proxyInstance = (ServiceInterface) Proxy.newProxyInstance(

ServiceInterface.class.getClassLoader(),

new Class<?>[] {ServiceInterface.class},

new MyInvocationHandler(new ServiceImpl())

);
```

#### 动态代理的应用场景

JDK动态代理广泛应用于各种框架中，如Spring AOP，它通过代理模式实现了方法的拦截和增强，用于事务管理、日志记录等功能。

#### 生成代理类的字节码

在JDK 8及之前的版本中，可以通过设置系统属性*`sun.misc.ProxyGenerator.saveGeneratedFiles`*为*true*来让JVM将生成的代理类字节码保存到磁盘。在Java 9及之后的版本中，相应的系统属性变更为*`jdk.proxy.ProxyGenerator.saveGeneratedFiles`*[1](https://blog.csdn.net/weixin_43973404/article/details/113094312)[2](https://cloud.tencent.com/developer/article/1336135)[3](https://blog.csdn.net/belongtocode/article/details/136991185)。

#### 底层原理解析

JDK动态代理的底层原理是通过*`Proxy`*类和*`InvocationHandler`*接口实现的。当调用代理对象的方法时，实际上是调用了*`InvocationHandler`*的*`invoke`*方法，然后由*`invoke`*方法根据方法名来调用目标对象的方法。生成的代理类会继承*`Proxy`*类，并实现了目标对象所实现的接口。

通过深入了解JDK动态代理的原理和实现，开发者可以更好地利用这一机制来设计和优化程序结构，实现代码的解耦和功能的增强。

> 其他参考文章：
>
> [彻底搞懂jdk动态代理并自己动手写一个动态代理-腾讯云开发者社区-腾讯云](https://cloud.tencent.com/developer/article/1336135)

