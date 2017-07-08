# Hibernate概览

## This is Hibernate

### What is Hibernate

Hibernate是一个开放源代码的_对象关系映射框架_，它对JDBC进行了非常_轻量级_的对象封装，它将POJO与数据库表建立映射关系，是一个全自动的orm框架，hibernate可以自动生成SQL语句，自动执行，使得Java程序员可以随心所欲的使用对象编程思维来操纵数据库。 Hibernate可以应用在任何使用JDBC的场合，既可以在Java的客户端程序使用，也可以在Servlet/JSP的Web应用中使用。

**ORM**:Objecting Relation Mapping,对象关系映射

### Why choose Hibernate

- Hibernate对JDBC访问数据库的代码做了封装,简化了数据访问层繁琐的重复性代码
- Hibernate是一个基于JDBC主流持久化框架,是一个优秀的ORM实现,_支持很多关系型数据库_很大程度上简化了DAO层编码工作

## Hibernate基本用法

### 创建映射文件

映射配置文件主要用于描述实体类与数据表间的映射关系,需要放置在与实体类同一个包下,且取名为`_className_.hbm.xml`

例:

    <hibernate-mapping>
        <class name="_classReference_" table="_tableName_">
            <id name="id" column="id">
                <generator class="native" ></generator>
            </id>
            <property name=""__" column="__" length=""></property>
        </class>
    </hibernate-mapping>

### 创建核心配置文件

Hibernate核心配置文件主要包含连接数据库和配置Hibernate的相关信息,需要放置于src下,且取名为`hibernate.cfg.xml`

例:

    <!-- 此处省略DTD头 -->
    <hibernate-configuration>
        <session-factory>
            <property name="hibernate.connection.driver_class">
                com.mysql.jdbc.Driver
            </property>
            <property name="hibernate.connection.url">
            </operty>
            <property name="hibernate.connection.username">
            </property>
            <property name="hibernate.connection.password">
            </property>
        </session-factory>
    </hibernate-configuration>

### 查询API--Query 的使用

Query可以使用HQL或本地数据库SQL语言进行查询.

- 1.SQL语句的执行:`Query query = Session.createQuery("_HQL_")`;
- 2.分页查询

    Query query = session.createQuery("from Customer");
    query.setFirstResult(10);//开始位置
    query.setMaxResult(10);

    List`<Customer>` list = query.list();
    session.getTransaction().commit();//提交事务
    session.close();
- 指定列查询:

    Query query = session.createQuery("select new Customer(name, address) from Customer")
- 条件查询
    //无名称参数
    //Query qeury = session.createQuery("from Customer where name=?");
    //指定名称参数
    Query qeury = session.createQuery("from Customer where name=:myname");
    //无名称参数赋值
    //query.setParameter(0, "姓名0)
    //有名称参数赋值
    query.setParameter("myname" ,"姓名0")

### 条件查询API--Criteria 的使用

- 1.创建: session.createCriteria(_className_.class);
- 2.查询所有:

    List`<Customer>` list = criteria.list();
- 3.分页查询

   criteria.setFirstResult(firstResult);
   criteria.setMaxResult(maxResult);
- 4.多条件查询

    criteria.add(Restrictions.eq("address", "上海"));
    Customer c = (Customer) criteria.uniqueResult();

    //或查询
    criteria.add(Restricitons.or(Restrictions.eq("name", "姓名1"), Restrictions.eq("address", "上海")));

## Hibernate持久化对象

### 持久化类的三种状态

- 瞬时态:一般指刚new出来的对象,不存在持久化标识OID,与hibernate无关联,不在session管理范围
- 持久态:在hibernate session管理范围内,持有持久化标识OID,在事务未提交前一直是持久态;当它放生改变时,hibernate可以检测到;它可能存在或不存在与数据库中
- 托管态:也叫游离态,此时持久态对象与session失去关联,尽管它还存在OID;当托管态对象发生改变时,hibernate不能检测到;它可能存在或不存在于数据库中

三种状态可以相互转化

### 一级缓存与二级缓存

Hibernate中提供了两级Cache，第一级别的缓存是Session级别的缓存，它是属于事务范围的缓存。这一级别的缓存由hibernate管理，一般情况下无需进行干预；当通过Hibernate session API进行如save/get/uodate等操作时,持久化对象将保存到session中.*一级缓存的目的是减少对数据库的访问*

第二级别的缓存是SessionFactory级别的缓存，它是属于进程范围或群集范围的缓存。这一级别的缓存,可以进行配置和更改，并且可以动态加载和卸载。 Hibernate还为查询结果提供了一个查询缓存，它依赖于第二级缓存；

处于一级缓存中的对象永远不会过期，除非应用程序显式清空缓存或者清除特定的对象必须提供数据过期策略，如基于内存的缓存中的对象的最大数目，允许对象处于缓存中的最长时间，以及允许对象处于缓存中的最长空闲时间物理存储介质内存内存和硬盘。

用户可以在单个类或类的单个集合的粒度上配置第二级缓存。如果类的实例被经常读但很少被修改，就可以考虑使用第二级缓存。只有为某个类或集合配置了第二级缓存，Hibernate在运行时才会把它的实例加入到第二级缓存中。

#### 一级缓存常用API

session.clear():清空一级缓存
session.evict(c):清空一级缓存中指定的一个对象
session.refresh():重新查询数据库,用数据库中信息来更新一级缓存与快照

#### 二级缓存的配置

二级缓存插件:EhCache:可作为进程范围的缓存，存放数据的物理介质可以是内存或硬盘，对Hibernate的查询缓存提供了支持。

### 注解配置

- @Entity:声明一个实体
- @Table:描述类与表的对应
- @Id:声明一个主键
- @GenerateValue:声明一个主键生成策略
- @Column:定义列
- @Temporal:声明日期类型

一对多:

- @OneToMany:
- @ManyToOne:
- @JoinColumn:
- @Cascade(JPA注解):

多对一:

- @ManyToMany:在一端配置中间表,另一端使用mappedBy表示放置外键维护劝

### Hibernate五种检索方式

- 1.导航对象图检索方式:根据已加载的对象导航到其他对象
- 2.OID检索方式:按照对象OID检索对象
- 3.HQL检索方式:使用面向对象HQL查询语言
- 4.QBC检索方式:使用QBC(QueryByCriteria)API来检索对象,这种API封装了基于字符串形式的查询语句,提供了更加面向对象的查询接口
- 5.本地SQL检索方式:使用本地数据库的SQL查询语句

### Hibernate事务管理

一个事务与其他事务隔离的程度称为隔离级别. 数据库规定了多种事务隔离级别, 不同隔离级别对应不同的干扰程度, 隔离级别越高, 数据一致性就越好, 但并发性越弱  

Hibernate事务级别划分:

- READ_UNCOMMITED 读未提交.会引发所有隔离问题
- READ_COMMITTED 读已提交.阻止脏读,可能发生不可重复读与虚读(ORACLE默认级别)
- REPEATABLE_READ 重复读.阻止脏读和不可重复读.可能发生虚读.(MySQL默认级别)
- SERIALIZABLE 串行化.解决所有问题.不允许两个事物同时操作一个目标数据(效率低)

事务问题:

- 脏读: 对于两个事物 T1, T2, T1 读取了已经被 T2 更新但还没有被提交的字段. 之后, 若 T2 回滚, T1读取的内容就是临时且无效的.
- 不可重复读: 对于两个事物 T1, T2, T1 读取了一个字段, 然后 T2 更新了该字段. 之后, T1再次读取同一个字段, 值就不同了.
- 幻读: 对于两个事物 T1, T2, T1 从一个表中读取了一个字段, 然后 T2 在该表中插入了一些新的行. 之后, 如果 T1 再次读取同一个表, 就会多出几行.

### Hibernate检索策略

两种类别的检索:类级别检索;关联级别检索

什么是抓取策略?指的是查找到某个对象后,通过这个对象去查询关联对象的信息时的一种策略.
通过设置"fetch"和"lazy"来配置

Fetch可取值有:

- SELECT 多条简单的sql   （默认值）
- JOIN 采用迫切左外连接
- SUBSELECT 将生成子查询的SQL

lazy可取值有:

- TURE 延迟检索   (默认值)
- FALSE 立即检索
- EXTRA 加强延迟检索(及其懒惰)
