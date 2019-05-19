---
layout: post
title: Java Annotation Reflection
categories: Java
description: Java 注解 反射 动态性
keywords: Java
---



注解
Annotation的作用： 
- 不是程序本身，可以对程序作出解释。(这一点，跟注释没什么区别) 
- 可以被其他程序(比如：编译器等)读取。(注解信息处理流程，是注解和注释的重大区别。如果没有注解信息处理流程，则注解毫无意义) 

Annotation的格式： 
- 注解是以“@注释名”在代码中存在的，还可以添加一些参数值，例如： @SuppressWarnings(value="unchecked")。 

Annotation在哪里使用?
- 可以附加在package, class, method, field等上面，相当于给它们添加了额外的辅助信 息，我们可以通过反射机制编程实现对这些元数据的访问。

 @Override 
 - 定义在java.lang.Override中，此注释只适用于修辞方法，表示一个方法声明打算重写超类中的另一个方法声明。 
 @Deprecated 
 - 定义在java.lang.Deprecated中，此注释可用于修辞方法、属性、类，表示不鼓励程序员使用这样的元素，通常是因为它很危险或存在更 好的选择。
@SuppressWarnings 
- 定义在java.lang.SuppressWarnings中，用来抑制编译时的警告信息 。 
- 与前两个注释有所不同，你需要添加一个参数才能正确使用，这些参数值都是已经定义好了的，

自定义注解
- 使用@interface自定义注解时，自动继承了java.lang.annotation.Annotation接口 
- 格式为： 
    - public @interface 注解名 {定义体} 
    - 其中的每一个方法实际上是声明了一个配置参数。 
    - 方法的名称就是参数的名称 
    - 返回值类型就是参数的类型（返回值类型只能是基本类型、Class、String、enum） 
    - 可以通过default来声明参数的默认值。 
    - 如果只有一个参数成员，一般参数名为value

元注解
元注解的作用就是负责注解其他注解。 Java定义了4个标准的 meta-annotation类型，它们被用来提供对其它 annotation 类型作说明。 
- @Target 用于描述注解的使用范围（即:被描述的注解可以用在什么地方）
- @Retention 表示需要在什么级别保存该注释信息，用于描述注解的生命周期
- @Documented 
- @Inherited 


Java的动态性
- 反射机制
- 动态编译
- 动态执行JavaScript代码
- 动态字节码操作

动态语言
- 程序运行时，可以改变程序结构或变量类型。典型的语言： Python、ruby、javascript等
- C,  C++,  JAVA不是动态语言，JAVA可以称之为“准动态语 言”。但是JAVA有一定的动态性，我们可以利用反射机制、 字节码操作获得类似动态语言的特性。

反射机制
- 指的是可以于运行时加载、探知、使用编译期间完全未知的类。 
- 程序在运行状态中，可以动态加载一个只有名称的类，对于任意一个已加载的类，都能够知道这个类的所有属性和方法；对于任意一个对象，都能够调用它的任意一个方法和属性；
 Class c = Class.forName(""); 
- 加载完类之后，在堆内存中，就产生了一个 Class 类型的对象（一个类只有一个 Class对象），这个对象就包含了完整的类的结构信息。 我们可以通过这个对象看到类的结构。这个对象就像一面镜子，透过 这个镜子看到类的结构，所以，我们形象的称之为：反射。 


Class类
- Class类的对象包含了某个被加载类的结构。一个被加载的类对应一个 Class对象。 
- 当一个class被加载，或当加载器（class loader）的defineClass()被 JVM调用，JVM 便自动产生一个Class 对象。 
- Class类是Reflection的根源。 针对任何您想动态加载、运行的类，唯有先获得相应的Class 对象

反射机制的常见作用
- 动态加载类、动态获取类的信息（属性、方法、构造器）
- 动态构造对象
- 动态调用类和对象的任意方法、构造器
- 动态调用和处理属性
- 获取泛型信息
- 处理注解

反射操作泛型(Generic)
- Java采用泛型擦除的机制来引入泛型。Java中的泛型仅仅是给编译器javac使用的，确保数据的安全性和免去强制类型转换的麻烦。但是，一旦编译完成，所有的和泛型有关的类型全部擦除。
- 为了通过反射操作这些类型以迎合实际开发的需要，Java就新增了ParameterizedType，GenericArrayType，TypeVariable 和WildcardType几种类型来代表不能被归一到Class类中的类型但是又和原始类型齐名的类型。
    - ParameterizedType: 表示一种参数化的类型，比如Collection<String> 
    - GenericArrayType: 表示一种元素类型是参数化类型或者类型变量的数组类型 
    - TypeVariable: 是各种类型变量的公共父接口 
    - WildcardType: 代表一种通配符类型表达式，比如?, ? extends Number, ? super Integer【wildcard是一个单词：就是“通配符”】

反射操作注解(annotation)
可以通过反射API:getAnnotations, getAnnotation获得相关的注解信息


反射机制性能
 setAccessible 
 - 启用和禁用访问安全检查的开关,值为 true 则指示反射的对象在使用时应该取消 Java 语言访问检查。值为 false 则指示反射的对象应该实施 Java 语言访问检查。并不是为true 就能访问为false就不能访问。 
 - 禁止安全检查，可以提高反射的运行速度。

动态编译

