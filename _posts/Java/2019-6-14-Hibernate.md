---
layout: post
title: Hibernate
categories: Java
description: Hibernate
keywords: Hibernate
---


# Hibernate

- Hibernate：是一个轻量级的持久化框架。没有侵入性。是一个orm映射框架。简化了jdbc操作。极大了提高了开发效率。提供了缓存机制。强大的查询机制。支持多种数据库（数据库移植）。
- 解决阻抗不匹配
- 映射规则
    - 将类名映射数据库的表名 
    - 将类的属性名映射为表的字段名 
    - 将类的属性类型映射为表的字段的数据类型 
    - 将对象的属性映射为表的记录


## 案例
- 创建项目
- 导入jar包
- 编写 hibernate.cfg.xml 文件
- 编写实体类(POJO)
- 编写实体类的映射文件(\*.,hbm.xml)---将映射文件加入到 hibernate.cfg.xml 
- 编写测试类 

### hibernate.cfg.xml 配置文件
- hbm2ddl.auto
- show_sql
```xml
<hibernate-configuration>
    <!-- session工厂的配置 -->
    <session-factory>
        <!-- jdbc配置 -->
        <property name="connection.driver_class">
            com.mysql.jdbc.Driver
        </property>
        <property name="connection.url"><!-- url的两种配置方式 -->
            <!-- jdbc:mysql://localhost:3306/hibernate4-->
            jdbc:mysql:///hibernate4
        </property>
        <property name="connection.username">root</property>
        <property name="connection.password">1234</property>
        <!-- 数据库方言
            hibernate支持多种数据库，通过设置方言hibernate才知道应该
            生成对应数据库的sql语句：hibernate支持的数据库的方言hibernate.properties文件中都有
         -->
        <property name="dialect">
            org.hibernate.dialect.MySQL5Dialect
        </property>
        <!-- 打印hibernate生成的sql语句 -->
        <property name="show_sql">true</property>
        <!-- 格式化打印的sql语句 -->
        <property name="format_sql">true</property>
        <!-- 根据不同值，进行数据表表的操作
            create  每次执行 都删除原有表，然后创建新表
            create-drop 执行前创建表，执行后删除表
            update 如果有则不改变表，如果没有则创建
         -->
        <property name="hbm2ddl.auto">update</property>
        <!-- 将所有映射文件添加到这里 -->
        <mapping resource="cn/sxt/pojo/User.hbm.xml"/>
    </session-factory>
</hibernate-configuration>
```

### \*.hbm.xml 映射文件
- package指定类的包名 可以不配置 如果不配置 那么在配置class的name时需要指定该类所在包
- id主键的配置，
    - name配置类的属性名
    - column数据库字段名，不写和属性名一致
    - type 指定属性的类型
    - length指定字段的长度
- 主键的生成策略
    - increment
    - identity
    - native
    - sequence
    - assigned
- property是配置类的属性,name指属性名

```xml
<!-- 类的映射文件信息 -->
<!-- package指定类的包名 可以不配置 如果不配置 那么在配置class的name时需要指定该类所在包-->
<hibernate-mapping>
    <!-- class配置类  name指类名  table指定表名  如果不写，默认类名为表名-->
    <class name="cn.sxt.pojo.User" table="t_user">
        <!-- id主键的配置 name配置类的属性名
             column数据库字段名 不写和属性名一致
             type 指定属性的类型
             length指定字段的长度
        -->
        <id name="id" column="id">
            <!-- 主键的生成策略
                increment 
                    用于为long,short或者int类型生成唯一标识。只有在没有其他进程往同一张表中插入数据时才能使用。在集群下不要使用。 
                identity 
                    对DB2,MySQL, MS SQL Server, Sybase和HypersonicSQL的内置标识字段提供支持。 返回的标识符是long, short 或者int类型的。 
                native -(如果是mysql自增，那么native和identity是一样)
                    根据底层数据库的能力选择identity, sequence 或者hilo中的一个。 
                sequence 
                    在DB2,PostgreSQL, Oracle, SAP DB, McKoi中使用序列（sequence)， 
                    而在Interbase中使用生成器(generator)。返回的标识符是long, short或者 int类型的。 
                <generator class="sequence">
                    <param name="sequence">user_seq</param>
                </generator>
                assigned 
                    让应用程序在save()之前为对象分配一个标示符。这是 <generator>元素没有指定时的默认生成策略。
             -->
            <generator class="identity">
            </generator>
        </id>
        <!-- property是配置类的属性  name指属性名 -->
        <property name="name" length="40"/>
        <property name="age" />
    </class>
</hibernate-mapping>

```

### 测试类
```java
public class Test {
    public static void main(String[] args) {
        //读取src下hibernate.cfg.xml 如果不为configure指明参数 默认读取hibernate.cfg.xml
        Configuration cfg = new Configuration().configure();
        //3.x版本不需要ServiceRegistry
        //4.0 ServiceRegistryBuilder
        //获取注册对象4.3的创建办法
        ServiceRegistry registry = new StandardServiceRegistryBuilder()
                                    .applySettings(cfg.getProperties())
                                    .build();
        //SessionFactory是一个重量级对象 session的工厂 生命周期是进程级别的 支持集群 线程安全的
        SessionFactory factory = cfg.buildSessionFactory(registry);
        /*Session (org.hibernate.Session) 
            表示应用程序与持久储存层之间交互操作的一个单线程对象，
            此对象生存期很短。 其隐藏了JDBC连接，也是Transaction的工厂。
             其会持有一个针对持久化对象的必选（第一级）缓存，在遍历对象图或者根据持久化标识查找对象时会用到
           session支持数据库操作
         * */
        Session session = null;
        //事务对象
        Transaction tx =null;
        try{
            session = factory.openSession();
            tx = session.beginTransaction();
            User u = new User("小红",22);
            //保存数据
            session.save(u);
            //提交事务
            tx.commit();
        }catch(Exception e){
            if(tx!=null)
            //回滚事务
            tx.rollback();
        }finally{
            if(session!=null)
                session.close();
        }
        factory.close();
    }
}
```
封装util
```java
public class HibernateUtil {
    private static Configuration cfg=null;
    private static ServiceRegistry registry=null;
    private static SessionFactory factory=null;
    static{
        cfg = new Configuration().configure();
        registry = new StandardServiceRegistryBuilder()
                    .applySettings(cfg.getProperties())
                    .build();
        factory = cfg.buildSessionFactory(registry);
    }
    public static Session getSession(){
        return factory.openSession();
    }
}

```

## 对象生命周期

### 对象的 3 种状态： 

1. 瞬时状态（Transient）：
    - 使用new操作符初始化的对象就是瞬时状态，他们没有任何和数据库表相关联的行为，只要应用不在引用这些对象（不在被其他对象所引用），他们的状态将会丢失，并由垃圾回收机制回收。

2. 持久化状态（Persist）：
    - 持久化状态的对象是具有数据库标识的实例，由持久化管理器Session统一管理；
    - 持久化状态的实例是在事务中进行操作，在事务结束时同数据库进行同步。当事务提交时，通过执行SQL的insert，update和delete语句把内存中的状态同步到数据库中。

3. 游离状态（detached）：
    - Session关闭之后，持久化状态的对象就变为离线状态。
    - 离线状态表示，这个对象不能再与数据库保持同步，他们不再受Hibernate的Session管理器管理。

- Transient 临时状态/瞬时状态 该对象是新创建的；一个持久化状态的对象被删除；一个游离状态的数据被删除 
- Persist 持久化状态 对象从数据库中查询出来时，临时状态的数据被保存时，游离状态的数据被更新/ 锁定 
- Detach 游离状态 持久化状态的数据被（session）清理 

![hibernate-cycle](/images/java/hibernate-cycle.png)

### 状态转移

- 瞬时状态转为持久状态
    - 使用Session对象的save()或saveOrUpdate()方法保存对象后，该对象的状态由瞬时状态转换为持久状态。
        - save直接添加数据发出一条insert语句
        - saveOrUpdate 判断保存的对象是否有id,如果有则发出update语句，如果没有则发出insert语句
    - 使用Session对象的get()或load()方法获取对象，该对象的状态是持久状态。
        - get 查询数据如果数据不存在则返回null
        - load查询数据如果数据不存在则抛出异常
- 持久状态转为瞬时状态
    - 执行Session对象的delete()方法后，对象由原来的持久状态变为瞬时状态，因为此时该对象没有与任何的数据库数据关联。
- 持久状态转为游离状态
    - 执行了Session对象的evict()、clear()或close()方法，对象由原来的持久状态转为游离状态。
        - evict 把某个对象从session中移除(Persistent->Detached)
        - clear 把所有对象从session中移除
        - close 关闭session，其中的对象也全部被移除了。
- 游离状态转为持久状态
    - 重新获取Session对象，执行Session对象的update()或saveOrUpdate()方法，对象由游离状态转为持久状态，该对象再次与Session对象相关联。
- 游离状态转为瞬时状态
　　- 执行Session对象的delete()方法，对象由游离状态转为瞬时状态。
　　- 处于瞬时状态或游离状态的对象不再被其他对象引用时，会被Java虚拟机按照垃圾回收机制处理。



status | Mem | Session | DB
|-|-|-|-|
Transient |Y|N|N|
Persistent|Y|Y|Y|
Detached|Y|N|Y|

## 映射

### 关联关系映射

#### 单向多对一
User Role
```xml
<hibernate-mapping>
    <class name="User" table="t_user">
        <id name="id" column="id"><generator class="identity"></generator></id>
        <!-- property是配置类的属性  name指属性名 -->
        <property name="name" length="40"/>
        <property name="age" />
        <many-to-one name="role" column="roleId" not-null="true"></many-to-one>
    </class>
</hibernate-mapping>

```

### 单向一对多
Role User
```xml
<hibernate-mapping>
    <class name="Role" table="t_role">
        <id name="id" column="id"><generator class="identity"></generator></id>
        <property name="name" length="40"/>
        <set name="users">
            <!-- 配置外键 -->
            <key column="roleId" not-null="true"></key><one-to-many class="User"/>
        </set>
    </class>
</hibernate-mapping>

```

### 双向一对多
User Role
```xml
        <!-- User.hbm.xml -->
        <many-to-one name="role" column="roleId"/>

        <!-- Role.hbm.xml -->
        <set name="users" inverse="true">
            <!-- 配置外键 -->
            <key column="roleId"></key>
            <one-to-many class="User"/>
        </set>
```

### 基于外键单向一对一
User IdCard
- 外键不重复的单向多对一
```xml
<hibernate-mapping>
    <class name="User" table="t_user">
        <id name="id" column="id">
            <generator class="identity">
            </generator>
        </id>
        <property name="name" length="40"/>
        <property name="age" />
        <many-to-one name="idCard" column="cardId" unique="true"/>
    </class>
</hibernate-mapping>
```

### 基于外键双向一对一
User IdCard
```xml
<!-- User.hbm.xml -->
<hibernate-mapping>
    <class name="cn.sxt.pojo.User" table="t_user">
        <id name="id" column="id">
            <generator class="identity">
            </generator>
        </id>
        <property name="name" length="40"/>
        <property name="age" />
        <many-to-one name="idCard" column="cardId" unique="true"/>
    </class>
</hibernate-mapping>

<!-- IdCard.hbm.xml -->
<hibernate-mapping>
    <class name="cn.sxt.pojo.IdCard" table="t_idCard">
        <id name="id" column="id">
            <generator class="assigned">
            </generator>
        </id>
        <property name="address"/>
        <one-to-one name="user" property-ref="idCard"/>
    </class>
</hibernate-mapping>

```

### 基于主键单向一对一
User IdCard
```xml
<hibernate-mapping>
    <class name="User" table="t_user">
        <id name="id" column="id">
            <generator class="foreign">
                <param name="property">idCard</param>
            </generator>
        </id>
        <property name="name" length="40"/>
        <property name="age" />
        <!-- 基于主键的一对一，constrained为true将添加外键约束 -->
        <one-to-one name="idCard" constrained="true"></one-to-one>
    </class>
</hibernate-mapping>

```


### 基于主键双向一对一
User IdCard
```xml
<!-- User.hbm.xml -->
<hibernate-mapping>
    <class name="User" table="t_user">
        <id name="id" column="id">
            <generator class="foreign"><param name="property">idCard</param></generator>
        </id>
        <property name="name" length="40"/>
        <property name="age" />
        <one-to-one name="idCard" constrained="true"></one-to-one>
    </class>
</hibernate-mapping>

<!-- IdCard.hbm.xml -->
<hibernate-mapping>
    <class name="IdCard" table="t_idCard">
        <id name="id" column="id">
            <generator class="assigned"></generator>
        </id>
        <property name="address"/>
        <one-to-one name="user"/>
    </class>
</hibernate-mapping>
```

### 单向多对多
Role Func
```xml
<hibernate-mapping>
    <class name="Role" table="t_role">
        <id name="id" column="id">
            <generator class="native"></generator>
        </id>
        <property name="name"/>
        <set name="funcs" table="t_role_func">
            <!-- 是当前类在关联表中的外键列名 -->
            <key column="rid"></key>
            <!-- 当前类所对应的类 在关联表中的外键列名 -->
            <many-to-many column="fid" class="Func"/>
        </set>
    </class>
</hibernate-mapping>

```

### 双向多对多
Role Func
```xml
<!-- Role.hbm.xml -->
<hibernate-mapping>
    <class name="Role" table="t_role">
        <id name="id" column="id">
            <generator class="native"></generator>
        </id>
        <property name="name"/>
        <set name="funcs" table="t_role_func">
            <!-- 是当前类在关联表中的外键列名 -->
            <key column="rid"></key>
            <!-- 当前类所对应的类 在关联表中的外键列名 -->
            <many-to-many column="fid" class="Func"/>
        </set>
    </class>
</hibernate-mapping>
<!-- Runc.hbm.xml -->
<hibernate-mapping>
    <class name="Func" table="t_func">
        <id name="id" column="id">
            <generator class="native">
            </generator>
        </id>
        <property name="name"/>
        <property name="url"/>
        <set name="roles" inverse="true"  table="t_role_func">
            <key column="fid"></key>
            <many-to-many column="rid" class="Role"/>
        </set>
    </class>
</hibernate-mapping>
```

#### cascade级联
cascade级联操作：表示当操作一个对象时，是否级联操作与之关联的对象。 如果 cascade 的值不为 none 时，操作对象需要关联对象的数据时，会先操作关联对象。
- 在many-to-one 的映射中
    - 使用 cascade='save-update'将会检查关联对象，会将关联对象保存或更新
    - 不建议使用 cascade='delete'会将级联关联对象删除，若多个记录关联到同一关联对象会出错
- 在 one-to-many 的映射中中
    - 使用 cascade='save-update'将会多出更新sql语句;效率比较低，不建议使用。 
    - 使用 cascade='delete'将会级联删除多的一端的数据，一般情况上下不会设置，大多为虚拟删除
- none 默认值  不级联
- save-update 保存更新时级联
- delete 删除时级联
- all 所有动作都级联 

#### inverse
inverse表示控制关系(外键)由谁来管理,多的一方和一的一方都是默认管理关系的，但一的一方可以设置inverse
- Inverse="true"表示由关联关系的另一端来管理关系;
- 由一的一方控制关系，那么管理关系时必然会发出update语句，但发出update语句不一定是一的一方管理关系
inverse 管理的是关联关系，cascade 管理的级联关系。建议管理关联关系由多的一端来关联。

### 组件映射

### 组合主键映射



## OpenSessionInView

## 加载和抓取

加载策略
- 即时加载：指当查询数据时直接将该数据从数据库中查询（get）
- 延迟加载（懒加载）：当真正使用数据时，才从数据库中查询数据（load） 

lazy 属性---指定是否启动懒加载 
- 对应 class 来说，默认是懒加载的。 
- Property 的 lazy 默认是即时加载的。当属性是大对象的时候使用延迟加载。 
- Many-to-one:false--不使用懒加载;当取多的一端数据时会将一的取出来 
- no-proxy:属性应该在实例变量第一次被访问时应该延迟抓取（fetchelazily） （需要运行时字节码的增强）。
    - Proxy（默认）:使用代理来实现懒加载 Set 的 lazy:true(默认) 使用懒加载 False:不使用懒加载
    - extra:一种比较聪明的懒加载策略，即调用集合的 size/contains 等方法的时候，hibernate并不会去加载整个集合的数据，而是发出一条聪明的SQL语句，以便获得需要的值，只有在真正需要用到这些集合元素对象数据的时候，才去发出查询语句加载所有对象的数据 

抓取策略：select/join 
- Select 抓取：表示查询关联对象时，使用 select 语句。（在 select 抓取下，可以用 lazy）
- Join 抓取：查询关联对象时，使用 join 连接语句。Lazy 没有意义（没有作用）。 
- subselect 抓取：如果要查询关联集合的内容。会查询之前已经查出的对象的所有关联集 合。(Category 对应了多个 Book)如果查询了（”文学”,”历史”）；那么在使用（lazy=true）”
文学”或”历史”的集合对象（”所对应的书籍信息”） .会将(“文学”和”历史”)的书籍信息一起查询。如果lazy=false；在查询“多个分类时”会将所有分类的书籍信息一起查询。


## HQL
- HQL：Hibernate Query Language；
    - hibernate 查询语言
    - 面向对象的查询
    - 对关键字大小写不敏感 

## Criteria

## 缓存

缓存分类：
- 一级缓存：又称为Session缓存，由Session来管理；和Session的生命周期相同。是事务级别的缓存（线程）
- 二级缓存：SessionFactory缓存；和SessionFactory生命周期相同；是进程级别的缓存；使用第三方提供的二级缓存插件
- 查询缓存：在二级缓存的基础上，使用查询缓存

- get 方法去查询数据时，首先检查缓存中是否有该数据。如果缓存中有该数据，直接取缓存中的数据。如果缓存中没有，则查询数据库；并且将该数据写入 Session 缓存中。 
- load 方法：查询数据，首先检查是否必须查询对象的属性。如果需要查询，那么检查缓 存中是否有，如果缓存中有该对象，则取缓存中的数据。如果缓存中没有，那么取数据中数 据；并且将该数据写入缓存；
- list 方法：查询数据，不会检查 Session 缓存中是否有数据，当从数据库中查询数据以后， 会将对象写入 Session 缓存
- iterate 方法首先会查询所有id,根据id查询数据时，是一种延迟加载方式；首先检查缓存是否有该对象，如果有，则取缓存中的数据，如果没有则到数据库中查询，并且将查询结果写入 Session 缓存中

- n+1 和 1 的问题：iterate()方法--首先查询所有的Id，当使用对象时，再根据id到数据库中查询该对象。所以如果查询所有数据，那么将执行n+1条sql语句。而list查询所有数据只执行1条sql语句。因此，当查询所有对象时应该使用list,而如果查询的是部分数据，那么使用iterate 

一级缓存的管理 
- clear 方法：清空缓存中的所有数据；清空以后当再次查询数据时会从数据库中重新查询 
- flush 方法：将缓存中的数据与数据库中的数据同步
- evict 方法:清除缓存中指定的对象
- close 方法：关闭 session 对象，释放缓存

## 事务隔离级别

## 锁机制

