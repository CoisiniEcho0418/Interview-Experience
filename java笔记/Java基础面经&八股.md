# Java基础面经&八股

------

> **部分内容参考JavaGuide网站：https://javaguide.cn/**



## [为什么说 Java 语言“编译与解释并存”？](https://javaguide.cn/java/basis/java-basic-questions-01.html#为什么说-java-语言-编译与解释并存)

这是因为 Java 语言既具有编译型语言的特征，也具有解释型语言的特征。因为 Java 程序要经过先编译，后解释两个步骤，由 Java 编写的程序需要先经过编译步骤，生成字节码（`.class` 文件），这种字节码必须由 Java 解释器来解释执行。

> 我们可以将高级编程语言按照程序的执行方式分为两种：
>
> - **编译型**：[编译型语言open in new window](https://zh.wikipedia.org/wiki/編譯語言) 会通过[编译器open in new window](https://zh.wikipedia.org/wiki/編譯器)将源代码一次性翻译成可被该平台执行的机器码。一般情况下，编译语言的执行速度比较快，开发效率比较低。常见的编译性语言有 C、C++、Go、Rust 等等。
> - **解释型**：[解释型语言open in new window](https://zh.wikipedia.org/wiki/直譯語言)会通过[解释器open in new window](https://zh.wikipedia.org/wiki/直譯器)一句一句的将代码解释（interpret）为机器代码后再执行。解释型语言开发效率比较快，执行速度比较慢。常见的解释性语言有 Python、JavaScript、PHP 等等。
> - ![编译型语言和解释型语言](https://oss.javaguide.cn/github/javaguide/java/basis/compiled-and-interpreted-languages.png)



## [Java 和 C++ 的区别?](https://javaguide.cn/java/basis/java-basic-questions-01.html#java-和-c-的区别)

虽然，Java 和 C++ 都是面向对象的语言，都支持封装、继承和多态，但是，它们还是有挺多不相同的地方：

- Java 不提供指针来直接访问内存，程序内存更加安全
- Java 的类是单继承的，C++ 支持多重继承；虽然 Java 的类不可以多继承，但是接口可以多继承。
- Java 有自动内存管理垃圾回收机制(GC)，不需要程序员手动释放无用内存。
- C ++同时支持方法重载和操作符重载，但是 Java 只支持方法重载（操作符重载增加了复杂性，这与 Java 最初的设计思想不符）。
- ……



## 讲一下Java面向对象的特点

封装、继承、多态是Java面向对象编程的三大特点。

- **封装（Encapsulation）**：封装是面向对象编程的基本特点之一，它将数据和方法封装在对象内部，隐藏对象的内部实现细节，只暴露必要的接口供外部访问。通过封装，可以实现信息的隐藏和保护，提高代码的安全性和可靠性。

- **继承（Inheritance）**：继承是面向对象编程的重要特点，它允许一个类（子类）继承另一个类（父类）的属性和方法。子类可以重用父类的代码，并可以通过扩展和重写来增加新的功能或修改现有功能。继承提高了代码的复用性和可维护性，同时也体现了类与类之间的关系。

- **多态（Polymorphism）**：多态是面向对象编程的核心概念之一，它允许不同对象对同一消息作出不同的响应。在Java中，多态性通过方法重载和方法重写来实现。方法重载是指在同一个类中可以定义多个同名方法，但参数列表不同；方法重写是指子类可以重写父类的方法，实现不同的行为。多态性提高了代码的灵活性和扩展性，使得程序更易于理解和维护。

  一个具体的例子是，假设有一个动物类（Animal）和它的两个子类：狗类（Dog）和猫类（Cat）。它们都有一个名为“makeSound”的方法，但是每种动物发出的声音是不同的。

  通过多态性，可以创建一个Animal类型的引用指向一个具体的Dog或Cat对象。当调用这个引用的“makeSound”方法时，根据实际指向的对象类型，会执行相应子类的方法，从而实现不同动物发出不同声音的效果。这样就体现了多态的特性，同一个方法调用可以产生不同的行为，提高了代码的灵活性和可扩展性。



## 多态和重载有什么关系？

重载是一种编译时多态，而多态是一种运行时多态。两者都是实现多态性的方式，但发生的时间点和机制不同。

- 重载是指在同一个类中，方法名相同但参数列表不同的情况，通过参数个数、类型或顺序的不同来区分不同的方法。重载是静态绑定的概念，编译器在编译期间根据方法的参数列表来确定调用哪个方法。
- 多态是指同一个方法名可以在不同的类中有不同的实现，不同的子类可以重写父类的方法，通过父类引用指向子类对象时，根据实际对象的类型来确定调用哪个方法。多态是动态绑定的概念，运行时根据对象的实际类型来确定调用哪个方法。



## Java中的private、default、protected、public关键字

> [Java中的private、default、protected、public关键字_java中protect public-CSDN博客](https://blog.csdn.net/weixin_39452419/article/details/132636198?ops_request_misc=%7B%22request%5Fid%22%3A%2252BD1B63-7856-4E84-8E89-03AA76BCB442%22%2C%22scm%22%3A%2220140713.130102334..%22%7D&request_id=52BD1B63-7856-4E84-8E89-03AA76BCB442&biz_id=0&utm_medium=distribute.pc_search_result.none-task-blog-2~all~sobaiduend~default-1-132636198-null-null.142^v100^pc_search_result_base2&utm_term=java private protected public default&spm=1018.2226.3001.4187)

1、public 的可见范围最广

被public修饰的类、变量和方法可以被所有的类访问。适用于一些对外提供的方法和类。

2、defalut 默认访问访问

如果一个类没有使用任何访问范围修饰符，相当于使用了default修饰符，这个类只能被同一包下的类访问。

适用于一些供本包访问设计的类和方法。

3、protected 受保护的访问范围

protected是用来保护子类的，子类可以访问用它修饰的成员，相当于传递给子类的一种继承的东西。另外，同包下的其他类也可以访问它修饰的变量。

4、private 可见范围最小

体现的是封装的思想，被private修饰的对象只能在同一类个中访问，因此，一些需要受保护的成员变量使用private做封装。
![image-20241012235153857](D:\Desktop\八股\java笔记\assets\image-20241012235153857.png)



## Java中子类重写（overwriting）父类的方法

> [Java中子类重写（overwriting）父类的方法_子类重写父类方法-CSDN博客](https://blog.csdn.net/hehahhe1227/article/details/117558746?ops_request_misc=%7B%22request%5Fid%22%3A%22CD533317-EEA2-40D5-A1DB-1A96AAA14CC9%22%2C%22scm%22%3A%2220140713.130102334..%22%7D&request_id=CD533317-EEA2-40D5-A1DB-1A96AAA14CC9&biz_id=0&utm_medium=distribute.pc_search_result.none-task-blog-2~all~sobaiduend~default-1-117558746-null-null.142^v100^pc_search_result_base2&utm_term=java 子类重写父类方法&spm=1018.2226.3001.4187)

### 一、子类和父类的关系

**1、构造方法**
子类在新建对象时，首先调用父类的构造方法，然后调用自己的构造方法。

**2、成员变量**
（1）子类可以继承父类public和protected修饰的成员变量和成员方法。

（2）子类覆盖父类的成员变量

**3、成员方法**
子类覆盖父类的同名同参数列表方法（重写），可以利用super调用父类被覆盖方法。

**4、类型转换**
（1）子类转父类（向上转换）

例如：B是继承A的子类，对于如下对象：

```java
class A {
}
class B extends A {
}
A A1 = new B();
```


那么A1只能调用父类和子类同时拥有的变量和成员方法。存在覆盖的情况时，成员方法调用子类，成员变量调用父类。

（2）父类转子类

会报错。

（3）不同子类之间转换

只保留共有的成员方法，重复的以赋值对象为准。成员变量为父类。

**5、隔代继承**
当直接父类和祖父类存在同名成员变量和成员方法时，采用就近原则，调用直接父类的。

 

### 二、重写

**1、含义**
当一个子类继承一个父类的时候，可以重写覆盖原来父类里面的方法，这个方法要求和父类方法的名称相同,参数列表相同。

**2、需要满足的条件**
（1）父类中的方法在子类中必须可见。对于父类中的private 方法，子类虽能继承，但无法访问和覆盖；对于父类中final的方法，子类继承但不能重写。

（2）子类和父类的方法必须是实例方法，如果父类是 static 方法而子类是实例方法，或者相反都会报错。如果父类和子类都是 static 方法，那么子类隐藏父类的方法，而不是重写父类方法。

例如：child类继承parent类，同时两者都有一个static方法printA。

```java
class parent {

	public static void printA() {
		System.out.println("父类静态");
	}
	 
	public void printB() {
		System.out.println("父类普通");
	}

}

class child extends parent {
	public static void printA() {
		System.out.println("子类静态");
	}

	public void printB() {
		System.out.println("子类普通");
	}

}
```

```java
public static void main(String[] args) {
	child c1 = new child();
	c1.printA();
	c1.printB();
}
```


则在执行main函数时，最后结果为

![img](https://i-blog.csdnimg.cn/blog_migrate/f60555092fd47e938b79775f97a1ba15.png)
（3）子类和父类的方法名、参数列表必须相同（定义），并且子类的返回值与父类相同或者是父类返回类型的子类型（jdk1.5 之后，否则会报错)。如果方法名称相同而参数列表不同，那么只是方法的重载，而非重写。

原因：

假设 B 是 A 的子类，初始化 a 为 B 类型，并调用 B 类重写过的方法 func()：

```java
A a = new B();
C aa = a.func();
```

A类中func返回值为C类型，若B类中func的返回值不是C类或其子类，则会出现错误。

（4）子类方法的访问权限不能小于父类方法的访问权限。访问权限由高到低：public、protected、包访问权限、private。

原因：

假设一个父类 A 拥有的方法 public void func() {} 可以被其他任意对象调用。这个方法被子类 B 覆写后为 void func() {} 即默认的访问权限只能被本包极其子类所访问。

假设其他包中的对象 C 调用方法：

```java
void get(A a) {
    a.func();
}
```

假设此时传入的对象为 B 类对象 b，此时 b 将转型为 a 但是 b 中的 func() 调用权限已经被缩小了，这将会造成错误。

（5）子类方法不能比父类方法抛出更多的编译时异常（不是运行时异常），即子类方法抛出的编译时异常或者和父类相同或者是父类异常的子类。

原因：

子类在覆盖父类方法的时候，父类的引用可以调用该方法。如果父类的引用调用子类的方法，那么这个多抛出来的异常，就可能处于一种无法被处理的状态。

总的来说，子类的规约应当强于父类。



## [接口和抽象类有什么共同点和区别？](https://javaguide.cn/java/basis/java-basic-questions-02.html#接口和抽象类有什么共同点和区别)

**共同点**：

- 都不能被实例化。
- 都可以包含抽象方法。
- 都可以有默认实现的方法（Java 8 可以用 `default` 关键字在接口中定义默认方法）。

**区别**：

- 接口主要用于对类的行为进行约束，你实现了某个接口就具有了对应的行为。抽象类主要用于代码复用，强调的是所属关系。
- 一个类只能继承一个类，但是可以实现多个接口。
- 接口中的成员变量只能是 `public static final` 类型的，不能被修改且必须有初始值，而抽象类的成员变量默认 default，可在子类中被重新定义，也可被重新赋值。

> 参考资料：
>
> [Java基础常见面试题总结(中) | JavaGuide](https://javaguide.cn/java/basis/java-basic-questions-02.html#接口和抽象类有什么共同点和区别)
>
> [Java 抽象类和接口的区别，看这一篇就够了，全面解析 | 二哥的Java进阶之路](https://javabetter.cn/oo/abstract-vs-interface.html)
>
> [七、java中的抽象类和抽象方法详解_java抽象方法-CSDN博客](https://blog.csdn.net/TTDreamTT/article/details/111604919)



## [深拷贝和浅拷贝区别了解吗？什么是引用拷贝？](https://javaguide.cn/java/basis/java-basic-questions-02.html#深拷贝和浅拷贝区别了解吗-什么是引用拷贝)

![浅拷贝、深拷贝、引用拷贝示意图](https://oss.javaguide.cn/github/javaguide/java/basis/shallow&deep-copy.png)



## String，StringBuilder， StringBuffer区别？单线程大量操作字符串用哪个？

回答：用StringBuilder

**补充：**

String、StringBuilder和StringBuffer都是Java中用于操作字符串的类。

String是不可变的字符序列，每次对String进行修改时都会创建一个新的String对象，因此在大量操作字符串时，使用String会频繁地创建对象，导致性能较低。

StringBuilder和StringBuffer都是可变的字符序列，可以对其进行多次修改而不创建新的对象。两者的区别在于线程安全性，StringBuffer是线程安全的，而StringBuilder是非线程安全的。因为StringBuffer的所有共有方法都是同步的，所以在多线程环境下使用StringBuffer可以保证线程安全，但是会降低性能。而StringBuilder没有同步方法，所以在单线程环境下使用StringBuilder性能更高。

因此，在单线程环境下进行大量的字符串操作时，应该使用StringBuilder，可以获得更好的性能。在多线程环境下，使用StringBuffer可以保证线程安全，但是会牺牲一定的性能。

综上所述，单线程大量操作字符串时应该使用StringBuilder，而在多线程环境下应该使用StringBuffer。



## Java 什么时候会涉及上下文切换

程序本身触发的**自发性上下文切换**、系统或虚拟机触发的**非自发性上下文切换**

​		**自发性上下文切换：**

- **sleep、wait、[yield](https://zhida.zhihu.com/search?q=yield&zhida_source=entity&is_preview=1)、join、park、synchronized、lock**

  **非自发性上下文切换：**

- 线程被分配的**时间片用完**、**JVM垃圾回收**（**STW**、线程暂停）、线程**执行优先级**



## Java 反射介绍，优缺点，用过吗

反射之所以被称为框架的灵魂，主要是因为它赋予了我们在运行时分析类以及执行类中方法的能力。通过反射你可以获取任意一个类的所有属性和方法，你还可以调用这些方法和属性。

反射可以让我们的代码更加灵活、为各种框架提供开箱即用的功能提供了便利。

不过，反射让我们在运行时有了分析操作类的能力的同时，也增加了安全问题，比如可以无视泛型参数的安全检查（泛型参数的安全检查发生在编译时）。另外，反射的性能也要稍差点，不过，对于框架来说实际是影响不大的。

像咱们平时大部分时候都是在写业务代码，很少会接触到直接使用反射机制的场景。但是！这并不代表反射没有用。相反，正是因为反射，你才能这么轻松地使用各种框架。像 Spring/Spring Boot、MyBatis 等等框架中都大量使用了反射机制。

**这些框架中也大量使用了动态代理，而动态代理的实现也依赖反射。**

比如下面是通过 JDK 实现动态代理的示例代码，其中就使用了反射类 `Method` 来调用指定的方法。

```java
public class DebugInvocationHandler implements InvocationHandler {
    /**
     * 代理类中的真实对象
     */
    private final Object target;

    public DebugInvocationHandler(Object target) {
        this.target = target;
    }

    public Object invoke(Object proxy, Method method, Object[] args) throws InvocationTargetException, IllegalAccessException {
        System.out.println("before method " + method.getName());
        Object result = method.invoke(target, args);
        System.out.println("after method " + method.getName());
        return result;
    }
}
```

另外，像 Java 中的一大利器 **注解** 的实现也用到了反射。

为什么你使用 Spring 的时候 ，一个`@Component`注解就声明了一个类为 Spring Bean 呢？为什么你通过一个 `@Value`注解就读取到配置文件中的值呢？究竟是怎么起作用的呢？

这些都是因为你可以基于反射分析类，然后获取到类/属性/方法/方法的参数上的注解。你获取到注解之后，就可以做进一步的处理。



## [Object 类的常见方法有哪些？](https://javaguide.cn/java/basis/java-basic-questions-02.html#object-类的常见方法有哪些)

Object 类是一个特殊的类，是所有类的父类。它主要提供了以下 11 个方法：

```java
/**
 * native 方法，用于返回当前运行时对象的 Class 对象，使用了 final 关键字修饰，故不允许子类重写。
 */
public final native Class<?> getClass()
/**
 * native 方法，用于返回对象的哈希码，主要使用在哈希表中，比如 JDK 中的HashMap。
 */
public native int hashCode()
/**
 * 用于比较 2 个对象的内存地址是否相等，String 类对该方法进行了重写以用于比较字符串的值是否相等。
 */
public boolean equals(Object obj)
/**
 * native 方法，用于创建并返回当前对象的一份拷贝。
 */
protected native Object clone() throws CloneNotSupportedException
/**
 * 返回类的名字实例的哈希码的 16 进制的字符串。建议 Object 所有的子类都重写这个方法。
 */
public String toString()
/**
 * native 方法，并且不能重写。唤醒一个在此对象监视器上等待的线程(监视器相当于就是锁的概念)。如果有多个线程在等待只会任意唤醒一个。
 */
public final native void notify()
/**
 * native 方法，并且不能重写。跟 notify 一样，唯一的区别就是会唤醒在此对象监视器上等待的所有线程，而不是一个线程。
 */
public final native void notifyAll()
/**
 * native方法，并且不能重写。暂停线程的执行。注意：sleep 方法没有释放锁，而 wait 方法释放了锁 ，timeout 是等待时间。
 */
public final native void wait(long timeout) throws InterruptedException
/**
 * 多了 nanos 参数，这个参数表示额外时间（以纳秒为单位，范围是 0-999999）。 所以超时的时间还需要加上 nanos 纳秒。。
 */
public final void wait(long timeout, int nanos) throws InterruptedException
/**
 * 跟之前的2个wait方法一样，只不过该方法一直等待，没有超时时间这个概念
 */
public final void wait() throws InterruptedException
/**
 * 实例被垃圾回收器回收的时候触发的操作
 */
protected void finalize() throws Throwable { }

```



### `StringBuilder`类不能调用`equals()`方法来判断是否相等

在Java中，`StringBuilder`类是可变的，它的`equals()`方法并没有被重写，因此它继承自`Object`类的`equals()`方法，该方法默认使用引用相等性来比较对象，即只有在两个`StringBuilder`对象引用的是同一个对象时才会返回`true`。

如果你想要比较`StringBuilder`对象的内容是否相等，可以通过将`StringBuilder`对象转换为`String`对象，然后使用`String`的`equals()`方法来进行比较。或者你也可以使用`StringBuilder`的`toString()`方法先将其转换为`String`，再进行比较。

```java
StringBuilder sb1 = new StringBuilder("hello");
StringBuilder sb2 = new StringBuilder("hello");

// 使用StringBuilder的toString()方法将其转换为String，然后使用equals()方法比较
if (sb1.toString().equals(sb2.toString())) {
    System.out.println("sb1 and sb2 are equal");
} else {
    System.out.println("sb1 and sb2 are not equal");
}

// 或者也可以使用contentEquals()方法来比较，例如：
if (sb1.toString().contentEquals(sb2)) {
    System.out.println("sb1 and sb2 are equal");
} else {
    System.out.println("sb1 and sb2 are not equal");
}
```



## `native`方法

https://blog.csdn.net/weixin_43629719/article/details/88823090



## HashCode

### [为什么要有 hashCode？](https://javaguide.cn/java/basis/java-basic-questions-02.html#为什么要有-hashcode)

我们以“`HashSet` 如何检查重复”为例子来说明为什么要有 `hashCode`？

下面这段内容摘自我的 Java 启蒙书《Head First Java》:

> 当你把对象加入 `HashSet` 时，`HashSet` 会先计算对象的 `hashCode` 值来判断对象加入的位置，同时也会与其他已经加入的对象的 `hashCode` 值作比较，如果没有相符的 `hashCode`，`HashSet` 会假设对象没有重复出现。但是如果发现有相同 `hashCode` 值的对象，这时会调用 `equals()` 方法来检查 `hashCode` 相等的对象是否真的相同。如果两者相同，`HashSet` 就不会让其加入操作成功。如果不同的话，就会重新散列到其他位置。这样我们就大大减少了 `equals` 的次数，相应就大大提高了执行速度。

其实， `hashCode()` 和 `equals()`都是用于比较两个对象是否相等。

**那为什么 JDK 还要同时提供这两个方法呢？**

这是因为在一些容器（比如 `HashMap`、`HashSet`）中，有了 `hashCode()` 之后，判断元素是否在对应容器中的效率会更高（参考添加元素进`HashSet`的过程）！

我们在前面也提到了添加元素进`HashSet`的过程，如果 `HashSet` 在对比的时候，同样的 `hashCode` 有多个对象，它会继续使用 `equals()` 来判断是否真的相同。也就是说 `hashCode` 帮助我们大大缩小了查找成本。

**那为什么不只提供 `hashCode()` 方法呢？**

这是因为两个对象的`hashCode` 值相等并不代表两个对象就相等。

**那为什么两个对象有相同的 `hashCode` 值，它们也不一定是相等的？**

因为 `hashCode()` 所使用的哈希算法也许刚好会让多个对象传回相同的哈希值。越糟糕的哈希算法越容易碰撞，但这也与数据值域分布的特性有关（所谓哈希碰撞也就是指的是不同的对象得到相同的 `hashCode` )。

总结下来就是：

- 如果两个对象的`hashCode` 值相等，那这两个对象不一定相等（哈希碰撞）。
- 如果两个对象的`hashCode` 值相等并且`equals()`方法也返回 `true`，我们才认为这两个对象相等。
- 如果两个对象的`hashCode` 值不相等，我们就可以直接认为这两个对象不相等。



### [为什么重写 equals() 时必须重写 hashCode() 方法？](https://javaguide.cn/java/basis/java-basic-questions-02.html#为什么重写-equals-时必须重写-hashcode-方法)

因为两个相等的对象的 `hashCode` 值必须是相等。也就是说如果 `equals` 方法判断两个对象是相等的，那这两个对象的 `hashCode` 值也要相等。

如果重写 `equals()` 时没有重写 `hashCode()` 方法的话就可能会导致 `equals` 方法判断是相等的两个对象，`hashCode` 值却不相等。

**思考**：重写 `equals()` 时没有重写 `hashCode()` 方法的话，使用 `HashMap` 可能会出现什么问题。

**总结**：

- `equals` 方法判断两个对象是相等的，那这两个对象的 `hashCode` 值也要相等。
- 两个对象有相同的 `hashCode` 值，他们也不一定是相等的（哈希碰撞）。



## String

### String，StringBuilder， StringBuffer区别？单线程大量操作字符串用哪个？

String、StringBuilder和StringBuffer都是Java中用于操作字符串的类。

String是不可变的字符序列，每次对String进行修改时都会创建一个新的String对象，因此在大量操作字符串时，使用String会频繁地创建对象，导致性能较低。

StringBuilder和StringBuffer都是可变的字符序列，可以对其进行多次修改而不创建新的对象。两者的区别在于线程安全性，StringBuffer是线程安全的，而StringBuilder是非线程安全的。因为StringBuffer的所有共有方法都是同步的，所以在多线程环境下使用StringBuffer可以保证线程安全，但是会降低性能。而StringBuilder没有同步方法，所以在单线程环境下使用StringBuilder性能更高。

因此，在单线程环境下进行大量的字符串操作时，应该使用StringBuilder，可以获得更好的性能。在多线程环境下，使用StringBuffer可以保证线程安全，但是会牺牲一定的性能。

综上所述，单线程大量操作字符串时应该使用StringBuilder，而在多线程环境下应该使用StringBuffer。

### [String、StringBuffer、StringBuilder 的区别？](https://javaguide.cn/java/basis/java-basic-questions-02.html#string、stringbuffer、stringbuilder-的区别)

> **对于三者使用的总结：**
>
> 1. 操作少量的数据: 适用 `String`
> 2. 单线程操作字符串缓冲区下操作大量数据: 适用 `StringBuilder`
> 3. 多线程操作字符串缓冲区下操作大量数据: 适用 `StringBuffer`



### [String 为什么是不可变的?](https://javaguide.cn/java/basis/java-basic-questions-02.html#string-为什么是不可变的)



### [字符串拼接用“+” 还是 StringBuilder?](https://javaguide.cn/java/basis/java-basic-questions-02.html#字符串拼接用-还是-stringbuilder)

字符串对象通过“+”的字符串拼接方式，实际上是通过 `StringBuilder` 调用 `append()` 方法实现的，拼接完成之后调用 `toString()` 得到一个 `String` 对象 。

不过，在循环内使用“+”进行字符串的拼接的话，存在比较明显的缺陷：**编译器不会创建单个 `StringBuilder` 以复用，会导致创建过多的 `StringBuilder` 对象**。

```java
String[] arr = {"he", "llo", "world"};
String s = "";
for (int i = 0; i < arr.length; i++) {
    s += arr[i];
}
System.out.println(s);
```

`StringBuilder` 对象是在循环内部被创建的，这意味着每循环一次就会创建一个 `StringBuilder` 对象。

如果直接使用 `StringBuilder` 对象进行字符串拼接的话，就不会存在这个问题了。

```java
String[] arr = {"he", "llo", "world"};
StringBuilder s = new StringBuilder();
for (String value : arr) {
    s.append(value);
}
System.out.println(s);
```



### [String s1 = new String("abc");这句话创建了几个字符串对象？](https://javaguide.cn/java/basis/java-basic-questions-02.html#string-s1-new-string-abc-这句话创建了几个字符串对象)

会创建 1 或 2 个字符串对象。

1、如果字符串常量池中不存在字符串对象“abc”的引用，那么它会在堆上创建两个字符串对象，其中一个字符串对象的引用会被保存在字符串常量池中。

示例代码（JDK 1.8）：

```java
String s1 = new String("abc");
```

对应的字节码：

![img](https://oss.javaguide.cn/github/javaguide/open-source-project/image-20220413175809959.png)

`ldc` 命令用于判断字符串常量池中是否保存了对应的字符串对象的引用，如果保存了的话直接返回，如果没有保存的话，会在堆中创建对应的字符串对象并将该字符串对象的引用保存到字符串常量池中。

2、如果字符串常量池中已存在字符串对象“abc”的引用，则只会在堆中创建 1 个字符串对象“abc”。

示例代码（JDK 1.8）：

```java
// 字符串常量池中已存在字符串对象“abc”的引用
String s1 = "abc";
// 下面这段代码只会在堆中创建 1 个字符串对象“abc”
String s2 = new String("abc");
```

对应的字节码：

![img](https://oss.javaguide.cn/github/javaguide/open-source-project/image-20220413180021072.png)

这里就不对上面的字节码进行详细注释了，7 这个位置的 `ldc` 命令不会在堆中创建新的字符串对象“abc”，这是因为 0 这个位置已经执行了一次 `ldc` 命令，已经在堆中创建过一次字符串对象“abc”了。7 这个位置执行 `ldc` 命令会直接返回字符串常量池中字符串对象“abc”对应的引用。



### [String#intern 方法有什么作用?](https://javaguide.cn/java/basis/java-basic-questions-02.html#string-intern-方法有什么作用)

`String.intern()` 是一个 native（本地）方法，其作用是将指定的字符串对象的引用保存在字符串常量池中，可以简单分为两种情况：

- 如果字符串常量池中保存了对应的字符串对象的引用，就直接返回该引用。
- 如果字符串常量池中没有保存了对应的字符串对象的引用，那就在常量池中创建一个指向该字符串对象的引用并返回。

示例代码（JDK 1.8） :

```java
// 在堆中创建字符串对象”Java“
// 将字符串对象”Java“的引用保存在字符串常量池中
String s1 = "Java";
// 直接返回字符串常量池中字符串对象”Java“对应的引用
String s2 = s1.intern();
// 会在堆中在单独创建一个字符串对象
String s3 = new String("Java");
// 直接返回字符串常量池中字符串对象”Java“对应的引用
String s4 = s3.intern();
// s1 和 s2 指向的是堆中的同一个对象
System.out.println(s1 == s2); // true
// s3 和 s4 指向的是堆中不同的对象
System.out.println(s3 == s4); // false
// s1 和 s4 指向的是堆中的同一个对象
System.out.println(s1 == s4); //true
```



### [String 类型的变量和常量做“+”运算时发生了什么？](https://javaguide.cn/java/basis/java-basic-questions-02.html#string-类型的变量和常量做-运算时发生了什么)

先来看字符串不加 `final` 关键字拼接的情况（JDK1.8）：

```java
String str1 = "str";
String str2 = "ing";
String str3 = "str" + "ing";
String str4 = str1 + str2;
String str5 = "string";
System.out.println(str3 == str4);//false
System.out.println(str3 == str5);//true
System.out.println(str4 == str5);//false
```

**对于编译期可以确定值的字符串，也就是常量字符串 ，jvm 会将其存入字符串常量池。并且，字符串常量拼接得到的字符串常量在编译阶段就已经被存放字符串常量池，这个得益于编译器的优化。**

常量折叠会把常量表达式的值求出来作为常量嵌在最终生成的代码中，这是 Javac 编译器会对源代码做的极少量优化措施之一(代码优化几乎都在即时编译器中进行)。

对于 `String str3 = "str" + "ing";` 编译器会给你优化成 `String str3 = "string";` 。

并不是所有的常量都会进行折叠，只有编译器在程序编译期就可以确定值的常量才可以：

- 基本数据类型( `byte`、`boolean`、`short`、`char`、`int`、`float`、`long`、`double`)以及字符串常量。
- `final` 修饰的基本数据类型和字符串变量
- 字符串通过 “+”拼接得到的字符串、基本数据类型之间算数运算（加减乘除）、基本数据类型的位运算（<<、>>、>>> ）

**引用的值在程序编译期是无法确定的，编译器无法对其进行优化。**

对象引用和“+”的字符串拼接方式，实际上是通过 `StringBuilder` 调用 `append()` 方法实现的，拼接完成之后调用 `toString()` 得到一个 `String` 对象 。

```java
String str4 = new StringBuilder().append(str1).append(str2).toString();
```

我们在平时写代码的时候，尽量避免多个字符串对象拼接，因为这样会重新创建对象。如果需要改变字符串的话，可以使用 `StringBuilder` 或者 `StringBuffer`。

不过，字符串使用 `final` 关键字声明之后，可以让编译器当做常量来处理。

示例代码：

```java
final String str1 = "str";
final String str2 = "ing";
// 下面两个表达式其实是等价的
String c = "str" + "ing";// 常量池中的对象
String d = str1 + str2; // 常量池中的对象
System.out.println(c == d);// true
```

被 `final` 关键字修饰之后的 `String` 会被编译器当做常量来处理，编译器在程序编译期就可以确定它的值，其效果就相当于访问常量。

如果 ，编译器在运行时才能知道其确切值的话，就无法对其优化。

示例代码（`str2` 在运行时才能确定其值）：

```java
final String str1 = "str";
final String str2 = getStr();
String c = "str" + "ing";// 常量池中的对象
String d = str1 + str2; // 在堆上创建的新的对象
System.out.println(c == d);// false
public static String getStr() {
      return "ing";
}
```



## [异常](https://javaguide.cn/java/basis/java-basic-questions-03.html#异常)

![Java 异常类层次结构图](https://oss.javaguide.cn/github/javaguide/java/basis/types-of-exceptions-in-java.png)



### java异常体系介绍一下？

Java异常类层次结构图：![图片](https://mmbiz.qpic.cn/sz_mmbiz_png/J0g14CUwaZenO51RiaFG6OPgJsEPx4Xc5SyBZEU5VbIY1icXJsYicxCVnYJkduLCulm4icPJQUhGs3Cicbe3mMj3Dcw/640?wx_fmt=png&from=appmsg&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)Java的异常体系主要基于两大类：`Throwable`类及其子类。`Throwable`有两个重要的子类：`Error`和`Exception`，它们分别代表了不同类型的异常情况。

1. **Error（错误）**：表示运行时环境的错误。错误是程序无法处理的严重问题，如系统崩溃、虚拟机错误、动态链接失败等。通常，程序不应该尝试捕获这类错误。例如，`OutOfMemoryError`、`StackOverflowError`等。

2. **Exception（异常）**：表示程序本身可以处理的异常条件。异常分为两大类：

3. - **非运行时异常**：这类异常在编译时期就必须被捕获或者声明抛出。它们通常是外部错误，如文件不存在（`FileNotFoundException`）、类未找到（`ClassNotFoundException`）等。非运行时异常强制程序员处理这些可能出现的问题，增强了程序的健壮性。
   - **运行时异常**：这类异常包括运行时异常（`RuntimeException`）和错误（`Error`）。运行时异常由程序错误导致，如空指针访问（`NullPointerException`）、数组越界（`ArrayIndexOutOfBoundsException`）等。运行时异常是不需要在编译时强制捕获或声明的。



### [Exception 和 Error 有什么区别？](https://javaguide.cn/java/basis/java-basic-questions-03.html#exception-和-error-有什么区别)

在 Java 中，所有的异常都有一个共同的祖先 `java.lang` 包中的 `Throwable` 类。`Throwable` 类有两个重要的子类:

- **`Exception`** :程序本身可以处理的异常，可以通过 `catch` 来进行捕获。`Exception` 又可以分为 Checked Exception (受检查异常，必须处理) 和 Unchecked Exception (不受检查异常，可以不处理)。
- **`Error`**：`Error` 属于程序无法处理的错误 ，我们没办法通过 `catch` 来进行捕获不建议通过`catch`捕获 。例如 Java 虚拟机运行错误（`Virtual MachineError`）、虚拟机内存不够错误(`OutOfMemoryError`)、类定义错误（`NoClassDefFoundError`）等 。这些异常发生时，Java 虚拟机（JVM）一般会选择线程终止。



### [Checked Exception 和 Unchecked Exception 有什么区别？](https://javaguide.cn/java/basis/java-basic-questions-03.html#checked-exception-和-unchecked-exception-有什么区别)

**Checked Exception** 即 受检查异常 ，Java 代码在编译过程中，如果受检查异常没有被 `catch`或者`throws` 关键字处理的话，就没办法通过编译。

比如下面这段 IO 操作的代码：

![img](https://oss.javaguide.cn/github/javaguide/java/basis/checked-exception.png)

除了`RuntimeException`及其子类以外，其他的`Exception`类及其子类都属于受检查异常 。常见的受检查异常有：IO 相关的异常、`ClassNotFoundException`、`SQLException`...。

**Unchecked Exception** 即 **不受检查异常** ，Java 代码在编译过程中 ，我们即使不处理不受检查异常也可以正常通过编译。

`RuntimeException` 及其子类都统称为非受检查异常，常见的有（建议记下来，日常开发中会经常用到）：

- `NullPointerException`(空指针错误)
- `IllegalArgumentException`(参数错误比如方法入参类型错误)
- `NumberFormatException`（字符串转换为数字格式错误，`IllegalArgumentException`的子类）
- `ArrayIndexOutOfBoundsException`（数组越界错误）
- `ClassCastException`（类型转换错误）
- `ArithmeticException`（算术错误）
- `SecurityException` （安全错误比如权限不够）
- `UnsupportedOperationException`(不支持的操作错误比如重复创建同一用户)
- ……



### [Throwable 类常用方法有哪些？](https://javaguide.cn/java/basis/java-basic-questions-03.html#throwable-类常用方法有哪些)

`String getMessage()`: 返回异常发生时的简要描述

`String toString()`: 返回异常发生时的详细信息

`String getLocalizedMessage()`: 返回异常对象的本地化信息。使用 `Throwable` 的子类覆盖这个方法，可以生成本地化信息。如果子类没有覆盖该方法，则该方法返回的信息与 `getMessage()`返回的结果相同

`void printStackTrace()`: 在控制台上打印 `Throwable` 对象封装的异常信息



### [如何使用 `try-with-resources` 代替`try-catch-finally`？](https://javaguide.cn/java/basis/java-basic-questions-03.html#如何使用-try-with-resources-代替try-catch-finally)

1. **适用范围（资源的定义）：** 任何实现 `java.lang.AutoCloseable`或者 `java.io.Closeable` 的对象
2. **关闭资源和 finally 块的执行顺序：** 在 `try-with-resources` 语句中，任何 catch 或 finally 块在声明的资源关闭后运行

《Effective Java》中明确指出：

> 面对必须要关闭的资源，我们总是应该优先使用 `try-with-resources` 而不是`try-finally`。随之产生的代码更简短，更清晰，产生的异常对我们也更有用。`try-with-resources`语句让我们更容易编写必须要关闭的资源的代码，若采用`try-finally`则几乎做不到这点。

Java 中类似于`InputStream`、`OutputStream`、`Scanner`、`PrintWriter`等的资源都需要我们调用`close()`方法来手动关闭，一般情况下我们都是通过`try-catch-finally`语句来实现这个需求，如下：

```java
//读取文本文件的内容
Scanner scanner = null;
try {
    scanner = new Scanner(new File("D://read.txt"));
    while (scanner.hasNext()) {
        System.out.println(scanner.nextLine());
    }
} catch (FileNotFoundException e) {
    e.printStackTrace();
} finally {
    if (scanner != null) {
        scanner.close();
    }
}
```

使用 Java 7 之后的 `try-with-resources` 语句改造上面的代码:

```java
try (Scanner scanner = new Scanner(new File("test.txt"))) {
    while (scanner.hasNext()) {
        System.out.println(scanner.nextLine());
    }
} catch (FileNotFoundException fnfe) {
    fnfe.printStackTrace();
}
```

当然多个资源需要关闭的时候，使用 `try-with-resources` 实现起来也非常简单，如果你还是用`try-catch-finally`可能会带来很多问题。

通过使用分号分隔，可以在`try-with-resources`块中声明多个资源。

```java
try (BufferedInputStream bin = new BufferedInputStream(new FileInputStream(new File("test.txt")));
     BufferedOutputStream bout = new BufferedOutputStream(new FileOutputStream(new File("out.txt")))) {
    int b;
    while ((b = bin.read()) != -1) {
        bout.write(b);
    }
}
catch (IOException e) {
    e.printStackTrace();
}
```



## [反射](https://javaguide.cn/java/basis/java-basic-questions-03.html#反射)



## [注解](https://javaguide.cn/java/basis/java-basic-questions-03.html#注解)

### [注解的解析方法有哪几种？](https://javaguide.cn/java/basis/java-basic-questions-03.html#注解的解析方法有哪几种)

注解只有被解析之后才会生效，常见的解析方法有两种：

- **编译期直接扫描**：编译器在编译 Java 代码的时候扫描对应的注解并处理，比如某个方法使用`@Override` 注解，编译器在编译的时候就会检测当前的方法是否重写了父类对应的方法。
- **运行期通过反射处理**：像框架中自带的注解(比如 Spring 框架的 `@Value`、`@Component`)都是通过反射来进行处理的。



### [SPI 和 API 有什么区别？](https://javaguide.cn/java/basis/java-basic-questions-03.html#spi-和-api-有什么区别)

![img](https://oss.javaguide.cn/github/javaguide/java/basis/spi/1ebd1df862c34880bc26b9d494535b3dtplv-k3u1fbpfcp-watermark.png)





### [序列化和反序列化](https://javaguide.cn/java/basis/java-basic-questions-03.html#序列化和反序列化)

如果我们需要持久化 Java 对象比如将 Java 对象保存在文件中，或者在网络传输 Java 对象，这些场景都需要用到序列化。

简单来说：

- **序列化**：将数据结构或对象转换成二进制字节流的过程
- **反序列化**：将在序列化过程中所生成的二进制字节流转换成数据结构或者对象的过程

下面是序列化和反序列化常见应用场景：

- 对象在进行网络传输（比如远程方法调用 RPC 的时候）之前需要先被序列化，接收到序列化的对象之后需要再进行反序列化；
- 将对象存储到文件之前需要进行序列化，将对象从文件中读取出来需要进行反序列化；
- 将对象存储到数据库（如 Redis）之前需要用到序列化，将对象从缓存数据库中读取出来需要反序列化；
- 将对象存储到内存之前需要进行序列化，从内存中读取出来之后需要进行反序列化。

综上：**序列化的主要目的是通过网络传输对象或者说是将对象存储到文件系统、数据库、内存中。**

![img](https://oss.javaguide.cn/github/javaguide/a478c74d-2c48-40ae-9374-87aacf05188c.png)





**如果有些字段不想进行序列化怎么办？**

对于不想进行序列化的变量，可以使用 `transient` 关键字修饰。

`transient` 关键字的作用是：阻止实例中那些用此关键字修饰的的变量序列化；当对象被反序列化时，被 `transient` 修饰的变量值不会被持久化和恢复。

关于 `transient` 还有几点注意：

- `transient` 只能修饰变量，不能修饰类和方法。
- `transient` 修饰的变量，在反序列化后变量值将会被置成类型的默认值。例如，如果是修饰 `int` 类型，那么反序列后结果就是 `0`。
- `static` 变量因为不属于任何对象(Object)，所以无论有没有 `transient` 关键字修饰，均不会被序列化。



## [I/O](https://javaguide.cn/java/basis/java-basic-questions-03.html#i-o)

> [Java IO 基础知识总结 | JavaGuide](https://javaguide.cn/java/io/io-basis.html)



### [有哪些常见的 IO 模型?](#有哪些常见的-io-模型)

UNIX 系统下， IO 模型一共有 5 种：**同步阻塞 I/O**、**同步非阻塞 I/O**、**I/O 多路复用**、**信号驱动 I/O** 和**异步 I/O**。

这也是我们经常提到的 5 种 IO 模型。



### [Java 中 3 种常见 IO 模型](https://javaguide.cn/java/io/io-model.html#java-中-3-种常见-io-模型)

### [BIO (Blocking I/O)](#bio-blocking-i-o)

**BIO 属于同步阻塞 IO 模型** 。

同步阻塞 IO 模型中，应用程序发起 read 调用后，会一直阻塞，直到内核把数据拷贝到用户空间。

![图源：《深入拆解Tomcat & Jetty》](https://oss.javaguide.cn/p3-juejin/6a9e704af49b4380bb686f0c96d33b81~tplv-k3u1fbpfcp-watermark.png)图源：《深入拆解Tomcat & Jetty》

在客户端连接数量不高的情况下，是没问题的。但是，当面对十万甚至百万级连接的时候，传统的 BIO 模型是无能为力的。因此，我们需要一种更高效的 I/O 处理模型来应对更高的并发量。



### [NIO (Non-blocking/New I/O)](#nio-non-blocking-new-i-o)

Java 中的 NIO 于 Java 1.4 中引入，对应 `java.nio` 包，提供了 `Channel` , `Selector`，`Buffer` 等抽象。NIO 中的 N 可以理解为 Non-blocking，不单纯是 New。它是支持面向缓冲的，基于通道的 I/O 操作方法。 对于高负载、高并发的（网络）应用，应使用 NIO 。

Java 中的 NIO 可以看作是 **I/O 多路复用模型**。也有很多人认为，Java 中的 NIO 属于同步非阻塞 IO 模型。

我们先来看看 **同步非阻塞 IO 模型**。

![图源：《深入拆解Tomcat & Jetty》](https://oss.javaguide.cn/p3-juejin/bb174e22dbe04bb79fe3fc126aed0c61~tplv-k3u1fbpfcp-watermark.png)图源：《深入拆解Tomcat & Jetty》

同步非阻塞 IO 模型中，应用程序会一直发起 read 调用，等待数据从内核空间拷贝到用户空间的这段时间里，线程依然是阻塞的，直到在内核把数据拷贝到用户空间。

相比于同步阻塞 IO 模型，同步非阻塞 IO 模型确实有了很大改进。通过轮询操作，避免了一直阻塞。

但是，这种 IO 模型同样存在问题：**应用程序不断进行 I/O 系统调用轮询数据是否已经准备好的过程是十分消耗 CPU 资源的。**

这个时候，**I/O 多路复用模型** 就上场了。

![img](https://oss.javaguide.cn/github/javaguide/java/io/88ff862764024c3b8567367df11df6ab~tplv-k3u1fbpfcp-watermark.png)

IO 多路复用模型中，线程首先发起 select 调用，询问内核数据是否准备就绪，等内核把数据准备好了，用户线程再发起 read 调用。read 调用的过程（数据从内核空间 -> 用户空间）还是阻塞的。

> 目前支持 IO 多路复用的系统调用，有 select，epoll 等等。select 系统调用，目前几乎在所有的操作系统上都有支持。
>
> - **select 调用**：内核提供的系统调用，它支持一次查询多个系统调用的可用状态。几乎所有的操作系统都支持。
> - **epoll 调用**：linux 2.6 内核，属于 select 调用的增强版本，优化了 IO 的执行效率。

**IO 多路复用模型，通过减少无效的系统调用，减少了对 CPU 资源的消耗。**

Java 中的 NIO ，有一个非常重要的**选择器 ( Selector )** 的概念，也可以被称为 **多路复用器**。通过它，只需要一个线程便可以管理多个客户端连接。当客户端数据到了之后，才会为其服务。

![Buffer、Channel和Selector三者之间的关系](https://oss.javaguide.cn/github/javaguide/java/nio/channel-buffer-selector.png)Buffer、Channel和Selector三者之间的关系



### [AIO (Asynchronous I/O)](#aio-asynchronous-i-o)

AIO 也就是 NIO 2。Java 7 中引入了 NIO 的改进版 NIO 2,它是异步 IO 模型。

异步 IO 是基于事件和回调机制实现的，也就是应用操作之后会直接返回，不会堵塞在那里，当后台处理完成，操作系统会通知相应的线程进行后续的操作。

![img](https://oss.javaguide.cn/github/javaguide/java/io/3077e72a1af049559e81d18205b56fd7~tplv-k3u1fbpfcp-watermark.png)

目前来说 AIO 的应用还不是很广泛。Netty 之前也尝试使用过 AIO，不过又放弃了。这是因为，Netty 使用了 AIO 之后，在 Linux 系统上的性能并没有多少提升。

最后，来一张图，简单总结一下 Java 中的 BIO、NIO、AIO。

![BIO、NIO 和 AIO 对比](https://oss.javaguide.cn/github/javaguide/java/nio/bio-aio-nio.png)BIO、NIO 和 AIO 对比



### NIO核心知识总结

#### NIO 核心组件

NIO 主要包括以下三个核心组件：

- **Buffer（缓冲区）**：NIO 读写数据都是通过缓冲区进行操作的。读操作的时候将 Channel 中的数据填充到 Buffer 中，而写操作时将 Buffer 中的数据写入到 Channel 中。
- **Channel（通道）**：Channel 是一个双向的、可读可写的数据传输通道，NIO 通过 Channel 来实现数据的输入输出。通道是一个抽象的概念，它可以代表文件、套接字或者其他数据源之间的连接。
- **Selector（选择器）**：允许一个线程处理多个 Channel，基于事件驱动的 I/O 多路复用模型。所有的 Channel 都可以注册到 Selector 上，由 Selector 来分配线程来处理事件。

三者的关系如下图所示（暂时不理解没关系，后文会详细介绍）：

![Buffer、Channel和Selector三者之间的关系](https://oss.javaguide.cn/github/javaguide/java/nio/channel-buffer-selector.png)

#### [Buffer（缓冲区）](https://javaguide.cn/java/io/nio-basis.html#buffer-缓冲区)



#### [Channel（通道）](https://javaguide.cn/java/io/nio-basis.html#channel-通道)



#### [Selector（选择器）](https://javaguide.cn/java/io/nio-basis.html#selector-选择器)



#### [NIO 零拷贝](https://javaguide.cn/java/io/nio-basis.html#nio-零拷贝)



## [语法糖](https://javaguide.cn/java/basis/java-basic-questions-03.html#语法糖)

### [什么是语法糖？](#什么是语法糖)

**语法糖（Syntactic sugar）** 代指的是编程语言为了方便程序员开发程序而设计的一种特殊语法，这种语法对编程语言的功能并没有影响。实现相同的功能，基于语法糖写出来的代码往往更简单简洁且更易阅读。

举个例子，Java 中的 `for-each` 就是一个常用的语法糖，其原理其实就是基于普通的 for 循环和迭代器。



```java
String[] strs = {"JavaGuide", "公众号：JavaGuide", "博客：https://javaguide.cn/"};
for (String s : strs) {
    System.out.println(s);
}
```

不过，JVM 其实并不能识别语法糖，Java 语法糖要想被正确执行，需要先通过编译器进行解糖，也就是在程序编译阶段将其转换成 JVM 认识的基本语法。这也侧面说明，Java 中真正支持语法糖的是 Java 编译器而不是 JVM。如果你去看`com.sun.tools.javac.main.JavaCompiler`的源码，你会发现在`compile()`中有一个步骤就是调用`desugar()`，这个方法就是负责解语法糖的实现的。

### [Java 中有哪些常见的语法糖？](#java-中有哪些常见的语法糖)

Java 中最常用的语法糖主要有泛型、自动拆装箱、变长参数、枚举、内部类、增强 for 循环、try-with-resources 语法、lambda 表达式等。
