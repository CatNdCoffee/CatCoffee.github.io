# JPA, Hibernate与Spring data JPA

JPA是一种规范,目的是整合第三方ORM框架.JPA几乎都是接口.而Hibernate就是JPA的一种实现.
Spring整合第三方框架的能力又很强，他要做的不仅仅是个最早的IOC容器这么简单一回事，现在Spring涉及的方面太广，主要是体现在和第三方工具的整合上。而在与第三方整合这方面，Spring做了持久化这一块的工作，我个人的感觉是Spring希望把持久化这块内容也拿下。于是就有了Spring-data-**这一系列包。包括，Spring-data-jpa,Spring-data-template,Spring-data-mongodb,Spring-data-redis，还有个民间产品，mybatis-spring，和前面类似，这是和mybatis整合的第三方包，这些都是干的持久化工具干的事儿。

## 基本使用方法

在使用持久化工具的时候，一般都有一个对象来操作数据库，在原生的Hibernate中叫做Session，在MyBatis中叫做SqlSession,在JPA中叫做EntityManager.spring data jpa让我们解脱了DAO层的操作，基本上所有CRUD都可以依赖于它来实现

### SpringDtaJPA的预生成方法进行CRUD

- 继承JpaRepsitory(extends)
- 使用默认方法

### 自定义简单查询

自定义的简单查询就是根据方法名来自动生成SQL，主要的语法是findXXBy,readAXXBy,queryXXBy,countXXBy, getXXBy后面跟属性名称

    User findByUserName(String userName);

也使用一些加一些关键字And、 Or

    User findByUserNameOrEmail(String username, String email);

修改、删除、统计也是类似语法

    Long deleteById(Long id);

Long countByUserName(String userName)
基本上SQL体系中的关键词都可以使用，例如：LIKE、 IgnoreCase、 OrderBy。

    List<User> findByEmailLike(String email);

    User findByUserNameIgnoreCase(String userName);

    List<User> findByUserNameOrderByEmailDesc(String email);

### 自定义SQL查询

其实Spring data 觉大部分的SQL都可以根据方法名定义的方式来实现，但是由于某些原因我们想使用自定义的SQL来查询，spring data也是完美支持的；在SQL的查询方法上面使用@Query注解，如涉及到删除和修改在需要加上@Modifying.也可以根据需要添加 @Transactional 对事物的支持，查询超时的设置等

    @Modifying
    @Query("update User u set u.userName = ?1 where c.id = ?2")
    int modifyByIdAndUserId(String  userName, Long id);

    @Transactional
    @Modifying
    @Query("delete from User where id = ?1")
    void deleteByUserId(Long id);

    @Transactional(timeout = 10)
    @Query("select u from User u where u.emailAddress = ?1")
    User findByEmailAddress(String emailAddress);

### 分页功能

spring data jpa已经帮我们实现了分页的功能.Spring封装了一个分页实现类--Pageable.在查询的方法中，需要传入参数Pageable ,当查询中有多个参数的时候Pageable建议做为最后一个参数传入

    Pageable pageable = new PageRequest(page, size, sort)

    Page<User> findALL(Pageable pageable);

    Page<User> findByUserName(String userName,Pageable pageable);
