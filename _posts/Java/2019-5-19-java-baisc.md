---
layout: post
title: Java 基础
categories: Java
description: Java 基础
keywords: Java
---

# Java 特点

Java是跨平台的
- Java程序的跨平台主要是指字节码文件可以在任何具有Java虚拟机的计算机或者电子设备上运行，Java虚拟机中的Java解释器负责将字节码文件解释成为特定的机器码进行运行。 

Java是简单的 
- 不再有#include 和#define 等预处理功能
- 不再有struct,union及typedef 
- 不再有函数、 
- 不再有指针、不再有多重继承 
- 不再有goto      
- 不再有操作符重载(Operatior Overloading) 
- 不再有全局变量           取消自动类型转换,要求强制转换 
- 不再有手动内存管理

Java是安全的
- Java取消了强大但又危险的指针。由于指针可进行移动运算，指针可随便指向一个内存区域， 而不管这个区域是否可用，这样做是危险的，因为原来这个内存地址可能存储着重要数据 或者是其他程序运行所占用的， 并且使用指针也容易数组越界。
- Java提供了自动内存管理机制，由垃圾回收器在后台自动回收, 
- Java在字节码的传输过程中使用了公开密钥加密机制(PKC)。
- 而在运行环境提供了四级安全性保障机制： 
- 字节码校验器 -类装载器 -运行时内存布局 -文件访问限制

Java是完全面向对象的
- Java和C++都是面向对象语言。也就是说，它们都能够实现面向对象思想（封装，继承，多态）。
- 由于C++为了照顾大量C语言使用者而兼容了C，使得自身仅仅成为了带类的C语言，多少影 响了其面向对象的彻底性！
- Java则是完全的面向对象语言，它句法更清晰，规模更小，更易学。它是在对多种程序设计 语言进行了深入细致研究的基础上，据弃了其他语言的不足之处，从根本上解决了c++的固 有缺陷。

Java是健壮的
- Java的强制类型机制、异常处理、垃圾的自动收集等是Java程序健壮性的重要保证。
- 对指针的丢弃是Java的明智选择。
- Java的安全检查机制使得Java更具健壮性。 


## Java跨平台原理

总结1：Java运行过程
- Java程序的运行分为两步：先编译再解释执行
- 通过“编译器”将Java源程序编译成Java 字节码文件（.class）(字节码文件采用结构中立 的中间文件格式)
- 通过不同的“虚拟机”将Java字节码文件解释为对应机器语言并执行 

总结2：Java跨平台和C跨平台的区别
- Java：一次编译，到处运行     C：多次编译，到处运行
- 在互联网情况下，平台各异，Java的跨平台更具有优势
- Java可以跨所有平台吗：要看有没有提供并安装相应的虚拟机
- Java的运行速度没有C语言快 
- Java需要将class文件解释成机器码再执行；C执行执行机器码  

总结3：字节码文件bytecode 
- .class文件  二进制文件 
- 格式中立、平台无关的二进制文件 
- 是编译的产物，是解释的原料

总结4：Java虚拟机 JVM 
- JVM是Java Virtual Machine（Java虚拟机）的缩写 
- JVM是一种用于计算设备的规范，它是一个虚构出来的计算机，是通过在实际的计算机上仿真模拟各种计算机功能来实现的。 
- JVM就是一个虚拟的用于执行bytecodes字节码的计算机
- Java虚拟机是Java最核心技术，也是跨平台的基础。
- Java语言使用Java虚拟机屏蔽了与具体平台相关的信息，使得Java语言编译程序只需生成在Java虚拟机上运行的目标代码（字节码），就可以在多种平台上不加修改地运行。 
- Java虚拟机在执行字节码时，把字节码解释成具体平台上的机器指令执行。这就是Java的能 够“一次编译，到处运行”的原因

总结5：JDK、JRE、JVM的区别联系 
- JDK: 
    - Java Development Kit 
    - 针对Java开发员的产品 
- JRE: 
    - Java Runtime Environment 
    - 是运行Java程序所必须的环境集合 
- JVM 
    - Java Virtual Machine 
    - 解释运行Java字节码文件，跨平台的核心

# 数据类型和运算符

## 数据类型
数据类型
- 基本数据类型
    - 数值型
        - 整数类型(byte,short,int,long)
        - 浮点类型(float,double)
    - 字符型(char)(两个字节)
    - 布尔型(boolean)(1bit)
- 引用数据类型
    - 类(class)
    - 接口(interface)
    - 数组



Java是一种强类型语言 
- 常量和变量是有数据类型的 
- 常量和变量都必须声明其数据类型

 
## 运算符
运算符
- 算术运算符
- 赋值运算符
- 扩展赋值运算符
- 关系运算符
- 逻辑运算符
- 位运算符
- 条件运算符

优先级
- 赋值<三目<逻辑<关系<算术<单目

## 基本数据类型的类型转换
- 自动类型转换
- 强制类型转换

# 流程控制语句
- 顺序
- 分支
    - if else
    - switch
- 循环
    - while
    - do while
    - for
- 跳转
    - break
    - continue 
    - return
- 多重循环
    - break label
    - continue label

# 面向对象编程

## 基础1
对象和类
- 对象 具体的事物
- 类 对象的抽象
- 关系 
    - 特殊到一般，具体到抽象
    - 类可以看成一类对象的模板，对象可以看成该类的一个具体实例。
    - 类是用于描述同一类形的对象的一个抽象的概念，类中定义了这一类对象所应具有的静态和动态属性。

类的组成
- 属性field
- 方法method
- 构造方法constructor
- 其他 代码块静态代码块 内部类

||成员变量|局部变量|
|声明位置|类中|方法中|
|作用范围|当前类中方法|当前方法|
|内存存放位置|堆|栈|
|默认值|有|无|

引用类型
- Java 语言中除基本类型之外的变量类型都称之为引用类型
-  Java中的对象和数组是通过引用对其操作的.
    - 引用可以理解为一种受限的指针 
    - 指针是可以进行与整数做加减运算的，两个指针之间也可以进行大小比较运算和相减运算。引用不行，只能进行赋值运算。 
    - 引用就是一个变量或对象的别名（引用的本质是一个对象）；指针是一个段内存空间的地址(指向存储一个变量值的空间或一个对象的空间)

内存分析
- 栈:存放局部变量
- 堆:存放new出的对象
- 方法区(静态区):
    - JVM只有一个方法区，所有线程共享
    - 存放不变或唯一的内容：类的信息(代码)、static变量、字符串常量
    - 实际上也是堆


this关键字
- this的作用: 
    - this表示的是当前对象本身， 
    - 更准确地说，this代表当前对象的一个引用。 
- 普通方法中使用this。 
    - 区分类成员属性和方法的形参. 
    - 调用当前对象的其他方法（可以省略） 
    - 位置：任意 
- 构造方法中使用this。 
    - 使用this来调用其它构造方法 
    - 位置：必须是第一条语句 

static关键字
- 在类中，用static声明的成员变量为静态成员变量 ,或者叫做： 类属性，类变量. 
    - 它为该类的公用变量，属于类，被该类的所有实例共享，在类被载入时被显式初始化， 
    - 对于该类的所有对象来说，static成员变量只有一份。被该类的所有对象共享！！ 
    - 以使用”对象.类属性”来调用。不过，一般都是用“类名.类属性” 
    - static变量置于方法区中！
- 用static声明的方法为静态方法 
    - 不需要对象，就可以调用(类名.方法名) 
    - 在调用该方法时，不会将对象的引用传递给它，所以在static方法中不可访问非static的成员。 
    - 静态方法不能以任何方式引用this和super关键字

静态初始化块
- 如果希望加载后，对整个类进行某些初始化操作，可以使用static初始化块。 
- 类第一次被载入时先执行static代码块；类多次载入时，static代码块只执行一次；Static 经常用来进行static变量的初始化。 
- 是在类初始化时执行，不是在创建对象时执行。 
- 静态初始化块中不能访问非static成员。 

垃圾回收算法
- 引用计数法 (无法处理循环引用)
- 复制(Copying)
- 标记-清除(Mark-Sweep) 暂停应用，内存碎片
- 标记-整理(Mark-Compact) (标记+复制)
- 引用可达法(根搜索算法)

堆划分
- 新生代
    - Eden
    - Survivor(From To)
- 老年代

分代垃圾收集器
- 新生代(young)
    - Serial  一条收集线程 安全点 暂停工作线程 Stop The World 复制算法
    - ParNew  Serial多线程版本   缩短安全点时间长度  单CPU效率无提高 多CPU 
    - Parallel Scavenge 关注系统吞吐量    复制算法
- 老年代(tenred)
    - CMS(Coucurrent Mark Sweep)  多并发低暂停  标记-清除算法 可与Serial ParNew 共同使用
    - Serial Old  单线程收集器         标记-整理算法  可与新生代any共同使用
    - Parallel Old      与Parallel Scavenge共同使用
- 分区收集器G1(Gabage First)(新老)


通用分代垃圾收集器
- minor GC (年轻代)
- major GC (老年代)
- full GC (System.gc()非立即启动)(老年代或持久代堆空间满)

垃圾回收机制关键点
- 垃圾回收机制只回收JVM堆内存里的对象空间。 
- 对其他物理连接，比如数据库连接、输入流输出流、Socket连接无能为力 
- 现在的JVM有多种垃圾回收实现算法，表现各异。 
- 垃圾回收发生具有不可预知性，程序无法精确控制垃圾回收机制执行。
- 可以将对象的引用变量设置为null，暗示垃圾回收机制可以回收该对象。 
- 程序员可以通过System.gc()或者Runtime.getRuntime().gc()来通知系统进行垃圾回收，会有一些效果，但是系统是否进行垃圾回收依然不确定。 
- 垃圾回收机制回收任何对象之前，总会先调用它的finalize方法（如果覆盖该方法，让一 个新的引用变量重新引用该对象，则会重新激活对象）。 
- 永远不要主动调用某个对象的finalize方法，应该交给垃圾回收机制调用。




## 基础2

面向对象特征
- 继承
- 封装、隐藏
- 多态


### 封装
封装访问控制符
||同一个类|同一个包|子类|所有类|
|private|\*||||
|default|\*|\*|||
|protect|\*|\*|\*||
|public|\*|\*|\*|\*|

### 继承
-  通过继承可以简化类的定义，实现代码的重用
- 子类继承父类的成员变量和成员方法，但不继承父类的构造方法
- java中只有单继承，没有像c++那样的多继承。多继承会引起混乱，使得继承链过于复杂，系统 难于维护。就像我们现实中，如果你有多个父母亲，那是一个多么混乱的世界啊。多继承，就是为了实现代码的复用性，却引入了复杂性，使得系统类之间的关系混乱。
- java中的多继承，可以通过接口来实现
- 如果定义一个类时，没有调用extends，则它的父类是：java.lang.Object。

方法重写(override)
- 在子类中可以根据需要对从基类中继承来的方法进行重写。 
- 重写方法必须和被重写方法具有相同方法名称、参数列表和返回类型。 
- 重写方法不能使用比被重写方法更严格的访问权限。（由于多态）
- “==”：方法名、形参列表相同。 
- “≤”：返回值类型和异常类型，子类小于等于父类。 
- “≥”：访问权限，子类大于等于父类 

super关键字
- super是直接父类对象的引用。 
- 可以通过super来访问父类中被子类覆盖的方法或属性。

### 多态
多态性
- 多态性是OOP中的一个重要特性，主要是用来实现动态联编的，换句话说，就是程序的最终状态只有 在执行过程中才被决定而非在编译期间就决定了。这对于大型系统来说能提高系统的灵活性和扩展
性。
- 引用变量的两种类型： 
    - 编译时类型（模糊一点，一般是一个父类） 由声明时的类型决定。 
    - 运行时类型（运行时，具体是哪个子类就是哪个子类）由实际对应的对象类型决定。  
- 多态的存在要有3个必要条件：
    - 要有继承，要有方法重写，父类引用指向子类对象

引用类型的类型转换
- 子类转换为父类：自动转换 
    - 上转型对象不能操作子类新增的成员变量和方法。 
    - 上转型对象可以操作子类继承或重写的成员变量和方法 
    - 如果子类重写了父类的某个方法，上转型对象调用该方法时，是调用的重写方法。 
- 父类转换为子类：强制转换
    -（绝不是做手术，而是父类的真面目就是一个子类，否则会出现类型转换错误）

抽象类
- 是一种模版模式。抽象类为所有子类提供了一个通用模版，子类可以在这个模版基础上进行扩 展。 
- 通过抽象类，可以避免子类设计的随意性。通过抽象类，我们就可以做到严格限制子类的设计， 使子类之间更加通用。 

- 抽象方法和抽象类均必须用abstract来修饰。 
- 抽象方法没有方法体，只需要声明不需实现。
- 有抽象方法的类只能定义能抽象类 
- 相反抽象类里面的方法不一定全是抽象方法，也可能没有抽象方法。
- 抽象类可以包含属性、方法、构造方法。 
- 抽象类不能实例化，及不能用new来实例化抽象类，只能用来被子类调用。 
- 抽象类只能用来继承。 
- 抽象方法必须被子类实现。抽象类的子类必须覆盖所有的抽象方法才能被实例化，否则还是抽象类

接口
- 接口就是比“抽象类”还“抽象”的“抽象类”，可以更加规范的对子类进行约束。全面地专 业地实现了：规范和具体实现的分离。 
- 接口就是规范，定义的是一组规则，体现了现实世界中“如果你是…则必须能…”的思想。如果你 是天使，则必须能飞。如果你是汽车，则必须能跑。如果你好人，则必须干掉坏人；如果你是坏人，则必须欺负好人。 
- 接口的本质是契约，就像我们人间的法律一样。制定好后大家都遵守。 
- 项目的具体需求是多变的，我们必须以不变应万变才能从容开发，此处的“不变”就是“规 范”。因此，我们开发项目往往都是面向接口编程！

接口相关规则
- 接口中所有方法都是抽象的。 
- 即使没有显式的将接口中的成员用public标示，也是public访问类型的 
- 接口中变量默认用 public static final标示，所以接口中定义的变量就是全局静态常量。
- 可以定义一个新接口，用extends去继承一个已有的接口 
- 可以定义一个类，用implements去实现一个接口中所有方法。 
- 可以定义一个抽象类，用implements去实现一个接口中部分方法。

 如何实现接口 
 - 子类通过implements来实现接口中的规范 
 - 接口不能创建实例，但是可用于声明引用变量类型。 
 - 一个类实现了接口，必须实现接口中所有方法，并且这些方法只能是public的。 
 - Java的类只支持单继承，接口支持多继承

多继承
 - C++支持多重继承，Java支持单重继承 
 - C++多重继承的危险性在于一个类可能继承了同一个方法的不同实现，会导致系统崩溃。 
 - Java中，一个类只能继承一个类，但同时可以实现多个接口，既可以实现多重继承的效 果和功能，也避免的多重继承的危险性。
 - extends 必须位于implements之前

内部类
- 将一个类定义置入另一个类定义中就叫作“内部类” 
    - 类中定义的内部类特点 - 内部类作为外部类的成员，可以直接访问外部类的成员（包括private成员），反之则不行。 
    - 内部类做为外部类成员，可声明为private、默认、protected或public。 
    - 内部类成员只有在内部类的范围之内是有效的。 
    - 用内部类定义在外部类中不可访问的属性。这样就在外部类中实现了比外部类的private还要小的 访问权限。 
    - 编译后生成两个类： OuterClass.class 和OuterClass$InnerClass.class
- 内部类分类 
    - 成员内部类(静态内部类、非静态内部类) 方法内部类     匿名内部类

匿名内部类Anonymous 
- 可以实现一个接口，或者继承一个父类 
- 只能实现一个接口 
- 适合创建那种只需要一次使用的类，不能重复使用。比较常见的是在图形界面编程GUI里用得到。 
- 匿名内部类要使用外部类的局部变量，必须使用final修饰该局部变量

# Java常用类
基本数据类型包装类
字符串相关类String，StringBuffer，StringBuilder
时间处理相关类Date，DateFormat，SimpleDateFormat，Calender
枚举类
Math类和Random类
File类

包装类作用
- 提供：字符串、基本类型数据、对象之间互相转化的方式！ 、
- 包含每种基本数据类型的相关属性如最大值、最小值等

自动装箱
- 基本类型就自动地封装到与它相同类型的包装中，如：Integer i = 100; 
- 本质上是，编译器编译时为我们添加了：Integer i = Integer.valueOf(100); 
自动拆箱
- 包装类对象自动转换成基本类型数据。如： int  a = new Integer(100); 
- 本质上，编译器编译时为我们添加了： int  a = new Integer(100).intValue();

String
- Java字符串就是Unicode字符序列

StringBuffer和StringBuilder非常类似，均代表可变的字符序列，而且方法也一样
- String：不可变字符序列 
- StringBuilder：可变字符序列、效率高、线程不安全 
- StringBuilder：可变字符序列、效率低、线程安全

# 异常机制

异常分类
- Error 
    - Error类层次描述了Java运行时系统内部错误和资源耗尽错误，一般指与JVM或动态加载等相关的 问题，如虚拟机错误，动态链接失败，系统崩溃等。
    - 这类错误是我们无法控制的，同时也是非常罕见的错误。所以在编程中，不去处理这类错误。
    - 打开JDK包：java.lang.Error，查看他的所有子类
    - 注：我们不需要管理Error！
- Exception 
    - 所有异常类的父类，其子类对应了各种各样可能出现的异常事件。 
    - Exception分类 
        - 运行时异常Runtime Exception（unchecked Exception）- 可不必对其处理，系统自动检测处理 
        - 一类特殊的异常，如被 0 除、数组下标超范围等，其产生比较频繁，处理麻烦，如果显式的声 明或捕获将会对程序可读性和运行效率影响很大 
        - 检查异常 Checked  Exception  
        - 必须捕获进行处理，否则会出现编译错误

异常处理
- try-catch-finally 
    - 在try-catch块后加入finally块，可以确保无论是否发生异常，finally块中的代码总能被执行 
        - 无异常  try-finally 
        - 有异常   try-catch-finally 
    - 通常在finally中关闭程序块已打开的资源，比如：文件流、释放数据库连接等。
    - finally块中语句不执行的唯一情况 
        - 异常处理代码中执行System.exit(1)退出Java虚拟机 
    - Finally块的具体执行过程 
        - 执行try或catch中代码 
        - 遇到return/throw，先执行finally中语句块 
        - 执行return/throw

注意 
- try-finally也可以呀 
- 遇到了return和throw还会执行finally语句；遇到System.exit()就不会执行finally语句了 
- throw了运行时异常，则方法声明中不需要throws