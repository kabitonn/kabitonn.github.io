---
layout: post
title: Java 容器
categories: Java
description: Java 容器
keywords: Java
---

# 集合

容器分类
- Collection
    - List
        - ArrayList
        - LinkedList
    - Set
        - HashSet
            - LinkedHashSet
        - TreeSet
- Map
    - HashMap
        - LinkedHashMap
    - TreeMap


List
- List 
    - 特点：有序  不唯一（可重复） 
- ArrayList 线性表中的顺序表 
    - 在内存中分配连续的空间，实现了长度可变的数组 
    - 优点：遍历元素和随机访问元素的效率比较高 
    - 缺点：添加和删除需大量移动元素效率低，按照内容查询效率低， 
- LinkedList 线性表中双向链表 
    - 采用双向链表存储方式。 
    - 缺点：遍历和随机访问元素效率低下 
    - 优点：插入、删除元素效率比较高（但是前提也是必须先低效率查询才可。如果插入删除发 生在头尾可以减少查询次数）

Set
- Set 
    - 特点：无序  唯一（不重复） 
- HashSet 
    - 采用Hashtable哈希表存储结构（神奇的结构） 
    - 优点：添加速度快  查询速度快 删除速度快 
    - 缺点：无序 
- LinkedHashSet 
    - 采用哈希表存储结构，同时使用链表维护次序 
    - 有序（添加顺序） 
- TreeSet 
    - 采用二叉树（红黑树）的存储结构 
    - 优点：有序  查询速度比List快（按照内容查询） 
    - 缺点：查询速度没有HashSet快

Map
- Map 
    - 特点 key-value映射 
- HashMap 
    - Key无序 唯一 （Set） 
    - Value 无序  不唯一 （Collection） 
- LinkedHashMap 
    - 有序的HashMap   速度快 
- TreeMap 
    - 有序   速度没有hash快 
- 问题：Set和Map有关系吗？ 
    - 采用了相同的数据结构，只用于map的key存储数据，就是Set

Iterator
- 所有集合类均未提供相应的遍历方法，而是把把遍历交给迭代器完成。迭代器为集 合而生，专门实现集合遍历
- Iterator是迭代器设计模式的具体实现
- Iterator方法 
    - boolean hasNext(): 判断是否存在另一个可访问的元素 
    - Object next(): 返回要访问的下一个元素 
    - void remove(): 删除上次访问返回的对象。
- ListIterator和Iterator的关系 
    - public interface ListIterator<E> extends Iterator<E> 
    - 都可以遍历List
- ListIterator和Iterator的区别 
    - 使用范围不同 
        - Iterator可以应用于更多的集合，Set、List和这些集合的子类型。 
        - 而ListIterator只能用于List及其子类型。 
    - 遍历顺序不同 
        - Iterator只能顺序向后遍历; ListIterator还可以逆序向前遍历 
        - Iterator可以在遍历的过程中remove();ListIterator可以在遍历的过程中remove()、add()、set() 
    - ListIterator可以定位当前的索引位置，nextIndex()和previousIndex()可以实现。Iterator没有此功能。


旧的集合类

Vector 
- 实现原理和ArrayList相同，功能相同，都是长度可变的数组结构，很多情况下可以互用 
- 两者的主要区别如下 
    - Vector是早期JDK接口，ArrayList是替代Vector的新接口 
    - Vector线程安全，效率低下；ArrayList重速度轻安全，线程非安全 
    - 长度需增长时，Vector默认增长一倍，ArrayList增长50% 

Hashtable类 
- 实现原理和HashMap相同，功能相同，底层都是哈希表结构，查询速度快，很多情况下可互用 
- 两者的主要区别如下 
    - Hashtable是早期JDK提供的接口，HashMap是新版JDK提供的接口 
    - Hashtable继承Dictionary类，HashMap实现Map接口 
    - Hashtable线程安全，HashMap线程非安全 
    - Hashtable不允许null值，HashMap允许null值

早期集合类Vector、Hashtable：线程安全的 
- 是怎么保证线程安排的，使用synchronized修饰方法 

为了提高性能，使用ArrayList、HashMap替换，线程不安全，但是性能好。使用ArrayList、 HashMap，需要线程安全怎么办呢？ 
- 使用Collections.synchronizedList(list);Collections.synchronizedMap(m);解决 
- 底层使用synchronized代码块锁 
- 虽然也是锁住了所有的代码，但是锁在方法里边，并所在方法外边性能可以理解为稍有提高吧。 毕竟进方法本身就要分配资源的 

在大量并发情况下如何提高集合的效率和安全呢？
- 提供了新的线程同步集合类，java.util.concurrent包下，使用Lock锁 
- ConcurrentHashMap、CopyOnWriteArrayList 、CopyOnWriteArraySet： 
- 注意 不是CopyOnWriteHashSet

 集合和数组的比较 
 - 数组不是面向对象的，存在明显的缺陷，集合完全弥补了数组的一些缺点，比数组更灵活更实 用，可大大提高软件的开发效率而且不同的集合框架类可适用于不同场合。具体如下： 
    - 1 : 数组容量固定且无法动态改变，集合类容量动态改变。 
    - 2：数组能存放基本数据类型和引用数据类型的数据，而集合类中只能放引用数据类型的数据。 
    - 3：数组无法判断其中实际存有多少元素，length只告诉了array容量；集合可以判断实际存有多 少元素，而对总的容量不关心 
    - 4：集合有多种数据结构（顺序表、链表、哈希表、树等）、多种特征（是否有序，是否唯一）、 不同适用场合（查询快，便于删除、有序），不像数组仅采用顺序表方式 
    - 5：集合以类的形式存在，具有封装、继承、多态等类的特性，通过简单的方法和属性调用即可 实现各种复杂操作，大大提高软件的开发效率。

