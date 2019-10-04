## base

**重写与重载的区别**

* 重写
  * 重写方法，外壳不变，核心重写
* 重载
  * 方法名字相同，参数不同，返回类型可以相同也可不同

**反射**

JAVA反射机制是在运行状态中，对于任意一个类，都能够知道这个类的所有属性和方法；对于任意一个对象，都能够调用它的任意方法和属性；这种动态获取信息以及动态调用对象方法的功能称为java语言的反射机制。

* https://www.jianshu.com/p/9be58ee20dee
* https://www.cnblogs.com/chanshuyi/p/head_first_of_reflection.html

**泛型**

泛型，即“参数化类型”。一提到参数，最熟悉的就是定义方法时有形参，然后调用此方法时传递实参。那么参数化类型怎么理解呢？

顾名思义，就是将类型由原来的具体的类型参数化，类似于方法中的变量参数，此时类型也定义成参数形式（可以称之为类型形参），

然后在使用/调用时传入具体的类型（类型实参）。

泛型的本质是为了参数化类型（在不创建新的类型的情况下，通过泛型指定的不同类型来控制形参具体限制的类型）。也就是说在泛型使用过程中，

操作的数据类型被指定为一个参数，这种参数类型可以用在类、接口和方法中，分别被称为泛型类、泛型接口、泛型方法。

Java中的泛型，只在编译阶段

* https://www.cnblogs.com/coprince/p/8603492.html

**接口回调**

类之间，模块之间，都有一定的调用关系。

一般来说，可以把使用某一接口的类创建的对象的引用赋给该接口声明的接口变量，那么该接口变量就可以调用被类实现的接口方法。

实际上，当接口变量调用被类实现的接口中的方法时，就是通知相应的对象调用接口的方法，这个调用的过程称为接口的回调

**多态**

多态就是指程序中定义的引用变量所指向的具体类型和通过该引用变量发出的方法调用在编程时并不确定，而是在程序运行期间才确定，即一个引用变量倒底会指向哪个类的实例对象，该引用变量发出的方法调用到底是哪个类中实现的方法，必须在由程序运行期间才能决定

实现方式

* https://www.cnblogs.com/kaleidoscope/p/9790766.html

## JVM && GC

* https://www.jianshu.com/p/76959115d486


## Java Object

* equal
    * 比较两个对象是否相等。Object类的默认实现，即比较2个对象的内存地址是否相等
    * 如果重写了equals方法，通常有必要重写hashCode
    * intern() 方法返回字符串对象的规范化表示形式。
        * 对于任意两个字符串 s 和 t，当且仅当 `s.equals(t) 为 true` 时，`s.intern() == t.intern()` 才为 true。
* notify
    * 唤醒一个在此对象监视器上等待的线程(监视器相当于就是锁的概念)。如果所有的线程都在此对象上等待，那么只会选择一个线程。选择是任意性的，并在对实现做出决定时发生。一个线程在对象监视器上等待可以调用wait方法。
* wait
    * wait方法会让当前线程等待直到另外一个线程调用对象的notify或notifyAll方法，或者超过参数设置的timeout超时时间。
    * 跟notify和notifyAll方法一样，当前线程必须是此对象的监视器所有者，否则还是会发生IllegalMonitorStateException异常。
* finalize
    * 该方法的作用是实例被垃圾回收器回收的时候触发的操作
    * https://blog.csdn.net/mingc0758/article/details/81351009

## 类加载机制

虚拟机把描述类的数据从class文件加载到内存，并对数据进行校验、转换解析和初始化，最终形成可以被虚拟机直接使用的Java类型

类的生命周期：

* 加载
* 链接
  * 验证
  * 准备
  * 解析
* 初始化
* 使用
* 卸载


**参考资料**

* https://www.cnblogs.com/luohanguo/p/9469851.html
* https://blog.csdn.net/xu768840497/article/details/79175335

## java 并发编程

* 隐式锁
  * synchronized
* 显示锁
  * Lock
  * ReadWriteLock
  * 基于 AQS( Abstract Queued Synchronizer)

### 原子操作

CAS是Compare And Set的缩写，是以一种无锁的方式实现并发控制。在实际情况下，同时操作同一个对象的概率非常小，所以多数加锁操作做的是无用功，CAS以一种 **乐观锁** 的方式实现并发控制。

**参考资料**

* https://juejin.im/post/5a73cbbff265da4e807783f5
* https://www.cnblogs.com/Mainz/p/3546347.html

### synchronized

#### 原理

**实现**

**优化**

Synchronized是通过对象内部的一个叫做监视器锁（monitor）来实现的。

但是监视器锁本质又是依赖于底层的操作系统的 **Mutex Lock** 来实现的。

而操作系统实现线程之间的切换这就需要从用户态转换到核心态，这个成本非常高，状态之间的转换需要相对比较长的时间，这就是为什么Synchronized效率低的原因。

因此，这种依赖于操作系统Mutex Lock所实现的锁我们称之为“重量级锁”。JDK中对Synchronized做的种种优化，其核心都是为了减少这种重量级锁的使用。JDK1.6以后，为了减少获得锁和释放锁所带来的性能消耗，提高性能，引入了“轻量级锁”和“偏向锁”。

* 无锁
* 偏向锁
* 轻量锁
  * 当代码进入同步块时，如果同步对象为无锁状态时，当前线程会在栈帧中创建一个锁记录(Lock Record)区域，同时将锁对象的对象头中 Mark Word 拷贝到锁记录中，再尝试使用 **CAS** 将 Mark Word 更新为指向锁记录的指针。
  * 拷贝对象头中的Mark Word复制到锁记录中
  * https://www.cnblogs.com/paddix/p/5405678.html
* 重量锁
* 解锁

公平锁

非公平锁

悲观锁

乐观锁

**参考资料**

* https://www.cnblogs.com/paddix/p/5374810.html

#### synchronized 与 Lock的区别

### volatile

volatile不能保证原子性

**不要将volatile用在getAndOperate场合（这种场合不原子，需要再加锁），仅仅set或者get的场景是适合volatile的**

实现是依靠内存屏障的指令，如果字段是volatile，Java内存模型将在写操作后插入一个写屏障指令，在读操作前插入一个读屏障指令。

内存屏障是一个CPU指令


#### 参考资料

* https://www.cnblogs.com/Mainz/p/3556430.html#


## IO

NIO

## 集合类

### HashMap HashTable ConcurrentHashMap

HashTable 没有null，对整个表synchronize

HashMap - 》

ConcurrentHashMap
java1.7 
    分段锁
    可重入锁

公平锁
非公平锁

公平锁（Fair）：加锁前检查是否有排队等待的线程，优先排队等待的线程，先来先得
非公平锁（Nonfair）：加锁时不考虑排队等待问题，直接尝试获取锁，获取不到自动到队尾等待


#### Java Hashmap

传统的Java HashMap 的实现是 **数组+链表** ，即使哈希函数取得再好，也很难达到元素百分百均匀分布

譬如，当 HashMap 中有大量的元素都存放到同一个桶中时，这个桶下有一条长长的链表，这个时候 HashMap 就相当于一个单链表，假如单链表有 n 个元素，遍历的时间复杂度就是 O(n)，完全失去了它的优势。

因此，引入了 **红黑树** ，在链表长度大于8的时候就对应链表转换为红黑树

其实，目的就是为了减少查找的次数，当链表过长时，转换为树的结构能有效减少查询的次数
<!-- todo 增加跳转链接 -->
譬如，可以将链表转换为AVL数（自平衡二叉查找树），但是AVL树的查找是稳定的，但是插入和删除是比较耗时的，因此才引入了红黑树

至于为什么是8，红黑树的平均查找长度是 **log(n)** ，长度为8的时候，平均查找长度为3，如果继续使用链表，平均查找长度为$\frac{1+2+3+4+5+6+7+8}{8}=4.5$，这才有转换为树的必要。链表长度如果是小于等于6，平均查找长度是3.5，虽然速度也很快的，但是转化为树结构和生成树的时间并不会太短。

还有选择6和8，中间有个差值7可以有效防止链表和树频繁转换。假设一下，如果设计成链表个数超过8则链表转换成树结构，链表个数小于8则树结构转换成链表，如果一个HashMap不停的插入、删除元素，链表个数在8左右徘徊，就会频繁的发生树转链表、链表转树，效率会很低。

### 参考资料

* https://www.cnblogs.com/heyonggang/p/9112731.html
* https://yemengying.com/2016/05/07/threadsafe-hashmap/
* https://www.jianshu.com/p/40078ed436b4


## Java Remote Method Invocation


## 参考资料

* https://github.com/CyC2018/CS-Notes