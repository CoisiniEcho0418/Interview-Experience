# Java集合面经&八股

------

> Java 集合， 也叫作容器，主要是由两大接口派生而来：一个是 `Collection`接口，主要用于存放单一元素；另一个是 `Map` 接口，主要用于存放键值对。对于`Collection` 接口，下面又有三个主要的子接口：`List`、`Set` 和 `Queue`。

![Java 集合框架概览](https://oss.javaguide.cn/github/javaguide/java/collection/java-collection-hierarchy.png)

> **`HashSet`，`TreeSet`，`ArrayList`,`LinkedList`,`HashMap`,`TreeMap` 都是线程不安全的。**



### 如何选用集合

我们主要根据集合的特点来选择合适的集合。比如：

- 我们需要根据键值获取到元素值时就选用 `Map` 接口下的集合，需要排序时选择 `TreeMap`,不需要排序时就选择 `HashMap`,需要保证线程安全就选用 `ConcurrentHashMap`。
- 我们只需要存放元素值时，就选择实现`Collection` 接口的集合，需要保证元素唯一时选择实现 `Set` 接口的集合比如 `TreeSet` 或 `HashSet`，不需要就选择实现 `List` 接口的比如 `ArrayList` 或 `LinkedList`，然后再根据实现这些接口的集合的特点来选用。



### 聊聊HashMap的底层结构

在 **JDK 1.7** 版本之前， HashMap 数据结构是**数组和链表**，HashMap通过哈希算法将元素的键（Key）映射到数组中的槽位（Bucket）。如果多个键映射到同一个槽位，它们会以链表的形式存储在同一个槽位上，因为链表的查询时间是O(n)，所以冲突很严重，一个索引上的链表非常长，效率就很低了。

所以在 **JDK 1.8** 版本的时候做了优化，当一个链表的长度超过8的时候就转换数据结构，不再使用链表存储，而是使用**红黑树**，查找时使用红黑树，时间复杂度O（log n），可以提高查询性能，但是在数量较少时，即数量小于6时，会将红黑树转换回链表。

![图片](https://mmbiz.qpic.cn/sz_mmbiz_png/J0g14CUwaZfW5jtMU7dzOUhL6avJSwsuZDX6iah5PhSzAx8UlmJNCQVdpnNnRqiauOojpKxcv8DATdmWJAq1NAXw/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

### 为什么要引入红黑树，而不用其他树？

- 为什么不使用二叉排序树？问题主要出现在二叉排序树在添加元素的时候极端情况下会出现线性结构。比如由于二叉排序树左子树所有节点的值均小于根节点的特点，如果我们添加的元素都比根节点小，会导致左子树线性增长，这样就失去了用树型结构替换链表的初衷，导致查询时间增长。所以这是不用二叉查找树的原因。
- 为什么不使用平衡二叉树呢？红黑树不追求"完全平衡"，而而AVL是严格平衡树，因此在增加或者删除节点的时候，根据不同情况，旋转的次数比红黑树要多。红黑树读取略逊于AVL，维护强于AVL，空间开销与AVL类似，内容极多时略优于AVL，维护优于AVL。

基本上主要的几种平衡树看来，**红黑树有着良好的稳定性和完整的功能，性能表现也很不错，综合实力强**，在诸如STL的场景中需要稳定表现。



### HashMap会出现红黑树一直增高变成无限高的情况吗？

不能无限增长。当集合中的节点数超过了阈值，HashMap会进行扩容，这时原始的红黑树节点会被打散，可能会退化成链表结构。

### HashMap读和写的时间复杂度是多少？

HashMap的读取（查找）和写入（插入、更新、删除）操作的时间复杂度均为O(1)，即常数时间复杂度。

这是因为HashMap内部使用哈希表来存储键值对，通过计算键的哈希值可以直接定位到对应的存储位置，从而实现快速的读取和写入操作。

在理想情况下，HashMap可以在常数时间内完成查找和插入操作，具有高效的性能表现。

### HashMap是线程安全的吗？怎么解决？

不是线程安全的。

- JDK 1.7 HashMap 采用数组 + 链表的数据结构，多线程背景下，在数组扩容的时候，存在 Entry 链死循环和数据丢失问题。
- JDK 1.8 HashMap 采用数组 + 链表 + 红黑二叉树的数据结构，优化了 1.7 中数组扩容的方案，解决了 Entry 链死循环和数据丢失问题。但是多线程背景下，put 方法存在数据覆盖的问题。

解决的方式：

- **使用ConcurrentHashMap**：ConcurrentHashMap是Java提供的线程安全的哈希表实现，它通过分段锁（Segment）和CAS操作来保证线程安全性，适用于高并发环境。
- **使用Collections.synchronizedMap**：可以通过Collections工具类中的synchronizedMap方法来创建一个线程安全的HashMap，该方法会返回一个同步的Map对象，但性能可能不如ConcurrentHashMap。

### 解决线程安全问题还有哪些办法？

- **使用同步关键字synchronized**：可以通过在方法或代码块上使用synchronized关键字来实现线程安全，确保同一时刻只有一个线程可以访问共享资源。
- **使用volatile关键字**：可以使用volatile关键字修饰变量，保证变量的可见性，即一个线程修改了变量的值，其他线程能立即看到最新值，从而避免数据不一致的问题。
- **使用线程安全的工具类**：Java中提供了诸如AtomicInteger、AtomicLong、CountDownLatch等线程安全的工具类，可以帮助解决并发场景下的线程安全性问题。
- **使用并发容器**：Java中提供了多种线程安全的并发容器，如ConcurrentLinkedQueue、CopyOnWriteArrayList等，可以替代传统的非线程安全容器来解决多线程并发访问问题。



### **HashSet 和 ArrayList 的区别**

- ArrayList 是基于动态数组实现的，HashSet 是基于 HashMap 实现的。
- ArrayList 允许重复元素和 null 值，可以有多个相同的元素；HashSet 保证每个元素唯一，不允许重复元素，基于元素的 hashCode 和 equals 方法来确定元素的唯一性。
- ArrayList 保持元素的插入顺序，可以通过索引访问元素；HashSet 不保证元素的顺序，元素的存储顺序依赖于哈希算法，并且可能随着元素的添加或删除而改变。



### **ArrayList 和 Vector 的比较**

- 线程安全性：Vector是线程安全的，ArrayList不是线程安全的。
- 扩容策略：ArrayList在底层数组不够用时在原来的基础上扩展0.5倍，Vector是扩展1倍。



### [比较 HashSet、LinkedHashSet 和 TreeSet 三者的异同](https://javaguide.cn/java/collection/java-collection-questions-01.html#比较-hashset、linkedhashset-和-treeset-三者的异同)

> `HashSet`、`LinkedHashSet` 和 `TreeSet` 都是 `Set` 接口的实现类，都能保证元素唯一，并且都不是线程安全的。
>
> `HashSet`、`LinkedHashSet` 和 `TreeSet` 的主要区别在于底层数据结构不同。`HashSet` 的底层数据结构是哈希表（基于 `HashMap` 实现）。`LinkedHashSet` 的底层数据结构是链表和哈希表，元素的插入和取出顺序满足 FIFO。`TreeSet` 底层数据结构是红黑树，元素是有序的，排序的方式有自然排序和定制排序。
>
> 底层数据结构不同又导致这三者的应用场景不同。`HashSet` 用于不需要保证元素插入和取出顺序的场景，`LinkedHashSet` 用于保证元素的插入和取出顺序满足 FIFO 的场景，`TreeSet` 用于支持对元素自定义排序规则的场景。



### [Comparable 和 Comparator 的区别](https://javaguide.cn/java/collection/java-collection-questions-01.html#comparable-和-comparator-的区别)

> `Comparable` 接口和 `Comparator` 接口都是 Java 中用于排序的接口，它们在实现类对象之间比较大小、排序等方面发挥了重要作用：
>
> - `Comparable` 接口实际上是出自`java.lang`包 它有一个 `compareTo(Object obj)`方法用来排序
> - `Comparator`接口实际上是出自 `java.util` 包它有一个`compare(Object obj1, Object obj2)`方法用来排序



### [ArrayDeque 与 LinkedList 的区别](https://javaguide.cn/java/collection/java-collection-questions-01.html#arraydeque-与-linkedlist-的区别)

> `ArrayDeque` 和 `LinkedList` 都实现了 `Deque` 接口，两者都具有队列的功能，但两者有什么区别呢？
>
> - `ArrayDeque` 是基于可变长的数组和双指针来实现，而 `LinkedList` 则通过链表来实现。
> - `ArrayDeque` 不支持存储 `NULL` 数据，但 `LinkedList` 支持。
> - `ArrayDeque` 是在 JDK1.6 才被引入的，而`LinkedList` 早在 JDK1.2 时就已经存在。
> - `ArrayDeque` 插入时可能存在扩容过程, 不过均摊后的插入操作依然为 O(1)。虽然 `LinkedList` 不需要扩容，但是每次插入数据时均需要申请新的堆空间，均摊性能相比更慢。
>
> 从性能的角度上，选用 `ArrayDeque` 来实现队列要比 `LinkedList` 更好。此外，`ArrayDeque` 也可以用于实现栈。



### [说一说 PriorityQueue](https://javaguide.cn/java/collection/java-collection-questions-01.html#说一说-priorityqueue)

> `PriorityQueue` 是在 JDK1.5 中被引入的, 其与 `Queue` 的区别在于元素出队顺序是与优先级相关的，即总是优先级最高的元素先出队。
>
> 这里列举其相关的一些要点：
>
> - `PriorityQueue` 利用了二叉堆的数据结构来实现的，底层使用可变长的数组来存储数据
> - `PriorityQueue` 通过堆元素的上浮和下沉，实现了在 O(logn) 的时间复杂度内插入元素和删除堆顶元素。
> - `PriorityQueue` 是非线程安全的，且不支持存储 `NULL` 和 `non-comparable` 的对象。
> - `PriorityQueue` 默认是小顶堆，但可以接收一个 `Comparator` 作为构造参数，从而来自定义元素优先级的先后。
>
> `PriorityQueue` 在面试中可能更多的会出现在手撕算法的时候，典型例题包括堆排序、求第 K 大的数、带权图的遍历等，所以需要会熟练使用才行。



### HashMap是线程安全的吗？怎么解决？

不是线程安全的。

- JDK 1.7 HashMap 采用数组 + 链表的数据结构，多线程背景下，在数组扩容的时候，存在 Entry 链死循环和数据丢失问题。
- JDK 1.8 HashMap 采用数组 + 链表 + 红黑二叉树的数据结构，优化了 1.7 中数组扩容的方案，解决了 Entry 链死循环和数据丢失问题。但是多线程背景下，put 方法存在数据覆盖的问题。

解决的方式：

- **使用ConcurrentHashMap**：ConcurrentHashMap是Java提供的线程安全的哈希表实现，它通过分段锁（Segment）和CAS操作来保证线程安全性，适用于高并发环境。
- **使用Collections.synchronizedMap**：可以通过Collections工具类中的synchronizedMap方法来创建一个线程安全的HashMap，该方法会返回一个同步的Map对象，但性能可能不如ConcurrentHashMap。

### 解决线程安全问题还有哪些办法？

- **使用同步关键字synchronized**：可以通过在方法或代码块上使用synchronized关键字来实现线程安全，确保同一时刻只有一个线程可以访问共享资源。
- **使用volatile关键字**：可以使用volatile关键字修饰变量，保证变量的可见性，即一个线程修改了变量的值，其他线程能立即看到最新值，从而避免数据不一致的问题。
- **使用线程安全的工具类**：Java中提供了诸如AtomicInteger、AtomicLong、CountDownLatch等线程安全的工具类，可以帮助解决并发场景下的线程安全性问题。
- **使用并发容器**：Java中提供了多种线程安全的并发容器，如ConcurrentLinkedQueue、CopyOnWriteArrayList等，可以替代传统的非线程安全容器来解决多线程并发访问问题。





### Map

#### [HashMap 和 Hashtable 的区别](https://javaguide.cn/java/collection/java-collection-questions-02.html#hashmap-和-hashtable-的区别)

#### [HashMap 和 HashSet 区别](https://javaguide.cn/java/collection/java-collection-questions-02.html#hashmap-和-hashset-区别)

#### [HashMap 和 TreeMap 区别](https://javaguide.cn/java/collection/java-collection-questions-02.html#hashmap-和-treemap-区别)

#### [HashSet 如何检查重复?](https://javaguide.cn/java/collection/java-collection-questions-02.html#hashset-如何检查重复)

> 当你把对象加入`HashSet`时，`HashSet` 会先计算对象的`hashcode`值来判断对象加入的位置，同时也会与其他加入的对象的 `hashcode` 值作比较，如果没有相符的 `hashcode`，`HashSet` 会假设对象没有重复出现。但是如果发现有相同 `hashcode` 值的对象，这时会调用`equals()`方法来检查 `hashcode` 相等的对象是否真的相同。如果两者相同，`HashSet` 就不会让加入操作成功。

在 JDK1.8 中，`HashSet`的`add()`方法只是简单的调用了`HashMap`的`put()`方法，并且判断了一下返回值以确保是否有重复元素。直接看一下`HashSet`中的源码：

```java
// Returns: true if this set did not already contain the specified element
// 返回值：当 set 中没有包含 add 的元素时返回真
public boolean add(E e) {
        return map.put(e, PRESENT)==null;
}
```

而在`HashMap`的`putVal()`方法中也能看到如下说明：

```java
// Returns : previous value, or null if none
// 返回值：如果插入位置没有元素返回null，否则返回上一个元素
final V putVal(int hash, K key, V value, boolean onlyIfAbsent,
                   boolean evict) {
...
}
```

也就是说，在 JDK1.8 中，实际上无论`HashSet`中是否已经存在了某元素，`HashSet`都会直接插入，只是会在`add()`方法的返回值处告诉我们插入前是否存在相同元素。



#### [HashMap 的底层实现](https://javaguide.cn/java/collection/java-collection-questions-02.html#hashmap-的底层实现)

在 **JDK 1.7** 版本之前， HashMap 数据结构是**数组和链表**，HashMap通过哈希算法将元素的键（Key）映射到数组中的槽位（Bucket）。如果多个键映射到同一个槽位，它们会以链表的形式存储在同一个槽位上，因为链表的查询时间是O(n)，所以冲突很严重，一个索引上的链表非常长，效率就很低了。

所以在 **JDK 1.8** 版本的时候做了优化，当一个链表的长度超过8的时候就转换数据结构，不再使用链表存储，而是使用**红黑树**，查找时使用红黑树，时间复杂度O（log n），可以提高查询性能，但是在数量较少时，即数量小于6时，会将红黑树转换回链表。

![图片](https://mmbiz.qpic.cn/sz_mmbiz_png/J0g14CUwaZfW5jtMU7dzOUhL6avJSwsuZDX6iah5PhSzAx8UlmJNCQVdpnNnRqiauOojpKxcv8DATdmWJAq1NAXw/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

#### 为什么要引入红黑树，而不用其他树？

- 为什么不使用二叉排序树？问题主要出现在二叉排序树在添加元素的时候极端情况下会出现线性结构。比如由于二叉排序树左子树所有节点的值均小于根节点的特点，如果我们添加的元素都比根节点小，会导致左子树线性增长，这样就失去了用树型结构替换链表的初衷，导致查询时间增长。所以这是不用二叉查找树的原因。
- 为什么不使用平衡二叉树呢？红黑树不追求"完全平衡"，而而AVL是严格平衡树，因此在增加或者删除节点的时候，根据不同情况，旋转的次数比红黑树要多。红黑树读取略逊于AVL，维护强于AVL，空间开销与AVL类似，内容极多时略优于AVL，维护优于AVL。

基本上主要的几种平衡树看来，**红黑树有着良好的稳定性和完整的功能，性能表现也很不错，综合实力强**，在诸如STL的场景中需要稳定表现。





#### [HashMap 的长度为什么是 2 的幂次方](https://javaguide.cn/java/collection/java-collection-questions-02.html#hashmap-的长度为什么是-2-的幂次方)

为了能让 HashMap 存取高效，尽量较少碰撞，也就是要尽量把数据分配均匀。我们上面也讲到了过了，Hash 值的范围值-2147483648 到 2147483647，前后加起来大概 40 亿的映射空间，只要哈希函数映射得比较均匀松散，一般应用是很难出现碰撞的。但问题是一个 40 亿长度的数组，内存是放不下的。所以这个散列值是不能直接拿来用的。用之前还要先做对数组的长度取模运算，得到的余数才能用来要存放的位置也就是对应的数组下标。这个数组下标的计算方法是“ `(n - 1) & hash`”。（n 代表数组长度）。这也就解释了 HashMap 的长度为什么是 2 的幂次方。

**这个算法应该如何设计呢？**

我们首先可能会想到采用%取余的操作来实现。但是，重点来了：**“取余(%)操作中如果除数是 2 的幂次则等价于与其除数减一的与(&)操作（也就是说 hash%length==hash&(length-1)的前提是 length 是 2 的 n 次方；）。”** 并且 **采用二进制位操作 &，相对于%能够提高运算效率，这就解释了 HashMap 的长度为什么是 2 的幂次方。



#### [HashMap 多线程操作导致死循环问题](https://javaguide.cn/java/collection/java-collection-questions-02.html#hashmap-多线程操作导致死循环问题)

JDK1.7 及之前版本的 `HashMap` 在多线程环境下扩容操作可能存在死循环问题，这是由于当一个桶位中有多个元素需要进行扩容时，多个线程同时对链表进行操作，头插法可能会导致链表中的节点指向错误的位置，从而形成一个环形链表，进而使得查询元素的操作陷入死循环无法结束。

为了解决这个问题，JDK1.8 版本的 HashMap 采用了尾插法而不是头插法来避免链表倒置，使得插入的节点永远都是放在链表的末尾，避免了链表中的环形结构。但是还是不建议在多线程下使用 `HashMap`，因为多线程下使用 `HashMap` 还是会存在数据覆盖的问题。并发环境下，推荐使用 `ConcurrentHashMap` 。



#### [HashMap 为什么线程不安全？](https://javaguide.cn/java/collection/java-collection-questions-02.html#hashmap-为什么线程不安全)



#### [ConcurrentHashMap 和 Hashtable 的区别](https://javaguide.cn/java/collection/java-collection-questions-02.html#concurrenthashmap-和-hashtable-的区别)



#### [ConcurrentHashMap 线程安全的具体实现方式/底层具体实现](https://javaguide.cn/java/collection/java-collection-questions-02.html#concurrenthashmap-线程安全的具体实现方式-底层具体实现)



#### [JDK 1.7 和 JDK 1.8 的 ConcurrentHashMap 实现有什么不同？](https://javaguide.cn/java/collection/java-collection-questions-02.html#jdk-1-7-和-jdk-1-8-的-concurrenthashmap-实现有什么不同)



#### [ConcurrentHashMap 为什么 key 和 value 不能为 null？](https://javaguide.cn/java/collection/java-collection-questions-02.html#concurrenthashmap-为什么-key-和-value-不能为-null)



#### [ConcurrentHashMap 能保证复合操作的原子性吗？](https://javaguide.cn/java/collection/java-collection-questions-02.html#concurrenthashmap-能保证复合操作的原子性吗)



#### Hashmap的put流程？

![图片](https://mmbiz.qpic.cn/sz_mmbiz_png/J0g14CUwaZfoVlP0ftECq7GBqNkxA95E2VCrQ8icrYk5oHDksiaiaHbTlqticw6n0vnkbwUH8nhQpgXqxSMGew5H9A/640?wx_fmt=png&from=appmsg&wxfrom=5&wx_lazy=1&wx_co=1)HashMap

HashMap的`put()`方法用于向HashMap中添加键值对。当调用HashMap的`put()`方法时，会按照以下详细流程执行：

> 第一步：根据要添加的键的哈希码计算在数组中的位置（索引）。

> 第二步：检查该位置是否为空（即没有键值对存在）

- 如果为空，则直接在该位置创建一个新的Entry对象来存储键值对。将要添加的键值对作为该Entry的键和值，并保存在数组的对应位置。将HashMap的修改次数（modCount）加1，以便在进行迭代时发现并发修改。

> 第三步：如果该位置已经存在其他键值对，检查该位置的第一个键值对的哈希码和键是否与要添加的键值对相同？

- 如果相同，则表示找到了相同的键，直接将新的值替换旧的值，完成更新操作。

> 第四步：如果第一个键值对的哈希码和键不相同，则需要遍历链表或红黑树来查找是否有相同的键：

如果键值对集合是链表结构：

- 从链表的头部开始逐个比较键的哈希码和equals()方法，直到找到相同的键或达到链表末尾。
- 如果找到了相同的键，则使用新的值取代旧的值，即更新键对应的值。
- 如果没有找到相同的键，则将新的键值对添加到链表的头部。

如果键值对集合是红黑树结构：

- 在红黑树中使用哈希码和equals()方法进行查找。根据键的哈希码，定位到红黑树中的某个节点，然后逐个比较键，直到找到相同的键或达到红黑树末尾。
- 如果找到了相同的键，则使用新的值取代旧的值，即更新键对应的值。
- 如果没有找到相同的键，则将新的键值对添加到红黑树中。

> 第五步：检查链表长度是否达到阈值（默认为8）：

- 如果链表长度超过阈值，且HashMap的数组长度大于等于64，则会将链表转换为红黑树，以提高查询效率。

> 第六步：检查负载因子是否超过阈值（默认为0.75）：

- 如果键值对的数量（size）与数组的长度的比值大于阈值，则需要进行扩容操作。

> 第七步：扩容操作：

- 创建一个新的两倍大小的数组。
- 将旧数组中的键值对重新计算哈希码并分配到新数组中的位置。
- 更新HashMap的数组引用和阈值参数。

> 第八步：完成添加操作。

需要注意的是，**HashMap中的键和值都可以为null**。

此外，HashMap是非线程安全的，如果在多线程环境下使用，需要采取额外的同步措施或使用线程安全的ConcurrentHashMap。



#### **hashcode 和 equals 方法只重写一个行不行，只重写 equals 没重写 hashcode，map put 的时候会发生什么**

##### 为什么重写 quals 时必须重写 hashCode ⽅法？

因为两个相等的对象的 `hashCode` 值必须是相等。也就是说如果 `equals` 方法判断两个对象是相等的，那这两个对象的 `hashCode` 值也要相等。

如果重写 `equals()` 时没有重写 `hashCode()` 方法的话就可能会导致 `equals` 方法判断是相等的两个对象，`hashCode` 值却不相等。

**总结**：

- `equals` 方法判断两个对象是相等的，那这两个对象的 `hashCode` 值也要相等。
- 两个对象有相同的 `hashCode` 值，他们也不一定是相等的（哈希碰撞）。

##### 只重写 equals 没重写 hashcode，map put 的时候会发生什么?

如果只重写 equals 方法，没有重写 hashcode 方法，那么会导致 equals 相等的两个对象，hashcode 不相等，这样的话，这两个对象会被放到不同的桶中，这样就会导致 get 的时候，找不到对应的值。



### Java集合使用注意事项

#### [集合去重](https://javaguide.cn/java/collection/java-collection-precautions-for-use.html#集合去重)



#### [集合转数组](https://javaguide.cn/java/collection/java-collection-precautions-for-use.html#集合转数组)

《阿里巴巴 Java 开发手册》的描述如下：

> **使用集合转数组的方法，必须使用集合的 `toArray(T[] array)`，传入的是类型完全一致、长度为 0 的空数组。**

`toArray(T[] array)` 方法的参数是一个泛型数组，如果 `toArray` 方法中没有传递任何参数的话返回的是 `Object`类 型数组。

```java
String [] s= new String[]{
    "dog", "lazy", "a", "over", "jumps", "fox", "brown", "quick", "A"
};
List<String> list = Arrays.asList(s);
Collections.reverse(list);
//没有指定类型的话会报错
s=list.toArray(new String[0]);
```

由于 JVM 优化，`new String[0]`作为`Collection.toArray()`方法的参数现在使用更好，`new String[0]`就是起一个模板的作用，指定了返回数组的类型，0 是为了节省空间，因为它只是为了说明返回的类型。详见：https://shipilev.net/blog/2016/arrays-wisdom-ancients/



#### [集合转数组](https://javaguide.cn/java/collection/java-collection-precautions-for-use.html#集合转数组)（看看就好，应该不是特别重要）

下面我们来总结一下使用注意事项。

**1、`Arrays.asList()`是泛型方法，传递的数组必须是对象数组，而不是基本类型。**

```java
int[] myArray = {1, 2, 3};
List myList = Arrays.asList(myArray);
System.out.println(myList.size());//1
System.out.println(myList.get(0));//数组地址值
System.out.println(myList.get(1));//报错：ArrayIndexOutOfBoundsException
int[] array = (int[]) myList.get(0);
System.out.println(array[0]);//1
```

当传入一个原生数据类型数组时，`Arrays.asList()` 的真正得到的参数就不是数组中的元素，而是数组对象本身！此时 `List` 的唯一元素就是这个数组，这也就解释了上面的代码。

**2、使用集合的修改方法: `add()`、`remove()`、`clear()`会抛出异常。**

```java
List myList = Arrays.asList(1, 2, 3);
myList.add(4);//运行时报错：UnsupportedOperationException
myList.remove(1);//运行时报错：UnsupportedOperationException
myList.clear();//运行时报错：UnsupportedOperationException
```

`Arrays.asList()` 方法返回的并不是 `java.util.ArrayList` ，而是 `java.util.Arrays` 的一个内部类,这个内部类并没有实现集合的修改方法或者说并没有重写这些方法。

```java
List myList = Arrays.asList(1, 2, 3);
System.out.println(myList.getClass());//class java.util.Arrays$ArrayList
```

**那我们如何正确的将数组转换为 `ArrayList` ?**

1、手动实现工具类

```java
//JDK1.5+
static <T> List<T> arrayToList(final T[] array) {
  final List<T> l = new ArrayList<T>(array.length);

  for (final T s : array) {
    l.add(s);
  }
  return l;
}


Integer [] myArray = { 1, 2, 3 };
System.out.println(arrayToList(myArray).getClass());//class java.util.ArrayList
```

2、最简便的方法

```java
List list = new ArrayList<>(Arrays.asList("a", "b", "c"))
```

3、使用 Java8 的 `Stream`(推荐)

```java
Integer [] myArray = { 1, 2, 3 };
List myList = Arrays.stream(myArray).collect(Collectors.toList());
//基本类型也可以实现转换（依赖boxed的装箱操作）
int [] myArray2 = { 1, 2, 3 };
List myList = Arrays.stream(myArray2).boxed().collect(Collectors.toList());
```

4、使用 Guava

对于不可变集合，你可以使用[`ImmutableList`open in new window](https://github.com/google/guava/blob/master/guava/src/com/google/common/collect/ImmutableList.java)类及其[`of()`open in new window](https://github.com/google/guava/blob/master/guava/src/com/google/common/collect/ImmutableList.java#L101)与[`copyOf()`open in new window](https://github.com/google/guava/blob/master/guava/src/com/google/common/collect/ImmutableList.java#L225)工厂方法：（参数不能为空）

```java
List<String> il = ImmutableList.of("string", "elements");  // from varargs
List<String> il = ImmutableList.copyOf(aStringArray);      // from array
```

对于可变集合，你可以使用[`Lists`open in new window](https://github.com/google/guava/blob/master/guava/src/com/google/common/collect/Lists.java)类及其[`newArrayList()`open in new window](https://github.com/google/guava/blob/master/guava/src/com/google/common/collect/Lists.java#L87)工厂方法：

```java
List<String> l1 = Lists.newArrayList(anotherListOrCollection);    // from collection
List<String> l2 = Lists.newArrayList(aStringArray);               // from array
List<String> l3 = Lists.newArrayList("or", "string", "elements"); // from varargs
```

5、使用 Apache Commons Collections

```java
List<String> list = new ArrayList<String>();
CollectionUtils.addAll(list, str);
```

6、 使用 Java9 的 `List.of()`方法

```java
Integer[] array = {1, 2, 3};
List<Integer> list = List.of(array);
```



