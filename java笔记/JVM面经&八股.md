# JVM面经&八股

------

> 大部分内容参考JavaGuide网站：https://javaguide.cn/java/jvm/



## Java内存区域详解（重点）

### [运行时数据区域](https://javaguide.cn/java/jvm/memory-area.html#运行时数据区域)

![Java 运行时数据区域（JDK1.7）](https://oss.javaguide.cn/github/javaguide/java/jvm/java-runtime-data-areas-jdk1.7.png)

![Java 运行时数据区域（JDK1.8 ）](https://oss.javaguide.cn/github/javaguide/java/jvm/java-runtime-data-areas-jdk1.8.png)

### jvm内存分布说一下

![图片](https://mmbiz.qpic.cn/sz_mmbiz_jpg/J0g14CUwaZee5LWu0KxeqiaibCYoHbhmhtTgSdYXUooyyibMdqlpibWGFK0ORIL12WCfekUdzbHNOk9XJUtXxCRrMA/640?wx_fmt=jpeg&from=appmsg&wxfrom=5&wx_lazy=1&wx_co=1)

JVM的内存结构主要分为以下几个部分：

- 程序计数器（Program Counter Register）：每个线程都有一个程序计数器。当线程执行 Java 方法时，程序计数器保存当前执行指令的地址，以便在 JVM 调用其他方法或恢复线程执行时重新回到正确的位置。
- Java 虚拟机栈（Java Virtual Machine Stacks）：每个线程都有一个虚拟机栈。虚拟机栈保存着方法执行期间的局部变量、操作数栈、方法出口等信息。线程每调用一个 Java 方法时，会创建一个栈帧（Stack Frame），栈帧包含着该方法的局部变量、操作数栈、方法返回地址等信息。栈帧在方法执行结束后会被弹出。
- 本地方法栈（Native Method Stack）：与 Java 虚拟机栈类似，但是为本地方法服务。
- Java 堆（Java Heap）：Java 堆是 Java 虚拟机中最大的一块内存区域，用于存储各种类型的对象实例，也是垃圾收集器的主要工作区域，Java 堆根据对象存活时间的不同，Java 堆还被分为年轻代、老年代两个区域，年轻代还被进一步划分为 Eden 区、From Survivor 0、To Survivor 1 区。
- 方法区（Method Area）：方法区也是所有线程共享的部分，它用于存储类的加载信息、静态变量、常量池、方法字节码等数据。在 Java 8 及以前的版本中，方法区被实现为永久代（Permanent Generation），在 Java 8 中被改为元空间（Metaspace）。



### **方法区为什么后来要移到元空间 ？**

> Java Guide 回答：
>
> 1、整个永久代有一个 JVM 本身设置的固定大小上限，无法进行调整（也就是受到 JVM 内存的限制），而元空间使用的是本地内存，受本机可用内存的限制，虽然元空间仍旧可能溢出，但是比原来出现的几率会更小。
>
> > 当元空间溢出时会得到如下错误：`java.lang.OutOfMemoryError: MetaSpace`
>
> 你可以使用 `-XX：MaxMetaspaceSize` 标志设置最大元空间大小，默认值为 unlimited，这意味着它只受系统内存的限制。`-XX：MetaspaceSize` 调整标志定义元空间的初始大小如果未指定此标志，则 Metaspace 将根据运行时的应用程序需求动态地重新调整大小。
>
> 2、元空间里面存放的是类的元数据，这样加载多少类的元数据就不由 `MaxPermSize` 控制了, 而由系统的实际可用空间来控制，这样能加载的类就更多了。
>
> 3、在 JDK8，合并 HotSpot 和 JRockit 的代码时, JRockit 从来没有一个叫永久代的东西, 合并之后就没有必要额外的设置这么一个永久代的地方了。
>
> 4、永久代会为 GC 带来不必要的复杂度，并且回收效率偏低。

方法区（Method Area）是所有线程共享的内存区域，用于存储已被虚拟机加载的类信息、常量、静态变量、即时编译器编译后的代码等数据。

方法区只是 JVM 规范中定义的一个概念，针对 Hotspot 虚拟机：

- Java8 之前使用永久代（Permanent Generation，简称 PermGen）实现
- 而 Java8 之后使用元空间（Metaspace）实现。

永久代问题：

- 内存溢出：永久代的空间是有限制的，可以通过 `-XX:PermSize` 设置永久代初始容量，通过`-XX:MaxPermSize` 设置永久代最大容量。但是当加载过多的类或者常量的时候，就可能导致永久代的空间不足，抛出 `java.lang.OutOfMemoryError: PermGen space` 异常。尤其是web应用会使用很多框架，这些框架会动态加载很多基础类，更容易导致OOM。
- **垃圾回收效率低下：**永久代中的类信息一般是在应用程序运行期间不会发生变化的，因此，如果开启了永久代的垃圾回收，就会造成大量的垃圾回收操作，导致垃圾回收效率低下，甚至会引起应用程序的暂停。此外，由于永久代主要存放 JVM 加载的类信息等永久存在的数据，这使得它在垃圾回收过程中的回收效率相对较低。在某些情况下，频繁触发的 Full GC 不仅无法有效回收永久代空间，还会严重影响 JVM 的性能。
- **无法动态调整大小：**永久代的大小一旦被设置，就无法动态调整，如果预估错误，就可能导致浪费内存或内存不足的问题。
- **无法回收常量池中的内存：**在永久代中，常量池是一个非常重要的部分，但是其中的常量无法被回收，即使这些常量已经不再被使用，也无法被垃圾回收器回收，这会浪费内存。

与永久代相比，使用元空间使用方法区具有以下优点：

- **突破内存限制，减少OOM。** 由于元空间使用的是本地内存，而不是 JVM 内存，因此理论上，其大小只受限于操作系统的实际可用内存。这大大减少了内存溢出的可能性。相较于永久代在 JVM 堆中预分配的有限空间，元空间的引入提供了更大的空间来存储类元数据。
- **提高 Full GC 的效率。** 在永久代中，Full GC 的触发比较频繁，而且效率较低。因为永久代中存放了很多 JVM 需要的类信息，这些数据大多数是不会被清理的，所以 Full GC 往往无法回收多少空间。但在元空间模型中，由于字符串常量池已移至堆中，静态变量也移至 Java 堆或者本地内存，因此可以更有效地进行垃圾回收，避免了因频繁的 Full GC 导致的性能影响。
- **满足不同的类加载需求和动态类加载的情况。** 在一些大型的、模块化的应用中，可能需要加载大量的类，这就需要大量的元数据存储空间。元空间可以动态地调整大小，能更好地满足这种需求。
- **避免永久代调优和大小设置的复杂性。** 在 Java8 之前的版本中，通常需要手动设置永久代的大小，以避免内存溢出的错误。这增加了应用的配置和管理的复杂性。而元空间使用本地内存，根据实际需求动态调整，大大简化了内存管理的复杂性。

总的来说，Java8 选择使用元空间（Metaspace）替代永久代（PermGen）是 JVM 内存模型的一次重大改进。解决了永久代面临的空间限制、低效的垃圾回收、以及复杂的内存管理等问题。元空间利用本地内存，能够动态调整大小，提供了更大的空间来存储类元数据，也更好地适应了大型、模块化应用的需求。



### [对象的创建](https://javaguide.cn/java/jvm/memory-area.html#对象的创建)



## JVM垃圾回收详解（重点）

> 常见面试题：
>
> - 如何判断对象是否死亡（两种方法）。
> - 简单的介绍一下强引用、软引用、弱引用、虚引用（虚引用与软引用和弱引用的区别、使用软引用能带来的好处）。
> - 如何判断一个常量是废弃常量
> - 如何判断一个类是无用的类
> - 垃圾收集有哪些算法，各自的特点？
> - HotSpot 为什么要分为新生代和老年代？
> - 常见的垃圾回收器有哪些？
> - 介绍一下 CMS,G1 收集器。
> - Minor Gc 和 Full GC 有什么不同呢？

![图片](https://mmbiz.qpic.cn/mmbiz_png/z40lCFUAHpkHxnyCaNz1ib6eAURPJVRIjNOgtKMgwjAHvjQYxflom6MAV4yg50Wcwg5tDnRianyg0KQicbLvYcwdQ/640?wx_fmt=png&from=appmsg&wxfrom=5&wx_lazy=1&wx_co=1)



### 有垃圾回收的是哪些地方？

垃圾回收主要是针对堆内存中的对象进行的，包括以下几个方面：

- **堆内存**：垃圾回收主要针对堆内存中不再被引用的对象进行回收，包括新生代和老年代中的对象。
- **永久代/元空间**：虚拟机中存放类的元数据信息的区域，也会进行垃圾回收，即对不再使用的类信息进行清理。
- **字符串常量池**：存放字符串常量的区域，也会进行垃圾回收，对不再被引用的字符串进行清理。



### [内存分配和回收原则](https://javaguide.cn/java/jvm/jvm-garbage-collection.html#内存分配和回收原则)



### [死亡对象判断方法](https://javaguide.cn/java/jvm/jvm-garbage-collection.html#死亡对象判断方法)

- [引用类型总结](https://javaguide.cn/java/jvm/jvm-garbage-collection.html#引用类型总结)

- [如何判断一个常量是废弃常量？](https://javaguide.cn/java/jvm/jvm-garbage-collection.html#如何判断一个常量是废弃常量)

- ![image-20240228171649466](D:\Desktop\Leetcode\java笔记\assets\image-20240228171649466.png)

  

### [垃圾收集器](https://javaguide.cn/java/jvm/jvm-garbage-collection.html#垃圾收集器)

- [CMS 收集器](https://javaguide.cn/java/jvm/jvm-garbage-collection.html#cms-收集器)
- [G1 收集器](https://javaguide.cn/java/jvm/jvm-garbage-collection.html#g1-收集器)

### [CMS 收集器](#cms-收集器)

**CMS（Concurrent Mark Sweep）收集器是一种以获取最短回收停顿时间为目标的收集器。它非常符合在注重用户体验的应用上使用。**

**CMS（Concurrent Mark Sweep）收集器是 HotSpot 虚拟机第一款真正意义上的并发收集器，它第一次实现了让垃圾收集线程与用户线程（基本上）同时工作。**

从名字中的**Mark Sweep**这两个词可以看出，CMS 收集器是一种 **“标记-清除”算法**实现的，它的运作过程相比于前面几种垃圾收集器来说更加复杂一些。整个过程分为四个步骤：

- **初始标记：** 暂停所有的其他线程，并记录下直接与 root 相连的对象，速度很快 ；
- **并发标记：** 同时开启 GC 和用户线程，用一个闭包结构去记录可达对象。但在这个阶段结束，这个闭包结构并不能保证包含当前所有的可达对象。因为用户线程可能会不断的更新引用域，所以 GC 线程无法保证可达性分析的实时性。所以这个算法里会跟踪记录这些发生引用更新的地方。
- **重新标记：** 重新标记阶段就是为了修正并发标记期间因为用户程序继续运行而导致标记产生变动的那一部分对象的标记记录，这个阶段的停顿时间一般会比初始标记阶段的时间稍长，远远比并发标记阶段时间短
- **并发清除：** 开启用户线程，同时 GC 线程开始对未标记的区域做清扫。

![CMS 收集器](https://oss.javaguide.cn/github/javaguide/java/jvm/cms-garbage-collector.png)

从它的名字就可以看出它是一款优秀的垃圾收集器，主要优点：**并发收集、低停顿**。但是它有下面三个明显的缺点：

- **对 CPU 资源敏感；**
- **无法处理浮动垃圾；**
- **它使用的回收算法-“标记-清除”算法会导致收集结束时会有大量空间碎片产生。**

**从 JDK9 开始，CMS 收集器已被弃用。**



### [G1 收集器](#g1-收集器)

**G1 (Garbage-First) 是一款面向服务器的垃圾收集器,主要针对配备多颗处理器及大容量内存的机器. 以极高概率满足 GC 停顿时间要求的同时,还具备高吞吐量性能特征.**

被视为 JDK1.7 中 HotSpot 虚拟机的一个重要进化特征。它具备以下特点：

- **并行与并发**：G1 能充分利用 CPU、多核环境下的硬件优势，使用多个 CPU（CPU 或者 CPU 核心）来缩短 Stop-The-World 停顿时间。部分其他收集器原本需要停顿 Java 线程执行的 GC 动作，G1 收集器仍然可以通过并发的方式让 java 程序继续执行。
- **分代收集**：虽然 G1 可以不需要其他收集器配合就能独立管理整个 GC 堆，但是还是保留了分代的概念。
- **空间整合**：与 CMS 的“标记-清除”算法不同，G1 从整体来看是基于“标记-整理”算法实现的收集器；从局部上来看是基于“标记-复制”算法实现的。
- **可预测的停顿**：这是 G1 相对于 CMS 的另一个大优势，降低停顿时间是 G1 和 CMS 共同的关注点，但 G1 除了追求低停顿外，还能建立可预测的停顿时间模型，能让使用者明确指定在一个长度为 M 毫秒的时间片段内，消耗在垃圾收集上的时间不得超过 N 毫秒。

G1 收集器的运作大致分为以下几个步骤：

- **初始标记**
- **并发标记**
- **最终标记**
- **筛选回收**

![G1 收集器](https://oss.javaguide.cn/github/javaguide/java/jvm/g1-garbage-collector.png)

**G1 收集器在后台维护了一个优先列表，每次根据允许的收集时间，优先选择回收价值最大的 Region(这也就是它的名字 Garbage-First 的由来)** 。这种使用 Region 划分内存空间以及有优先级的区域回收方式，保证了 G1 收集器在有限时间内可以尽可能高的收集效率（把内存化整为零）。

**从 JDK9 开始，G1 垃圾收集器成为了默认的垃圾收集器。**



## 类加载过程详解

### [类的生命周期](https://javaguide.cn/java/jvm/class-loading-process.html#类的生命周期)

![一个类的完整生命周期](https://oss.javaguide.cn/github/javaguide/java/jvm/lifecycle-of-a-class.png)



### [类加载过程](https://javaguide.cn/java/jvm/class-loading-process.html#类加载过程)

![类加载过程](https://oss.javaguide.cn/github/javaguide/java/jvm/class-loading-procedure.png)



### [类卸载](https://javaguide.cn/java/jvm/class-loading-process.html#类卸载)

> **卸载类即该类的 Class 对象被 GC。**
>
> 卸载类需要满足 3 个要求:
>
> 1. 该类的所有的实例对象都已被 GC，也就是说堆不存在该类的实例对象。
> 2. 该类没有在其他任何地方被引用
> 3. 该类的类加载器的实例已被 GC
>
> 所以，在 JVM 生命周期内，由 jvm 自带的类加载器加载的类是不会被卸载的。但是由我们自定义的类加载器加载的类是可能被卸载的。
>
> 只要想通一点就好了，JDK 自带的 `BootstrapClassLoader`, `ExtClassLoader`, `AppClassLoader` 负责加载 JDK 提供的类，所以它们(类加载器的实例)肯定不会被回收。而我们自定义的类加载器的实例是可以被回收的，所以使用我们自定义加载器加载的类是可以被卸载掉的。



## 类加载器详解（重点）

### [类加载器](https://javaguide.cn/java/jvm/classloader.html#类加载器)

> 类加载器是一个负责加载类的对象。`ClassLoader` 是一个抽象类。给定类的二进制名称，类加载器应尝试定位或生成构成类定义的数据。典型的策略是将名称转换为文件名，然后从文件系统中读取该名称的“类文件”。
>
> 每个 Java 类都有一个引用指向加载它的 `ClassLoader`。不过，数组类不是通过 `ClassLoader` 创建的，而是 JVM 在需要的时候自动创建的，数组类通过`getClassLoader()`方法获取 `ClassLoader` 的时候和该数组的元素类型的 `ClassLoader` 是一致的。



### [双亲委派模型](https://javaguide.cn/java/jvm/classloader.html#双亲委派模型)

> `ClassLoader` 类使用委托模型来搜索类和资源。每个 `ClassLoader` 实例都有一个相关的父类加载器。需要查找类或资源时，`ClassLoader` 实例会在试图亲自查找类或资源之前，将搜索类或资源的任务委托给其父类加载器。
>  虚拟机中被称为 "bootstrap class loader"的内置类加载器本身没有父类加载器，但是可以作为 `ClassLoader` 实例的父类加载器。

从上面的介绍可以看出：

- `ClassLoader` 类使用委托模型来搜索类和资源。
- 双亲委派模型要求除了顶层的启动类加载器外，其余的类加载器都应有自己的父类加载器。
- `ClassLoader` 实例会在试图亲自查找类或资源之前，将搜索类或资源的任务委托给其父类加载器。

下图展示的各种类加载器之间的层次关系被称为类加载器的“**双亲委派模型(Parents Delegation Model)**”。

![类加载器层次关系图](https://oss.javaguide.cn/github/javaguide/java/jvm/class-loader-parents-delegation-model.png)

其实这个双亲翻译的容易让别人误解，我们一般理解的双亲都是父母，这里的双亲更多地表达的是“父母这一辈”的人而已，并不是说真的有一个 `MotherClassLoader` 和一个`FatherClassLoader` 。个人觉得翻译成单亲委派模型更好一些，不过，国内既然翻译成了双亲委派模型并流传了，按照这个来也没问题，不要被误解了就好。

另外，类加载器之间的父子关系一般不是以继承的关系来实现的，而是通常使用组合关系来复用父加载器的代码。

```java
public abstract class ClassLoader {
  ...
  // 组合
  private final ClassLoader parent;
  protected ClassLoader(ClassLoader parent) {
       this(checkCreateClassLoader(), parent);
  }
  ...
}
```

在面向对象编程中，有一条非常经典的设计原则：组合优于继承，多用组合少用继承。

```java
protected Class<?> loadClass(String name, boolean resolve)
    throws ClassNotFoundException
{
    synchronized (getClassLoadingLock(name)) {
        //首先，检查该类是否已经加载过
        Class c = findLoadedClass(name);
        if (c == null) {
            //如果 c 为 null，则说明该类没有被加载过
            long t0 = System.nanoTime();
            try {
                if (parent != null) {
                    //当父类的加载器不为空，则通过父类的loadClass来加载该类
                    c = parent.loadClass(name, false);
                } else {
                    //当父类的加载器为空，则调用启动类加载器来加载该类
                    c = findBootstrapClassOrNull(name);
                }
            } catch (ClassNotFoundException e) {
                //非空父类的类加载器无法找到相应的类，则抛出异常
            }

            if (c == null) {
                //当父类加载器无法加载时，则调用findClass方法来加载该类
                //用户可通过覆写该方法，来自定义类加载器
                long t1 = System.nanoTime();
                c = findClass(name);

                //用于统计类加载器相关的信息
                sun.misc.PerfCounter.getParentDelegationTime().addTime(t1 - t0);
                sun.misc.PerfCounter.getFindClassTime().addElapsedTimeFrom(t1);
                sun.misc.PerfCounter.getFindClasses().increment();
            }
        }
        if (resolve) {
            //对类进行link操作
            resolveClass(c);
        }
        return c;
    }
}

```

结合上面的源码，简单总结一下双亲委派模型的执行流程：

- 在类加载的时候，系统会首先判断当前类是否被加载过。已经被加载的类会直接返回，否则才会尝试加载（每个父类加载器都会走一遍这个流程）。
- 类加载器在进行类加载的时候，它首先不会自己去尝试加载这个类，而是把这个请求委派给父类加载器去完成（调用父加载器 `loadClass()`方法来加载类）。这样的话，所有的请求最终都会传送到顶层的启动类加载器 `BootstrapClassLoader` 中。
- 只有当父加载器反馈自己无法完成这个加载请求（它的搜索范围中没有找到所需的类）时，子加载器才会尝试自己去加载（调用自己的 `findClass()` 方法来加载类）。
- 如果子类加载器也无法加载这个类，那么它会抛出一个 `ClassNotFoundException` 异常。

🌈 拓展一下：

**JVM 判定两个 Java 类是否相同的具体规则**：JVM 不仅要看类的全名是否相同，还要看加载此类的类加载器是否一样。只有两者都相同的情况，才认为两个类是相同的。即使两个类来源于同一个 `Class` 文件，被同一个虚拟机加载，只要加载它们的类加载器不同，那这两个类就必定不相同。



### [双亲委派模型的好处](#双亲委派模型的好处)

双亲委派模型保证了 Java 程序的稳定运行，可以避免类的重复加载（JVM 区分不同类的方式不仅仅根据类名，相同的类文件被不同的类加载器加载产生的是两个不同的类），也保证了 Java 的核心 API 不被篡改。

如果没有使用双亲委派模型，而是每个类加载器加载自己的话就会出现一些问题，比如我们编写一个称为 `java.lang.Object` 类的话，那么程序运行的时候，系统就会出现两个不同的 `Object` 类。双亲委派模型可以保证加载的是 JRE 里的那个 `Object` 类，而不是你写的 `Object` 类。这是因为 `AppClassLoader` 在加载你的 `Object` 类时，会委托给 `ExtClassLoader` 去加载，而 `ExtClassLoader` 又会委托给 `BootstrapClassLoader`，`BootstrapClassLoader` 发现自己已经加载过了 `Object` 类，会直接返回，不会去加载你写的 `Object` 类。



### [打破双亲委派模型方法](https://javaguide.cn/java/jvm/classloader.html#打破双亲委派模型方法)

我们比较熟悉的 Tomcat 服务器为了能够优先加载 Web 应用目录下的类，然后再加载其他目录下的类，就自定义了类加载器 `WebAppClassLoader` 来打破双亲委托机制。这也是 Tomcat 下 Web 应用之间的类实现隔离的具体原理。

Tomcat 的类加载器的层次结构如下：

![Tomcat 的类加载器的层次结构](https://oss.javaguide.cn/github/javaguide/java/jvm/tomcat-class-loader-parents-delegation-model.png)





## JVM线上问题排查和性能调优案例

https://javaguide.cn/java/jvm/jvm-in-action.html