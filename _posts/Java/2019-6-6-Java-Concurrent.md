---
layout: post
title: 并发&线程池
categories: Java
description: 并发
keywords: 并发 线程池
---

# 同步

## synchronized 关键字
- 作用
    - synchronized 锁什么？锁对象。 
    - 可能锁对象包括： this， 临界资源对象，Class 类对象。 
- 同步方法:同步方法锁定的是当前对象。当多线程通过同一个对象引用多次调用当前同步方法时，需同步执行。 
- 同步代码块
    - 同步代码块的同步粒度更加细致，是商业开发中推荐的编程方式。可以定位到具体的同步位置，而不是简单的将方法整体实现同步逻辑。在效率上，相对更高。 
    - 锁定临界对象：同步代码块在执行时，是锁定object对象。当多个线程调用同一个方法时，锁定对象不变的情况下，需同步执行。 
    - 锁定当前对象：当锁定对象为 this 时，同步方法。 

## 锁的底层实现 
- Java 虚拟机中的同步(Synchronization)基于进入和退出管程(Monitor)对象实现。同步方法并不是由monitor enter和monitor exit指令来实现同步的，而是由方法调用指令读取运行时常量池中方法的 ACC_SYNCHRONIZED 标志来隐式实现的。 

    - 对象内存简图 
![object-monitor](/images/java/object-monitor.png)

>   - 对象头：存储对象的hashCode、锁信息或分代年龄或GC标志，类型指针指向对象的类元数据，JVM通过这个指针确定该对象是哪个类的实例等信息。 
>   - 实例变量：存放类的属性数据信息，包括父类的属性信息 
>   - 填充数据：由于虚拟机要求对象起始地址必须是8字节的整数倍。填充数据不是必须存在的，仅仅是为了字节对齐
>   - 当在对象上加锁时，数据是记录在对象头中。当执行synchronized同步方法或同步代码块时，会在对象头中记录锁标记，锁标记指向的是monitor对象（也称为管程或监视器锁）的起始地址。每个对象都存在着一个monitor与之关联，对象与其monitor之间的关系有 存在多种实现方式，如monitor可以与对象一起创建销毁或当线程试图获取对象锁时自动生成，但当一个monitor被某个线程持有后，它便处于锁定状态。 在 Java 虚拟机(HotSpot)中，monitor 是由 ObjectMonitor 实现的。ObjectMonitor中有两个队列，WaitSet和EntryList以及Owner标记。其中WaitSet是用于管理等待队列(wait)线程的，EntryList是用于管理锁池阻塞线程的，Owner标记用于记录当前执行线程
    - 线程状态图
![object-monitor](/images/java/thread-life.png)
>   - 当多线程并发访问同一个同步代码时，首先会进入EntryList，当线程获取锁标记后，monitor中的Owner记录此线程，并在monitor中的计数器执行递增计算（+1），代表锁定，其他线程在EntryList中继续阻塞。若执行线程调用 wait 方法，则 monitor 中的计数器执行 赋值为 0 计算，并将Owner 标记赋值为 null，代表放弃锁，执行线程进如WaitSet 中阻塞。 若执行线程调用notify/notifyAll方法，WaitSet中的线程被唤醒，进入EntryList中阻塞(先进入就绪状态再进入锁池队列),等待获取锁标记。若执行线程的同步代码执行结束，同样会释放锁标记，monitor中的Owner标记赋值为null，且计数器赋值为 0 计算

## 锁的种类
- Java 中锁的种类大致分为偏向锁，自旋锁，轻量级锁，重量级锁。 
- 锁的使用方式为：先提供偏向锁，如果不满足的时候，升级为轻量级锁，再不满足，升级为重量级锁。自旋锁是一个过渡的锁状态，不是一种实际的锁类型。 锁只能升级，不能降级。 
    - 重量级锁：EntryList解释的锁
    - 偏向锁：是一种编译解释锁。如果代码中不可能出现多线程并发争抢同一个锁的时候，JVM 编译代码，解释执行的时候，会自动的放弃同步信息。消除 synchronized 的同步代码结果。使用 锁标记的形式记录锁状态。在 Monitor 中有变量 ACC_SYNCHRONIZED。当变量值使用的时候， 代表偏向锁锁定。可以避免锁的争抢和锁池状态的维护。提高效率。 
    - 轻量级锁：过渡锁。当偏向锁不满足，也就是有多线程并发访问，锁定同一个对象的时候，先提升 为轻量级锁。也是使用标记 ACC_SYNCHRONIZED 标记记录的。ACC_UNSYNCHRONIZED标记记录未获取到锁信息的线程。就是只有两个线程争抢锁标记的时候，优先使用轻量级锁。 两个线程也可能出现重量级锁。 
    - 自旋锁：是一个过渡锁，是偏向锁和轻量级锁的过渡。当获取锁的过程中，未获取到。为了提高效率，JVM自动执行若干次空循环，再次申请锁，而不是进入阻塞状态的情况。称为自旋锁。自旋锁提高效率就是避免线程状态的变更。
 
## volatile 关键字 
- 变量的线程可见性。在CPU计算过程中，会将计算过程需要的数据加载到CPU计算缓存中，当CPU计算中断时，有可能刷新缓存，重新读取内存中的数据。在线程运行的过程中，如果某变量被其他线程修改，可能造成数据不一致的情况，从而导致结果错误。而 volatile 修饰的变量是线程可见的，当 JVM 解释 volatile 修饰的变量时，会通知 CPU，在计算过程中， 每次使用变量参与计算时，都会检查内存中的数据是否发生变化，而不是一直使用 CPU 缓 存中的数据，可以保证计算结果的正确。 
- volatile 只是通知底层计算时，CPU 检查内存数据，而不是让一个变量在多个线程中同步。 

## wait&notify 

## AtomicXxx 类型组 
- 原子类型。 在 concurrent.atomic 包中定义了若干原子类型，这些类型中的每个方法都是保证了原子操作的。多线程并发访问原子类型对象中的方法，不会出现数据错误。在多线程开发中，如果某数据需要多个线程同时操作，且要求计算原子性，可以考虑使用原子类型对象。 
- 注意：原子类型中的方法 是保证了原子操作，但多个方法之间是没有原子性的。如：AtomicInteger i = new AtomicInteger(0); if(i.get() != 5)  i.incrementAndGet(); 在上述代码中， get 方法和 incrementAndGet 方法都是原子操作，但复合使用时，无法保证原子性，仍旧可能出现数据错误。

## CountDownLatch 门闩 
- 门闩是 concurrent 包中定义的一个类型，是用于多线程通讯的一个辅助类型。 
- 门闩相当于在一个门上加多个锁，当线程调用 await 方法时，会检查门闩数量，如果门闩数量大于 0，线程会阻塞等待。当线程调用 countDown 时，会递减门闩的数量，当门闩数 量为 0 时，await 阻塞线程可执行。 

## 锁的重入 
- 在 Java 中，同步锁是可以重入的。只有同一线程调用同步方法或执行同步代码块，对同一个对象加锁时才可重入。
- 当线程持有锁时，会在monitor的计数器中执行递增计算，若当前线程调用其他同步代码，且同步代码的锁对象相同时，monitor 中的计数器继续递增。每个同步代码执行结束， monitor 中的计数器都会递减，直至所有同步代码执行结束，monitor 中的计数器为 0 时，释放锁标记，Owner 标记赋值为 null
 
## ReentrantLock 
- 重入锁，建议应用的同步方式。相对效率比 synchronized 高。量级较轻。 synchronized 在 JDK1.5 版本开始，尝试优化。到 JDK1.7 版本后，优化效率已经非常好了。 在绝对效率上，不比 reentrantLock 差多少。
- 使用重入锁， 必须 必须必须 手工释放锁标记。一般都是在 finally 代码块中定义释放锁标 记的 unlock 方法。 

## ThreadLocal 
- 在一个操作系统中，线程和进程是有数量上 限的。在操作系统中，确定线程和进程唯一性的 唯一条件就是线程或进程 ID。 操作系统在回收线程或进程的时候，不是一 定杀死线程或进程，在繁忙的时候，只会做情况 线程或进程栈数据的操作，重复使用线程或进程 

# 同步容器
- 解决并发情况下的容器线程安全问题的。给多线程环境准备一个线程安全的容器对象。线程安全的容器对象： Vector, Hashtable。线程安全容器对象，都是使用 synchronized 方法实现的。 concurrent 包中的同步容器，大多数是使用系统底层技术实现的线程安全。类似 native。 Java8 中使用 CAS

## Map/Set
- ConcurrentHashMap/ConcurrentHashSet 底层哈希实现的同步 Map(Set)。效率高，线程安全。使用系统底层技术实现线程安全。量级较 synchronized 低。key 和 value 不能为 null。 
- ConcurrentSkipListMap/ConcurrentSkipListSet底层跳表（SkipList）实现的同步 Map(Set)。有序，效率比 ConcurrentHashMap 稍低。 

## List 
- CopyOnWriteArrayList 写时复制集合。写入效率低，读取效率高。每次写入数据，都会创建一个新的底层数组。

## Queue 
- ConcurrentLinkedQueue 基础链表同步队列
- LinkedBlockingQueue 阻塞队列，队列容量不足自动阻塞，队列容量为 0 自动阻塞。 
- ArrayBlockingQueue 
    - 底层数组实现的有界队列。自动阻塞。根据调用 API（add/put/offer）不同，有不同特 性。 当容量不足的时候，有阻塞能力。 
        - add 方法在容量不足的时候，抛出异常。 
        - put 方法在容量不足的时候，阻塞等待。 
        - offer 方法
            - 单参数 offer 方法，不阻塞。容量不足的时候，返回 false。当前新增数据操作放弃。 
            - 三参数 offer 方法（offer(value,times,timeunit)），容量不足的时候，阻塞 times 时长（单 位为 timeunit），如果在阻塞时长内，有容量空闲，新增数据返回 true。如果阻塞时长范围 内，无容量空闲，放弃新增数据，返回 false。 
- DelayQueue 延时队列。根据比较机制，实现自定义处理顺序的队列。常用于定时任务。 如：定时关机。 
- LinkedTransferQueue 转移队列，使用 transfer 方法，实现数据的即时处理。没有消费者，就阻塞。 
- SynchronusQueue 同步队列，是一个容量为 0 的队列。是一个特殊的 TransferQueue。 必须现有消费线程等待，才能使用的队列。 add 方法，无阻塞。若没有消费线程阻塞等待数据，则抛出异常。 put 方法，有阻塞。若没有消费线程阻塞等待数据，则阻塞。 

# 线程池

## Executor 
- 线程池顶级接口。定义方法，void execute(Runnable)。方法是用于处理任务的一个服务 方法。调用者提供 Runnable 接口的实现，线程池通过线程执行这个 Runnable。服务方法无 返回值的。是 Runnable 接口中的 run 方法无返回值。 常用方法 - void execute(Runnable) 作用是： 启动线程任务的。 

## ExecutorService 
- Executor 接口的子接口。提供了一个新的服务方法，submit。有返回值（Future 类型）。 submit 方法提供了 overload 方法。
    - 其中有参数类型为 Runnable 的，不需要提供返回值的； 
    - 有参数类型为 Callable，可以提供线程执行后的返回值。 Future，是 submit 方法的返回值。代表未来，也就是线程执行结束后的一种结果。如返回值。 
- 常见方法  void execute(Runnable)， Future submit(Callable)， Future submit(Runnable) 
- 线程池状态： Running， ShuttingDown， Termitnaed 
    - Running 线程池正在执行中。活动状态。 
    - ShuttingDown 线程池正在关闭过程中。优雅关闭。一旦进入这个状态，线程池不再接 收新的任务，处理所有已接收的任务，处理完毕后，关闭线程池。 
    - Terminated 线程池已经关闭。 

## Future 
- 未来结果，代表线程任务执行结束后的结果。获取线程执行结果的方式是通过get方法获取的。get无参，阻塞等待线程执行结束，并得到结果。get有参，阻塞固定时长，等待线程执行结束后的结果，如果在阻塞时长范围内，线程未执行结束，抛出异常。 
- 常用方法： T get()  T get(long, TimeUnit) 

## Callable
- 可执行接口。 类似 Runnable 接口。也是可以启动一个线程的接口。其中定义的方法是 call。call 方法的作用和 Runnable 中的 run 方法完全一致。call 方法有返回值。 
- 接口方法 ： Object call();相当于 Runnable 接口中的 run 方法。区别为此方法有返回值。 不能抛出已检查异常。 
- 和 Runnable 接口的选择 - 需要返回值或需要抛出异常时，使用 Callable，其他情况可 任意选择

## Executors 
- 工具类型。为 Executor 线程池提供工具方法。可以快速的提供若干种线程池。如：固定 容量的，无限容量的，容量为 1 等各种线程池。 
- 线程池是一个进程级的重量级资源。默认的生命周期和 JVM 一致。当开启线程池后， 直到 JVM 关闭为止，是线程池的默认生命周期。如果手工调用 shutdown 方法，那么线程池 执行所有的任务后，自动关闭。 
    - 开始 创建线程池。 
    - 结束 JVM 关闭或调用 shutdown 并处理完所有的任务。 
- 类似 Arrays，Collections 等工具类型的功用。 

## FixedThreadPool 
- 容量固定的线程池。活动状态和线程池容量是有上限的线程池。所有的线程池中，都有 一个任务队列。使用的是 BlockingQueue<Runnable>作为任务的载体。当任务数量大于线程池容量的时候，没有运行的任务保存在任务队列中，当线程有空闲的，自动从队列中取出任 务执行。 
- 使用场景： 大多数情况下，使用的线程池，首选推荐 FixedThreadPool。OS 系统和硬件 是有线程支持上限。不能随意的无限制提供线程池。 线程池默认的容量上限是 Integer.MAX_VALUE。 
    常见的线程池容量： 
        - PC  200。 
        - 服务器  1000-10000
        - queued tasks 任务队列 
        - completed tasks 结束任务队列

## CachedThreadPool 
- 缓存的线程池。容量不限（Integer.MAX_VALUE）。自动扩容。容量管理策略：如果线程池中的线程数量不满足任务执行，创建新的线程。每次有新任务无法即时处理的时候，都会创建新的线程。当线程池中的线程空闲时长达到一定的临界值（默认 60 秒），自动释放线程。
- 应用场景： 内部应用或测试应用。 
    - 内部应用，有条件的内部数据瞬间处理时应用，如：电信平台夜间执行数据整理（有把握在短时间内处理完所有工作，且对硬件和软件有足够的信心）
    - 测试应用，在测试的时候，尝试得到硬件或软件的最高负载量，用于提供 FixedThreadPool 容量的指导

## ScheduledThreadPool
- 计划任务线程池。可以根据计划自动执行任务的线程池。 scheduleAtFixedRate(Runnable, start_limit, limit, timeunit) 
    - runnable 要执行的任务。 
    - start_limit 第一次任务执行的间隔。 
    - limit 多次任务执行的间隔。 
    - timeunit 多次任务执行间隔的时间单位。 
- 使用场景： 计划任务时选用（DelaydQueue），如：电信行业中的数据整理，每分钟整理，每小时整理，每天整理等

## SingleThreadExceutor
- 单一容量的线程池。 
- 使用场景： 保证任务顺序时使用。如： 游戏大厅中的公共频道聊天。秒杀。 

##  ForkJoinPool
- 分支合并线程池（mapduce 类似的设计思想）。适合用于处理复杂任务。 
    - 初始化线程容量与 CPU 核心数相关。 
    - 线程池中运行的内容必须是 ForkJoinTask 的子类型（RecursiveTask,RecursiveAction）。 
- ForkJoinPool 分支合并线程池。 可以递归完成复杂任务。要求可分支合并的任务必须是 ForkJoinTask 类型的子类型。
    - 其中提供了分支和合并的能力。ForkJoinTask 类型提供了两个 抽象子类型，RecursiveTask 有返回结果的分支合并任务,RecursiveAction 无返回结果的分支合 并任务。（Callable/Runnable）compute 方法：就是任务的执行逻辑。 
- ForkJoinPool 没有所谓的容量。默认都是 1 个线程。根据任务自动的分支新的子线程。 当子线程任务结束后，自动合并。所谓自动是根据 fork 和 join 两个方法实现的。 
- 应用： 主要是做科学计算或天文计算的。数据分析的

##  ThreadPoolExecutor 
- 线程池底层实现。除 ForkJoinPool 外，其他常用线程池底层都是使用 ThreadPoolExecutor实现的。 
- public ThreadPoolExecutor (int corePoolSize,  int maximumPoolSize,   long keepAliveTime,TimeUnit unit, BlockingQueue<Runnable> workQueue  ); 
    - 核心容量，创建线程池的时候，默认有多少线程。也是线程池保持 的最少线程数
    - 最大容量，线程池最多有多少线程
    - 生命周期，0 为永久。当线程空闲多久后，自动回收 
    - 生命周期单位，为生命周期提供单位，如：秒，毫秒
    - 任务队列，阻塞队列。注意，泛型必须是 Runnable
- 使用场景： 默认提供的线程池不满足条件时使用


