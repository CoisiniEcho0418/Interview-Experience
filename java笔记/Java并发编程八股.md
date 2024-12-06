# Java并发编程八股

------

## [什么是线程和进程?](https://javaguide.cn/java/concurrent/java-concurrent-questions-01.html#什么是线程和进程)



## [说说线程的生命周期和状态?](https://javaguide.cn/java/concurrent/java-concurrent-questions-01.html#说说线程的生命周期和状态)

![Java 线程状态变迁图](https://oss.javaguide.cn/github/javaguide/java/concurrent/640.png)



## java中多线程的生命周期

 **就绪状态：**就绪状态的线程又叫做可运行状态，表示当前线程具有抢夺CPU时间片的权力（CPU时间片就是执行权）。当一个线程抢夺到CPU时间片之后，就开始执行run方法，run方法的开始执行标志着线程进入运行状态。

**运行状态：**run方法的开始执行标志着这个线程进入运行状态，当之前占有的CPU时间片用完之后，会重新回到就绪状态继续抢夺CPU时间片，当再次抢到CPU时间之后，会重新进入run方法接着上一次的代码继续往下执行。

**阻塞状态：**当一个线程遇到阻塞事件，例如接收用户键盘输入，或者sleep方法等，此时线程会进入阻塞状态，阻塞状态的线程会放弃之前占有的CPU时间片。之前的时间片没了需要再次回到就绪状态抢夺CPU时间片。

**锁池：**在这里找共享对象的对象锁线程进入锁池找共享对象的对象锁的时候，会释放之前占有CPU时间片，有可能找到了，有可能没找到，没找到则在锁池中等待，如果找到了会进入就绪状态继续抢夺CPU时间片。（这个进入锁池，可以理解为一种阻塞状态）



## [为什么 wait() 方法不定义在 Thread 中？](https://javaguide.cn/java/concurrent/java-concurrent-questions-01.html#为什么-wait-方法不定义在-thread-中)

`wait()` 是让获得对象锁的线程实现等待，会自动释放当前线程占有的对象锁。每个对象（`Object`）都拥有对象锁，既然要释放当前线程占有的对象锁并让其进入 WAITING 状态，自然是要操作对应的对象（`Object`）而非当前的线程（`Thread`）。

类似的问题：**为什么 `sleep()` 方法定义在 `Thread` 中？**

因为 `sleep()` 是让当前线程暂停执行，不涉及到对象类，也不需要获得对象锁。



## [可以直接调用 Thread 类的 run 方法吗？](https://javaguide.cn/java/concurrent/java-concurrent-questions-01.html#可以直接调用-thread-类的-run-方法吗)

这是另一个非常经典的 Java 多线程面试问题，而且在面试中会经常被问到。很简单，但是很多人都会答不上来！

new 一个 `Thread`，线程进入了新建状态。调用 `start()`方法，会启动一个线程并使线程进入了就绪状态，当分配到时间片后就可以开始运行了。 `start()` 会执行线程的相应准备工作，然后自动执行 `run()` 方法的内容，这是真正的多线程工作。 但是，直接执行 `run()` 方法，会把 `run()` 方法当成一个 main 线程下的普通方法去执行，并不会在某个线程中执行它，所以这并不是多线程工作。

**总结：调用 `start()` 方法方可启动线程并使线程进入就绪状态，直接执行 `run()` 方法的话不会以多线程的方式执行。**



## 创建线程的五种方式

> 1、继承于Thread类
>
> 2、方式二：实现Runnable接口
>
> 3、方式三：实现Callable接口
>
> 4、方式四：使用线程池
>
> 5、方式五：使用匿名类

参考资料：

[java创建线程（Thread）的5种方式_java new thread-CSDN博客](https://blog.csdn.net/weixin_43824267/article/details/112706385)

[java手写题汇总(建议收藏)_牛客网 (nowcoder.com)](https://www.nowcoder.com/discuss/650808006875127808?sourceSSR=users)



## Synchronized的底层原理是什么？锁升级的过程了解吗？

- **底层实现**：Synchronized关键字底层是使用monitor对象锁实现的，每一个对象关联一个monitor对象，而monitor对象可以看成是一个对象锁，它采用互斥的方式让同一时刻至多只有一个线程能持有对象锁，其他线程再想获取这个对 象锁时会被阻塞住，这样就能保证拥有锁的线程可以安全的执行临界区的代码。
- **锁升级**：是指JVM根据锁的竞争情况和对象的状态，将对象的锁从偏向锁、轻量级锁升级为重量级锁的过程。偏向锁是指针对无竞争的情况下，锁会偏向于第一个获取锁的线程；轻量级锁是指针对短时间内只有一个线程竞争锁的情况下，使用CAS操作来避免阻塞；重量级锁是指多个线程竞争同一个锁时，通过操作系统的互斥量来实现线程阻塞和唤醒。锁升级的过程是为了提高多线程并发访问的效率和性能。



### 线程是怎么确定拿到锁的？

线程确定拿到锁的过程:是通过检查锁的状态并尝试获取锁来实现的。在JVM中，锁信息具体是存放在Java对象头中的。

当一个线程尝试进入synchronized代码块或方法时，JVM会检查对应对象的锁状态。如果对象的锁未被其他线程持有，即锁状态为可获取，那么该线程将成功获取锁并进入临界区执行代码。

### 锁信息具体放到哪的？

锁的状态信息是**Java对象头中的 Mark Word 字段**，这个字段对象关于锁的信息、垃圾回收信息等。

![图片](https://mmbiz.qpic.cn/mmbiz_png/J0g14CUwaZfJOPQiaCz5OIDZrhtDIiajoPWc9CDTPV50ULbulg3Aph7rHGibdCbtsgiaFiaGELcNeCYojvK2jpxibl3A/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

JVM通过操作对象的头部信息来实现锁的获取、释放以及等待队列的管理。当线程成功获取锁后，对象的头部信息会被更新为当前线程的标识，表示该线程拥有了这个锁。

其他线程在尝试获取同一个锁时，会检查对象的头部信息，如果锁已经被其他线程持有，它们将会被阻塞直到锁被释放。





## [volatile 关键字](https://javaguide.cn/java/concurrent/java-concurrent-questions-02.html#volatile-关键字)

以一个常见的面试题为例讲解一下 `volatile` 关键字禁止指令重排序的效果。

面试中面试官经常会说：“单例模式了解吗？来给我手写一下！给我解释一下双重检验锁方式实现单例模式的原理呗！”

**双重校验锁实现对象单例（线程安全）**：

```java
public class Singleton {

    private volatile static Singleton uniqueInstance;

    private Singleton() {
    }

    public  static Singleton getUniqueInstance() {
       //先判断对象是否已经实例过，没有实例化过才进入加锁代码
        if (uniqueInstance == null) {
            //类对象加锁
            synchronized (Singleton.class) {
                if (uniqueInstance == null) {
                    uniqueInstance = new Singleton();
                }
            }
        }
        return uniqueInstance;
    }
}

```

`uniqueInstance` 采用 `volatile` 关键字修饰也是很有必要的， `uniqueInstance = new Singleton();` 这段代码其实是分为三步执行：

1. 为 `uniqueInstance` 分配内存空间
2. 初始化 `uniqueInstance`
3. 将 `uniqueInstance` 指向分配的内存地址

但是由于 JVM 具有指令重排的特性，执行顺序有可能变成 1->3->2。指令重排在单线程环境下不会出现问题，但是在多线程环境下会导致一个线程获得还没有初始化的实例。例如，线程 T1 执行了 1 和 3，此时 T2 调用 `getUniqueInstance`() 后发现 `uniqueInstance` 不为空，因此返回 `uniqueInstance`，但此时 `uniqueInstance` 还未被初始化。



### [volatile 可以保证原子性么？](https://javaguide.cn/java/concurrent/java-concurrent-questions-02.html#volatile-可以保证原子性么)



### Volatile 和synchronized的区别是什么？

作用：

- synchronized 表示只有一个线程可以获取作用对象的锁，执行代码，阻塞其他线程。
- volatile 表示变量在 CPU 的寄存器中是不确定的，必须从主存中读取。保证多线程环境下变量的可见性；禁止指令重排序。

区别：

- volatile不需要加锁，比synchronized更轻量级，不会阻塞线程；
- 从内存可见性角度，volatile读相当于加锁，volatile写相当于解锁；
- synchronized既能够保证可见性，又能保证原子性，而volatile只能保证可见性，无法保证原子性。



### volatile关键字是如何保证内存可见性的？底层是怎么实现的？

volatile关键字通过两种机制来保证内存可见性：

- **禁止指令重排序**：在程序运行时，为了提高性能，编译器和处理器可能会对指令进行重排序，这可能会导致变量的更新操作被延迟执行或者乱序执行，从而使得其他线程无法立即看到最新的值。使用volatile关键字修饰的变量会禁止指令重排序，保证变量的更新操作按照代码顺序执行。
- **内存屏障（Memory Barrier）**：在多核处理器架构下，每个线程都有自己的缓存，volatile关键字会在写操作后插入写屏障（Write Barrier），在读操作前插入读屏障（Read Barrier），确保变量的更新能够立即被其他线程看到，保证内存可见性。

通过禁止指令重排序和插入内存屏障，volatile关键字能够保证被修饰变量的更新操作对其他线程是可见的，从而有效解决了多线程环境下的内存可见性问题。

### 为什么需要保证内存可见性？

如果不保证内存可见性，就会出现数据脏读，一个线程修改了共享变量的值，但其他线程无法立即看到最新值，导致其他线程读取到了过期数据，从而产生错误的结果。

通过保证内存可见性，避免数据不一致性和并发访问带来的问题，保证程序的正确性和稳定性。



## [乐观锁和悲观锁](https://javaguide.cn/java/concurrent/java-concurrent-questions-02.html#乐观锁和悲观锁)

### [什么是悲观锁？](#什么是悲观锁)

悲观锁总是假设最坏的情况，认为共享资源每次被访问的时候就会出现问题(比如共享数据被修改)，所以每次在获取资源操作的时候都会上锁，这样其他线程想拿到这个资源就会阻塞直到锁被上一个持有者释放。也就是说，**共享资源每次只给一个线程使用，其它线程阻塞，用完后再把资源转让给其它线程**。

像 Java 中`synchronized`和`ReentrantLock`等独占锁就是悲观锁思想的实现。

```java
public void performSynchronisedTask() {
    synchronized (this) {
        // 需要同步的操作
    }
}

private Lock lock = new ReentrantLock();
lock.lock();
try {
   // 需要同步的操作
} finally {
    lock.unlock();
}
```

高并发的场景下，激烈的锁竞争会造成线程阻塞，大量阻塞线程会导致系统的上下文切换，增加系统的性能开销。并且，悲观锁还可能会存在死锁问题，影响代码的正常运行。



### [什么是乐观锁？](#什么是乐观锁)

乐观锁总是假设最好的情况，认为共享资源每次被访问的时候不会出现问题，线程可以不停地执行，无需加锁也无需等待，只是在提交修改的时候去验证对应的资源（也就是数据）是否被其它线程修改了（具体方法可以使用版本号机制或 CAS 算法）。

在 Java 中`java.util.concurrent.atomic`包下面的原子变量类（比如`AtomicInteger`、`LongAdder`）就是使用了乐观锁的一种实现方式 **CAS** 实现的。

```java
// LongAdder 在高并发场景下会比 AtomicInteger 和 AtomicLong 的性能更好
// 代价就是会消耗更多的内存空间（空间换时间）
LongAdder sum = new LongAdder();
sum.increment();
```

高并发的场景下，乐观锁相比悲观锁来说，不存在锁竞争造成线程阻塞，也不会有死锁的问题，在性能上往往会更胜一筹。但是，如果冲突频繁发生（写占比非常多的情况），会频繁失败和重试，这样同样会非常影响性能，导致 CPU 飙升。

理论上来说：

- 悲观锁通常多用于写比较多的情况（多写场景，竞争激烈），这样可以避免频繁失败和重试影响性能，悲观锁的开销是固定的。不过，如果乐观锁解决了频繁失败和重试这个问题的话（比如`LongAdder`），也是可以考虑使用乐观锁的，要视实际情况而定。
- 乐观锁通常多用于写比较少的情况（多读场景，竞争较少），这样可以避免频繁加锁影响性能。不过，乐观锁主要针对的对象是单个共享变量（参考`java.util.concurrent.atomic`包下面的原子变量类）。



### [如何实现乐观锁？](#如何实现乐观锁)

乐观锁一般会使用版本号机制或 CAS 算法实现，CAS 算法相对来说更多一些，这里需要格外注意。

#### [版本号机制](#版本号机制)

一般是在数据表中加上一个数据版本号 `version` 字段，表示数据被修改的次数。当数据被修改时，`version` 值会加一。当线程 A 要更新数据值时，在读取数据的同时也会读取 `version` 值，在提交更新时，若刚才读取到的 version 值为当前数据库中的 `version` 值相等时才更新，否则重试更新操作，直到更新成功。

**举一个简单的例子**：假设数据库中帐户信息表中有一个 version 字段，当前值为 1 ；而当前帐户余额字段（ `balance` ）为 $100 。

1. 操作员 A 此时将其读出（ `version`=1 ），并从其帐户余额中扣除 $50（ $100-$50 ）。
2. 在操作员 A 操作的过程中，操作员 B 也读入此用户信息（ `version`=1 ），并从其帐户余额中扣除 $20 （ $100-$20 ）。
3. 操作员 A 完成了修改工作，将数据版本号（ `version`=1 ），连同帐户扣除后余额（ `balance`=$50 ），提交至数据库更新，此时由于提交数据版本等于数据库记录当前版本，数据被更新，数据库记录 `version` 更新为 2 。
4. 操作员 B 完成了操作，也将版本号（ `version`=1 ）试图向数据库提交数据（ `balance`=$80 ），但此时比对数据库记录版本时发现，操作员 B 提交的数据版本号为 1 ，数据库记录当前版本也为 2 ，不满足 “ 提交版本必须等于当前版本才能执行更新 “ 的乐观锁策略，因此，操作员 B 的提交被驳回。

这样就避免了操作员 B 用基于 `version`=1 的旧数据修改的结果覆盖操作员 A 的操作结果的可能。

#### [CAS 算法](#cas-算法)

CAS 的全称是 **Compare And Swap（比较与交换）** ，用于实现乐观锁，被广泛应用于各大框架中。CAS 的思想很简单，就是用一个预期值和要更新的变量值进行比较，两值相等才会进行更新。

CAS 是一个原子操作，底层依赖于一条 CPU 的原子指令。

> **原子操作** 即最小不可拆分的操作，也就是说操作一旦开始，就不能被打断，直到操作完成。

CAS 涉及到三个操作数：

- **V**：要更新的变量值(Var)
- **E**：预期值(Expected)
- **N**：拟写入的新值(New)

当且仅当 V 的值等于 E 时，CAS 通过原子方式用新值 N 来更新 V 的值。如果不等，说明已经有其它线程更新了 V，则当前线程放弃更新。

**举一个简单的例子**：线程 A 要修改变量 i 的值为 6，i 原值为 1（V = 1，E=1，N=6，假设不存在 ABA 问题）。

1. i 与 1 进行比较，如果相等， 则说明没被其他线程修改，可以被设置为 6 。
2. i 与 1 进行比较，如果不相等，则说明被其他线程修改，当前线程放弃更新，CAS 操作失败。

当多个线程同时使用 CAS 操作一个变量时，只有一个会胜出，并成功更新，其余均会失败，但失败的线程并不会被挂起，仅是被告知失败，并且允许再次尝试，当然也允许失败的线程放弃操作。

Java 语言并没有直接实现 CAS，CAS 相关的实现是通过 C++ 内联汇编的形式实现的（JNI 调用）。因此， CAS 的具体实现和操作系统以及 CPU 都有关系。

`sun.misc`包下的`Unsafe`类提供了`compareAndSwapObject`、`compareAndSwapInt`、`compareAndSwapLong`方法来实现的对`Object`、`int`、`long`类型的 CAS 操作。



### [乐观锁存在哪些问题？](#乐观锁存在哪些问题)

ABA 问题是乐观锁最常见的问题。

#### [ABA 问题](#aba-问题)

如果一个变量 V 初次读取的时候是 A 值，并且在准备赋值的时候检查到它仍然是 A 值，那我们就能说明它的值没有被其他线程修改过了吗？很明显是不能的，因为在这段时间它的值可能被改为其他值，然后又改回 A，那 CAS 操作就会误认为它从来没有被修改过。这个问题被称为 CAS 操作的 **"ABA"问题。**

ABA 问题的解决思路是在变量前面追加上**版本号或者时间戳**。JDK 1.5 以后的 `AtomicStampedReference` 类就是用来解决 ABA 问题的，其中的 `compareAndSet()` 方法就是首先检查当前引用是否等于预期引用，并且当前标志是否等于预期标志，如果全部相等，则以原子方式将该引用和该标志的值设置为给定的更新值。

#### [循环时间长开销大](https://javaguide.cn/java/concurrent/java-concurrent-questions-02.html#循环时间长开销大)

CAS 经常会用到自旋操作来进行重试，也就是不成功就一直循环执行直到成功。如果长时间不成功，会给 CPU 带来非常大的执行开销。

#### [只能保证一个共享变量的原子操作](#只能保证一个共享变量的原子操作)

CAS 只对单个共享变量有效，当操作涉及跨多个共享变量时 CAS 无效。但是从 JDK 1.5 开始，提供了`AtomicReference`类来保证引用对象之间的原子性，你可以把多个变量放在一个对象里来进行 CAS 操作.所以我们可以使用锁或者利用`AtomicReference`类把多个共享变量合并成一个共享变量来操作。



### 乐观锁如何实现，有哪些缺点？(面试题)

观锁通过在更新数据时先检查数据版本号（或时间戳等），若版本号匹配则更新数据，否则拒绝更新，实现并发控制。实现方式包括：

1. 添加版本字段：在数据表中增加一个版本字段，每次更新数据时增加版本号。
2. 检查版本号：在更新数据时，先查询当前版本号，然后在更新时检查版本号是否匹配。

乐观锁的缺点是在高并发场景下，冲突较多，可能导致大量的重试操作。所以，在数据竞争非常激烈的环境中，乐观锁可能不太适用。



## [synchronized 关键字](https://javaguide.cn/java/concurrent/java-concurrent-questions-02.html#synchronized-关键字)

### [如何使用 synchronized？](#如何使用-synchronized)

`synchronized` 关键字的使用方式主要有下面 3 种：

1. 修饰实例方法
2. 修饰静态方法
3. 修饰代码块

**1、修饰实例方法** （锁当前对象实例）

给当前对象实例加锁，进入同步代码前要获得 **当前对象实例的锁** 。

```java
synchronized void method() {
    //业务代码
}
```

**2、修饰静态方法** （锁当前类）

给当前类加锁，会作用于类的所有对象实例 ，进入同步代码前要获得 **当前 class 的锁**。

这是因为静态成员不属于任何一个实例对象，归整个类所有，不依赖于类的特定实例，被类的所有实例共享。

```java
synchronized static void method() {
    //业务代码
}
```

静态 `synchronized` 方法和非静态 `synchronized` 方法之间的调用互斥么？不互斥！如果一个线程 A 调用一个实例对象的非静态 `synchronized` 方法，而线程 B 需要调用这个实例对象所属类的静态 `synchronized` 方法，是允许的，不会发生互斥现象，因为访问静态 `synchronized` 方法占用的锁是当前类的锁，而访问非静态 `synchronized` 方法占用的锁是当前实例对象锁。

**3、修饰代码块** （锁指定对象/类）

对括号里指定的对象/类加锁：

- `synchronized(object)` 表示进入同步代码库前要获得 **给定对象的锁**。
- `synchronized(类.class)` 表示进入同步代码前要获得 **给定 Class 的锁**

```java
synchronized(this) {
    //业务代码
}
```

**总结：**

- `synchronized` 关键字加到 `static` 静态方法和 `synchronized(class)` 代码块上都是是给 Class 类上锁；
- `synchronized` 关键字加到实例方法上是给对象实例上锁；
- 尽量不要使用 `synchronized(String a)` 因为 JVM 中，字符串常量池具有缓存功能。



### [synchronized 底层原理了解吗？](https://javaguide.cn/java/concurrent/java-concurrent-questions-02.html#synchronized-底层原理了解吗)

![执行 monitorenter 获取锁](https://oss.javaguide.cn/github/javaguide/java/concurrent/synchronized-get-lock-code-block.png)

![执行 monitorexit 释放锁](https://oss.javaguide.cn/github/javaguide/java/concurrent/synchronized-release-lock-block.png)

> `synchronized` 同步语句块的实现使用的是 `monitorenter` 和 `monitorexit` 指令，其中 `monitorenter` 指令指向同步代码块的开始位置，`monitorexit` 指令则指明同步代码块的结束位置。
>
> `synchronized` 修饰的方法并没有 `monitorenter` 指令和 `monitorexit` 指令，取得代之的确实是 `ACC_SYNCHRONIZED` 标识，该标识指明了该方法是一个同步方法。



### [synchronized 和 volatile 有什么区别？](#synchronized-和-volatile-有什么区别)

`synchronized` 关键字和 `volatile` 关键字是两个互补的存在，而不是对立的存在！

- `volatile` 关键字是线程同步的轻量级实现，所以 `volatile`性能肯定比`synchronized`关键字要好 。但是 `volatile` 关键字只能用于变量而 `synchronized` 关键字可以修饰方法以及代码块 。
- `volatile` 关键字能保证数据的可见性，但不能保证数据的原子性。`synchronized` 关键字两者都能保证。
- `volatile`关键字主要用于解决变量在多个线程之间的可见性，而 `synchronized` 关键字解决的是多个线程之间访问资源的同步性。



## [ReentrantLock](#reentrantlock)

### [ReentrantLock 是什么？](#reentrantlock-是什么)

`ReentrantLock` 实现了 `Lock` 接口，是一个可重入且独占式的锁，和 `synchronized` 关键字类似。不过，`ReentrantLock` 更灵活、更强大，增加了轮询、超时、中断、公平锁和非公平锁等高级功能。



`ReentrantLock` 里面有一个内部类 `Sync`，`Sync` 继承 AQS（`AbstractQueuedSynchronizer`），添加锁和释放锁的大部分操作实际上都是在 `Sync` 中实现的。`Sync` 有公平锁 `FairSync` 和非公平锁 `NonfairSync` 两个子类。

`ReentrantLock` 默认使用非公平锁，也可以通过构造器来显式的指定使用公平锁。

```java
// 传入一个 boolean 值，true 时为公平锁，false 时为非公平锁
public ReentrantLock(boolean fair) {
    sync = fair ? new FairSync() : new NonfairSync();
}
```



### [公平锁和非公平锁有什么区别？](#公平锁和非公平锁有什么区别)

- **公平锁** : 锁被释放之后，先申请的线程先得到锁。性能较差一些，因为公平锁为了保证时间上的绝对顺序，上下文切换更频繁。
- **非公平锁**：锁被释放之后，后申请的线程可能会先获取到锁，是随机或者按照其他优先级排序的。性能更好，但可能会导致某些线程永远无法获取到锁。



### [synchronized 和 ReentrantLock 有什么区别？](#synchronized-和-reentrantlock-有什么区别)

> synchronized 和 ReentrantLock 都是 Java 中提供的可重入锁：
>
> - **用法不同**：synchronized 可用来修饰普通方法、静态方法和代码块，而 ReentrantLock 只能用在代码块上。
> - **获取锁和释放锁方式不同**：synchronized 会自动加锁和释放锁，当进入 synchronized 修饰的代码块之后会自动加锁，当离开 synchronized 的代码段之后会自动释放锁。而 ReentrantLock 需要手动加锁和释放锁
> - **锁类型不同**：synchronized 属于非公平锁，而 ReentrantLock 既可以是公平锁也可以是非公平锁。
> - **响应中断不同**：ReentrantLock 可以响应中断，解决死锁的问题，而 synchronized 不能响应中断。
> - **底层实现不同**：synchronized 是 JVM 层面通过监视器实现的，而 ReentrantLock 是基于 AQS 实现的。

#### [两者都是可重入锁](#两者都是可重入锁)

**可重入锁** 也叫递归锁，指的是线程可以再次获取自己的内部锁。比如一个线程获得了某个对象的锁，此时这个对象锁还没有释放，当其再次想要获取这个对象的锁的时候还是可以获取的，如果是不可重入锁的话，就会造成死锁。

JDK 提供的所有现成的 `Lock` 实现类，包括 `synchronized` 关键字锁都是可重入的。

在下面的代码中，`method1()` 和 `method2()`都被 `synchronized` 关键字修饰，`method1()`调用了`method2()`。

```java
public class SynchronizedDemo {
    public synchronized void method1() {
        System.out.println("方法1");
        method2();
    }

    public synchronized void method2() {
        System.out.println("方法2");
    }
}
```

由于 `synchronized`锁是可重入的，同一个线程在调用`method1()` 时可以直接获得当前对象的锁，执行 `method2()` 的时候可以再次获取这个对象的锁，不会产生死锁问题。假如`synchronized`是不可重入锁的话，由于该对象的锁已被当前线程所持有且无法释放，这就导致线程在执行 `method2()`时获取锁失败，会出现死锁问题。



#### [ReentrantLock 比 synchronized 增加了一些高级功能](#reentrantlock-比-synchronized-增加了一些高级功能)

相比`synchronized`，`ReentrantLock`增加了一些高级功能。主要来说主要有三点：

- **等待可中断** : `ReentrantLock`提供了一种能够中断等待锁的线程的机制，通过 `lock.lockInterruptibly()` 来实现这个机制。也就是说正在等待的线程可以选择放弃等待，改为处理其他事情。
- **可实现公平锁** : `ReentrantLock`可以指定是公平锁还是非公平锁。而`synchronized`只能是非公平锁。所谓的公平锁就是先等待的线程先获得锁。`ReentrantLock`默认情况是非公平的，可以通过 `ReentrantLock`类的`ReentrantLock(boolean fair)`构造方法来指定是否是公平的。
- **可实现选择性通知（锁可以绑定多个条件）**: `synchronized`关键字与`wait()`和`notify()`/`notifyAll()`方法相结合可以实现等待/通知机制。`ReentrantLock`类当然也可以实现，但是需要借助于`Condition`接口与`newCondition()`方法。

关于 `Condition`接口的补充：

> `Condition`是 JDK1.5 之后才有的，它具有很好的灵活性，比如可以实现多路通知功能也就是在一个`Lock`对象中可以创建多个`Condition`实例（即对象监视器），**线程对象可以注册在指定的`Condition`中，从而可以有选择性的进行线程通知，在调度线程上更加灵活。 在使用`notify()/notifyAll()`方法进行通知时，被通知的线程是由 JVM 选择的，用`ReentrantLock`类结合`Condition`实例可以实现“选择性通知”** ，这个功能非常重要，而且是 `Condition` 接口默认提供的。而`synchronized`关键字就相当于整个 `Lock` 对象中只有一个`Condition`实例，所有的线程都注册在它一个身上。如果执行`notifyAll()`方法的话就会通知所有处于等待状态的线程，这样会造成很大的效率问题。而`Condition`实例的`signalAll()`方法，只会唤醒注册在该`Condition`实例中的所有等待线程。



### [可中断锁和不可中断锁有什么区别？](#可中断锁和不可中断锁有什么区别)

- **可中断锁**：获取锁的过程中可以被中断，不需要一直等到获取锁之后 才能进行其他逻辑处理。`ReentrantLock` 就属于是可中断锁。
- **不可中断锁**：一旦线程申请了锁，就只能等到拿到锁以后才能进行其他的逻辑处理。 `synchronized` 就属于是不可中断锁。



## [ThreadLocal](#threadlocal)

### [ThreadLocal 有什么用？](#threadlocal-有什么用)

通常情况下，我们创建的变量是可以被任何一个线程访问并修改的。**如果想实现每一个线程都有自己的专属本地变量该如何解决呢？**

JDK 中自带的`ThreadLocal`类正是为了解决这样的问题。 **`ThreadLocal`类主要解决的就是让每个线程绑定自己的值，可以将`ThreadLocal`类形象的比喻成存放数据的盒子，盒子中可以存储每个线程的私有数据。**

如果你创建了一个`ThreadLocal`变量，那么访问这个变量的每个线程都会有这个变量的本地副本，这也是`ThreadLocal`变量名的由来。他们可以使用 `get()` 和 `set()` 方法来获取默认值或将其值更改为当前线程所存的副本的值，从而避免了线程安全问题。

再举个简单的例子：两个人去宝屋收集宝物，这两个共用一个袋子的话肯定会产生争执，但是给他们两个人每个人分配一个袋子的话就不会出现这样的问题。如果把这两个人比作线程的话，那么 ThreadLocal 就是用来避免这两个线程竞争的。



### [如何使用 ThreadLocal？](https://javaguide.cn/java/concurrent/java-concurrent-questions-03.html#如何使用-threadlocal)

相信看了上面的解释，大家已经搞懂 `ThreadLocal` 类是个什么东西了。下面简单演示一下如何在项目中实际使用 `ThreadLocal` 。

```java
import java.text.SimpleDateFormat;
import java.util.Random;

public class ThreadLocalExample implements Runnable{

     // SimpleDateFormat 不是线程安全的，所以每个线程都要有自己独立的副本
    private static final ThreadLocal<SimpleDateFormat> formatter = ThreadLocal.withInitial(() -> new SimpleDateFormat("yyyyMMdd HHmm"));

    public static void main(String[] args) throws InterruptedException {
        ThreadLocalExample obj = new ThreadLocalExample();
        for(int i=0 ; i<10; i++){
            Thread t = new Thread(obj, ""+i);
            Thread.sleep(new Random().nextInt(1000));
            t.start();
        }
    }

    @Override
    public void run() {
        System.out.println("Thread Name= "+Thread.currentThread().getName()+" default Formatter = "+formatter.get().toPattern());
        try {
            Thread.sleep(new Random().nextInt(1000));
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
        //formatter pattern is changed here by thread, but it won't reflect to other threads
        formatter.set(new SimpleDateFormat());

        System.out.println("Thread Name= "+Thread.currentThread().getName()+" formatter = "+formatter.get().toPattern());
    }

}
```



### [ThreadLocal 原理了解吗？](#threadlocal-原理了解吗)

从 `Thread`类源代码入手。

```java
public class Thread implements Runnable {
    //......
    //与此线程有关的ThreadLocal值。由ThreadLocal类维护
    ThreadLocal.ThreadLocalMap threadLocals = null;

    //与此线程有关的InheritableThreadLocal值。由InheritableThreadLocal类维护
    ThreadLocal.ThreadLocalMap inheritableThreadLocals = null;
    //......
}
```

从上面`Thread`类 源代码可以看出`Thread` 类中有一个 `threadLocals` 和 一个 `inheritableThreadLocals` 变量，它们都是 `ThreadLocalMap` 类型的变量,我们可以把 `ThreadLocalMap` 理解为`ThreadLocal` 类实现的定制化的 `HashMap`。默认情况下这两个变量都是 null，只有当前线程调用 `ThreadLocal` 类的 `set`或`get`方法时才创建它们，实际上调用这两个方法的时候，我们调用的是`ThreadLocalMap`类对应的 `get()`、`set()`方法。

`ThreadLocal`类的`set()`方法

```java
public void set(T value) {
    //获取当前请求的线程
    Thread t = Thread.currentThread();
    //取出 Thread 类内部的 threadLocals 变量(哈希表结构)
    ThreadLocalMap map = getMap(t);
    if (map != null)
        // 将需要存储的值放入到这个哈希表中
        map.set(this, value);
    else
        createMap(t, value);
}
ThreadLocalMap getMap(Thread t) {
    return t.threadLocals;
}
```

通过上面这些内容，我们足以通过猜测得出结论：**最终的变量是放在了当前线程的 `ThreadLocalMap` 中，并不是存在 `ThreadLocal` 上，`ThreadLocal` 可以理解为只是`ThreadLocalMap`的封装，传递了变量值。** `ThrealLocal` 类中可以通过`Thread.currentThread()`获取到当前线程对象后，直接通过`getMap(Thread t)`可以访问到该线程的`ThreadLocalMap`对象。

**每个`Thread`中都具备一个`ThreadLocalMap`，而`ThreadLocalMap`可以存储以`ThreadLocal`为 key ，Object 对象为 value 的键值对。**

```java
ThreadLocalMap(ThreadLocal<?> firstKey, Object firstValue) {
    //......
}
```

比如我们在同一个线程中声明了两个 `ThreadLocal` 对象的话， `Thread`内部都是使用仅有的那个`ThreadLocalMap` 存放数据的，`ThreadLocalMap`的 key 就是 `ThreadLocal`对象，value 就是 `ThreadLocal` 对象调用`set`方法设置的值。

`ThreadLocal` 数据结构如下图所示：

![ThreadLocal 数据结构](https://oss.javaguide.cn/github/javaguide/java/concurrent/threadlocal-data-structure.png)



### [ThreadLocal 内存泄露问题是怎么导致的？](#threadlocal-内存泄露问题是怎么导致的)

`ThreadLocalMap` 中使用的 key 为 `ThreadLocal` 的弱引用，而 value 是强引用。所以，如果 `ThreadLocal` 没有被外部强引用的情况下，在垃圾回收的时候，key 会被清理掉，而 value 不会被清理掉。

这样一来，`ThreadLocalMap` 中就会出现 key 为 null 的 Entry。假如我们不做任何措施的话，value 永远无法被 GC 回收，这个时候就可能会产生内存泄露。`ThreadLocalMap` 实现中已经考虑了这种情况，在调用 `set()`、`get()`、`remove()` 方法的时候，会清理掉 key 为 null 的记录。使用完 `ThreadLocal`方法后最好手动调用`remove()`方法



### ThreadLocal 解释文章（推荐）

[ThreadLocal 内存泄露原理及分析](https://mp.weixin.qq.com/s/IJsIQgRUO4HxZQk0w1lrfg)

[ThreadLocal最佳实践（包括Web User Token存储）](https://mp.weixin.qq.com/s/ErOkac1zN95bZA8kXPk74w)



## [线程池](https://javaguide.cn/java/concurrent/java-concurrent-questions-03.html#线程池)

### [什么是线程池?](#什么是线程池)

顾名思义，线程池就是管理一系列线程的资源池。当有任务要处理时，直接从线程池中获取线程来处理，处理完之后线程并不会立即被销毁，而是等待下一个任务。

### [为什么要用线程池？](#为什么要用线程池)

池化技术想必大家已经屡见不鲜了，线程池、数据库连接池、HTTP 连接池等等都是对这个思想的应用。池化技术的思想主要是为了减少每次获取资源的消耗，提高对资源的利用率。

**线程池**提供了一种限制和管理资源（包括执行一个任务）的方式。 每个**线程池**还维护一些基本统计信息，例如已完成任务的数量。

这里借用《Java 并发编程的艺术》提到的来说一下**使用线程池的好处**：

- **降低资源消耗**。通过重复利用已创建的线程降低线程创建和销毁造成的消耗。
- **提高响应速度**。当任务到达时，任务可以不需要等到线程创建就能立即执行。
- **提高线程的可管理性**。线程是稀缺资源，如果无限制的创建，不仅会消耗系统资源，还会降低系统的稳定性，使用线程池可以进行统一的分配，调优和监控。

### [如何创建线程池？](#如何创建线程池)

**方式一：通过`ThreadPoolExecutor`构造函数来创建（推荐）。**

![通过构造方法实现](/assets/threadpoolexecutor构造函数-UftlG_nP.png)通过构造方法实现

**方式二：通过 `Executor` 框架的工具类 `Executors` 来创建。**

我们可以创建多种类型的 `ThreadPoolExecutor`：

- **`FixedThreadPool`**：该方法返回一个固定线程数量的线程池。该线程池中的线程数量始终不变。当有一个新的任务提交时，线程池中若有空闲线程，则立即执行。若没有，则新的任务会被暂存在一个任务队列中，待有线程空闲时，便处理在任务队列中的任务。
- **`SingleThreadExecutor`：** 该方法返回一个只有一个线程的线程池。若多余一个任务被提交到该线程池，任务会被保存在一个任务队列中，待线程空闲，按先入先出的顺序执行队列中的任务。
- **`CachedThreadPool`：** 该方法返回一个可根据实际情况调整线程数量的线程池。初始大小为 0。当有新任务提交时，如果当前线程池中没有线程可用，它会创建一个新的线程来处理该任务。如果在一段时间内（默认为 60 秒）没有新任务提交，核心线程会超时并被销毁，从而缩小线程池的大小。
- **`ScheduledThreadPool`**：该方法返回一个用来在给定的延迟后运行任务或者定期执行任务的线程池。



### [线程池常见参数有哪些？如何解释？](https://javaguide.cn/java/concurrent/java-concurrent-questions-03.html#线程池常见参数有哪些-如何解释)

> ```java
> /**
>  * 用给定的初始参数创建一个新的ThreadPoolExecutor。
>  */
> public ThreadPoolExecutor(int corePoolSize,//线程池的核心线程数量
>                           int maximumPoolSize,//线程池的最大线程数
>                           long keepAliveTime,//当线程数大于核心线程数时，多余的空闲线程存活的最长时间
>                           TimeUnit unit,//时间单位
>                           BlockingQueue<Runnable> workQueue,//任务队列，用来储存等待执行任务的队列
>                           ThreadFactory threadFactory,//线程工厂，用来创建线程，一般默认即可
>                           RejectedExecutionHandler handler//拒绝策略，当提交的任务过多而不能及时处理时，我们可以定制策略来处理任务
>                            ) {
>     if (corePoolSize < 0 ||
>         maximumPoolSize <= 0 ||
>         maximumPoolSize < corePoolSize ||
>         keepAliveTime < 0)
>         throw new IllegalArgumentException();
>     if (workQueue == null || threadFactory == null || handler == null)
>         throw new NullPointerException();
>     this.corePoolSize = corePoolSize;
>     this.maximumPoolSize = maximumPoolSize;
>     this.workQueue = workQueue;
>     this.keepAliveTime = unit.toNanos(keepAliveTime);
>     this.threadFactory = threadFactory;
>     this.handler = handler;
> }
> ```

**`ThreadPoolExecutor` 3 个最重要的参数：**

- **`corePoolSize` :** 任务队列未达到队列容量时，最大可以同时运行的线程数量。
- **`maximumPoolSize` :** 任务队列中存放的任务达到队列容量的时候，当前可以同时运行的线程数量变为最大线程数。
- **`workQueue`:** 新任务来的时候会先判断当前运行的线程数量是否达到核心线程数，如果达到的话，新任务就会被存放在队列中。

`ThreadPoolExecutor`其他常见参数 :

- **`keepAliveTime`**:线程池中的线程数量大于 `corePoolSize` 的时候，如果这时没有新的任务提交，多余的空闲线程不会立即销毁，而是会等待，直到等待的时间超过了 `keepAliveTime`才会被回收销毁，线程池回收线程时，会对核心线程和非核心线程一视同仁，直到线程池中线程的数量等于 `corePoolSize` ，回收过程才会停止。
- **`unit`** : `keepAliveTime` 参数的时间单位。
- **`threadFactory`** :executor 创建新线程的时候会用到。
- **`handler`** :饱和策略。关于饱和策略下面单独介绍一下。

下面这张图可以加深你对线程池中各个参数的相互关系的理解（图片来源：《Java 性能调优实战》）：

![线程池各个参数的关系](https://javaguide.cn/assets/%E7%BA%BF%E7%A8%8B%E6%B1%A0%E5%90%84%E4%B8%AA%E5%8F%82%E6%95%B0%E4%B9%8B%E9%97%B4%E7%9A%84%E5%85%B3%E7%B3%BB-JlZBQPFq.png)



### [线程池的饱和策略有哪些？](https://javaguide.cn/java/concurrent/java-concurrent-questions-03.html#线程池的饱和策略有哪些)

如果当前同时运行的线程数量达到最大线程数量并且队列也已经被放满了任务时，`ThreadPoolExecutor` 定义一些策略:

- **`ThreadPoolExecutor.AbortPolicy`：** 抛出 `RejectedExecutionException`来拒绝新任务的处理。
- **`ThreadPoolExecutor.CallerRunsPolicy`：** 调用执行自己的线程运行任务，也就是直接在调用`execute`方法的线程中运行(`run`)被拒绝的任务，如果执行程序已关闭，则会丢弃该任务。因此这种策略会降低对于新任务提交速度，影响程序的整体性能。如果您的应用程序可以承受此延迟并且你要求任何一个任务请求都要被执行的话，你可以选择这个策略。
- **`ThreadPoolExecutor.DiscardPolicy`：** 不处理新任务，直接丢弃掉。
- **`ThreadPoolExecutor.DiscardOldestPolicy`：** 此策略将丢弃最早的未处理的任务请求。



### [线程池常用的阻塞队列有哪些？](https://javaguide.cn/java/concurrent/java-concurrent-questions-03.html#线程池常用的阻塞队列有哪些)

新任务来的时候会先判断当前运行的线程数量是否达到核心线程数，如果达到的话，新任务就会被存放在队列中。

不同的线程池会选用不同的阻塞队列，我们可以结合内置线程池来分析。

- 容量为 `Integer.MAX_VALUE` 的 `LinkedBlockingQueue`（无界队列）：`FixedThreadPool` 和 `SingleThreadExector` 。`FixedThreadPool`最多只能创建核心线程数的线程（核心线程数和最大线程数相等），`SingleThreadExector`只能创建一个线程（核心线程数和最大线程数都是 1），二者的任务队列永远不会被放满。
- `SynchronousQueue`（同步队列）：`CachedThreadPool` 。`SynchronousQueue` 没有容量，不存储元素，目的是保证对于提交的任务，如果有空闲线程，则使用空闲线程来处理；否则新建一个线程来处理任务。也就是说，`CachedThreadPool` 的最大线程数是 `Integer.MAX_VALUE` ，可以理解为线程数是可以无限扩展的，可能会创建大量线程，从而导致 OOM。
- `DelayedWorkQueue`（延迟阻塞队列）：`ScheduledThreadPool` 和 `SingleThreadScheduledExecutor` 。`DelayedWorkQueue` 的内部元素并不是按照放入的时间排序，而是会按照延迟的时间长短对任务进行排序，内部采用的是“堆”的数据结构，可以保证每次出队的任务都是当前队列中执行时间最靠前的。`DelayedWorkQueue` 添加元素满了之后会自动扩容原来容量的 1/2，即永远不会阻塞，最大扩容可达 `Integer.MAX_VALUE`，所以最多只能创建核心线程数的线程。



### [线程池处理任务的流程了解吗？](https://javaguide.cn/java/concurrent/java-concurrent-questions-03.html#线程池处理任务的流程了解吗)

![图解线程池实现原理](https://oss.javaguide.cn/javaguide/%E5%9B%BE%E8%A7%A3%E7%BA%BF%E7%A8%8B%E6%B1%A0%E5%AE%9E%E7%8E%B0%E5%8E%9F%E7%90%86.png)

1. 如果当前运行的线程数小于核心线程数，那么就会新建一个线程来执行任务。
2. 如果当前运行的线程数等于或大于核心线程数，但是小于最大线程数，那么就把该任务放入到任务队列里等待执行。
3. 如果向任务队列投放任务失败（任务队列已经满了），但是当前运行的线程数是小于最大线程数的，就新建一个线程来执行任务。
4. 如果当前运行的线程数已经等同于最大线程数了，新建线程将会使当前运行的线程超出最大线程数，那么当前任务会被拒绝，饱和策略会调用`RejectedExecutionHandler.rejectedExecution()`方法。



### [如何设计一个能够根据任务的优先级来执行的线程池？](https://javaguide.cn/java/concurrent/java-concurrent-questions-03.html#如何设计一个能够根据任务的优先级来执行的线程池)

我们上面也提到了，不同的线程池会选用不同的阻塞队列作为任务队列，比如`FixedThreadPool` 使用的是`LinkedBlockingQueue`（无界队列），由于队列永远不会被放满，因此`FixedThreadPool`最多只能创建核心线程数的线程。

假如我们需要实现一个优先级任务线程池的话，那可以考虑使用 `PriorityBlockingQueue` （优先级阻塞队列）作为任务队列（`ThreadPoolExecutor` 的构造函数有一个 `workQueue` 参数可以传入任务队列）。

![ThreadPoolExecutor构造函数](https://oss.javaguide.cn/github/javaguide/java/concurrent/common-parameters-of-threadpool-workqueue.jpg)

`PriorityBlockingQueue` 是一个支持优先级的无界阻塞队列，可以看作是线程安全的 `PriorityQueue`，两者底层都是使用小顶堆形式的二叉堆，即值最小的元素优先出队。不过，`PriorityQueue` 不支持阻塞操作。

要想让 `PriorityBlockingQueue` 实现对任务的排序，传入其中的任务必须是具备排序能力的，方式有两种：

1. 提交到线程池的任务实现 `Comparable` 接口，并重写 `compareTo` 方法来指定任务之间的优先级比较规则。
2. 创建 `PriorityBlockingQueue` 时传入一个 `Comparator` 对象来指定任务之间的排序规则(推荐)。

不过，这存在一些风险和问题，比如：

- `PriorityBlockingQueue` 是无界的，可能堆积大量的请求，从而导致 OOM。
- 可能会导致饥饿问题，即低优先级的任务长时间得不到执行。
- 由于需要对队列中的元素进行排序操作以及保证线程安全（并发控制采用的是可重入锁 `ReentrantLock`），因此会降低性能。

对于 OOM 这个问题的解决比较简单粗暴，就是继承`PriorityBlockingQueue` 并重写一下 `offer` 方法(入队)的逻辑，当插入的元素数量超过指定值就返回 false 。

饥饿问题这个可以通过优化设计来解决（比较麻烦），比如等待时间过长的任务会被移除并重新添加到队列中，但是优先级会被提升。

对于性能方面的影响，是没办法避免的，毕竟需要对任务进行排序操作。并且，对于大部分业务场景来说，这点性能影响是可以接受的。



### [如何动态修改线程池的参数？](#如何动态修改线程池的参数)

美团技术团队在[《Java 线程池实现原理及其在美团业务中的实践》open in new window](https://tech.meituan.com/2020/04/02/java-pooling-pratice-in-meituan.html)这篇文章中介绍到对线程池参数实现可自定义配置的思路和方法。

美团技术团队的思路是主要对线程池的核心参数实现自定义可配置。这三个核心参数是：

- **`corePoolSize` :** 核心线程数线程数定义了最小可以同时运行的线程数量。
- **`maximumPoolSize` :** 当队列中存放的任务达到队列容量的时候，当前可以同时运行的线程数量变为最大线程数。
- **`workQueue`:** 当新任务来的时候会先判断当前运行的线程数量是否达到核心线程数，如果达到的话，新任务就会被存放在队列中。

**为什么是这三个参数？**

我在[Java 线程池详解open in new window](https://javaguide.cn/java/concurrent/java-thread-pool-summary.html) 这篇文章中就说过这三个参数是 `ThreadPoolExecutor` 最重要的参数，它们基本决定了线程池对于任务的处理策略。

**如何支持参数动态配置？** 且看 `ThreadPoolExecutor` 提供的下面这些方法。

![img](https://oss.javaguide.cn/github/javaguide/java/concurrent/threadpoolexecutor-methods.png)

格外需要注意的是`corePoolSize`， 程序运行期间的时候，我们调用 `setCorePoolSize()`这个方法的话，线程池会首先判断当前工作线程数是否大于`corePoolSize`，如果大于的话就会回收工作线程。

另外，你也看到了上面并没有动态指定队列长度的方法，美团的方式是自定义了一个叫做 `ResizableCapacityLinkedBlockIngQueue` 的队列（主要就是把`LinkedBlockingQueue`的 capacity 字段的 final 关键字修饰给去掉了，让它变为可变的）



### 如何动态监控线程池

[Spring Boot项目中线程池的动态监控 (qq.com)](https://mp.weixin.qq.com/s/mAw0dASvikfCfssT1j14cQ)

[Springboot中如何做线程池的动态监控？ (qq.com)](https://mp.weixin.qq.com/s/tQnOBssogRmE9hEUB2xB_A)



## [AQS](#aqs)

### [AQS 是什么？](#aqs-是什么)

AQS 的全称为 `AbstractQueuedSynchronizer` ，翻译过来的意思就是抽象队列同步器。这个类在 `java.util.concurrent.locks` 包下面。

![img](https://oss.javaguide.cn/github/javaguide/java/AQS.png)

AQS 就是一个抽象类，主要用来构建锁和同步器。

AQS 为构建锁和同步器提供了一些通用功能的实现，因此，使用 AQS 能简单且高效地构造出应用广泛的大量的同步器，比如我们提到的 `ReentrantLock`，`Semaphore`，其他的诸如 `ReentrantReadWriteLock`，`SynchronousQueue`等等皆是基于 AQS 的。



### [AQS 的原理是什么？](#aqs-的原理是什么)

AQS 核心思想是，如果被请求的共享资源空闲，则将当前请求资源的线程设置为有效的工作线程，并且将共享资源设置为锁定状态。如果被请求的共享资源被占用，那么就需要一套线程阻塞等待以及被唤醒时锁分配的机制，这个机制 AQS 是用 **CLH 队列锁** 实现的，即将暂时获取不到锁的线程加入到队列中。

CLH(Craig,Landin,and Hagersten) 队列是一个虚拟的双向队列（虚拟的双向队列即不存在队列实例，仅存在结点之间的关联关系）。AQS 是将每条请求共享资源的线程封装成一个 CLH 锁队列的一个结点（Node）来实现锁的分配。在 CLH 同步队列中，一个节点表示一个线程，它保存着线程的引用（thread）、 当前节点在队列中的状态（waitStatus）、前驱节点（prev）、后继节点（next）。

CLH 队列结构如下图所示：

![img](https://oss.javaguide.cn/p3-juejin/40cb932a64694262993907ebda6a0bfe~tplv-k3u1fbpfcp-zoom-1.png)

AQS(`AbstractQueuedSynchronizer`)的核心原理图（图源[Java 并发之 AQS 详解open in new window](https://www.cnblogs.com/waterystone/p/4920797.html)）如下：

![img](https://oss.javaguide.cn/github/javaguide/java/CLH.png)

另外，状态信息 `state` 可以通过 `protected` 类型的`getState()`、`setState()`和`compareAndSetState()` 进行操作。并且，这几个方法都是 `final` 修饰的，在子类中无法被重写。

```java
//返回同步状态的当前值
protected final int getState() {
     return state;
}
 // 设置同步状态的值
protected final void setState(int newState) {
     state = newState;
}
//原子地（CAS操作）将同步状态值设置为给定值update如果当前同步状态的值等于expect（期望值）
protected final boolean compareAndSetState(int expect, int update) {
      return unsafe.compareAndSwapInt(this, stateOffset, expect, update);
}
```

以 `ReentrantLock` 为例，`state` 初始值为 0，表示未锁定状态。A 线程 `lock()` 时，会调用 `tryAcquire()` 独占该锁并将 `state+1` 。此后，其他线程再 `tryAcquire()` 时就会失败，直到 A 线程 `unlock()` 到 `state=`0（即释放锁）为止，其它线程才有机会获取该锁。当然，释放锁之前，A 线程自己是可以重复获取此锁的（`state` 会累加），这就是可重入的概念。但要注意，获取多少次就要释放多少次，这样才能保证 state 是能回到零态的。



## 面经

### 多线程如何按照顺序执行？

1. join()
2. CountDownLatch
3. 单线程池
4. CompletableFuture
5. Semaphore
6. CyclicBarrier

**参考文章：**[保证多线程顺序执行，四种方案，你知道几种？](https://mp.weixin.qq.com/s/Vj8gemiG3HcXC7Nf6bRoaA)

[手撕面试题：多个线程顺序执行问题-阿里云开发者社区 (aliyun.com)](https://developer.aliyun.com/article/776793)

[让线程按顺序执行8种方法 （转） - myseries - 博客园 (cnblogs.com)](https://www.cnblogs.com/myseries/p/11575757.html)



### 如何让多线程同步执行

1. CountDownLatch
2. CyclicBarrier

**参考文章：**[Java并发编程（二）如何保证线程同时/交替执行 - 六层楼 - 博客园 (cnblogs.com)](https://www.cnblogs.com/rever/p/14777913.html)

[如何让多个（如：20个线程同时执行）（笔记）_g.怎么让多个线程同时运行,例如4个线程同时执行业务?-CSDN博客](https://blog.csdn.net/weixin_44377223/article/details/109351578)



### 牛客网有关Java并发的问题的一些总结

[java手写题汇总(建议收藏)_牛客网 (nowcoder.com)](https://www.nowcoder.com/discuss/650808006875127808?sourceSSR=users)



### 线程有哪些状态

<img src="assets\image-20241015174703930.png" alt="image-20241015174703930" style="zoom: 80%;" />

<img src="https://mmbiz.qpic.cn/mmbiz_png/z40lCFUAHpm9K41GFdu8ib6z2D61FgAtJTtRnRfUh4AoQB2diaYpicoyREVeJtYELJyCgu60xVXneRJqWb426ZRWw/640?wx_fmt=png&from=appmsg&tp=wxpic&wxfrom=5&wx_lazy=1&wx_co=1" alt="图片" style="zoom: 80%;" />
