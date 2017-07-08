# Spring Data JPA

## JPA, Hibernate与Spring data JPA

JPA是一种规范,目的是整合第三方ORM框架.JPA几乎都是接口.而Hibernate就是JPA的一种实现.
Spring整合第三方框架的能力又很强，他要做的不仅仅是个最早的IOC容器这么简单一回事，现在Spring涉及的方面太广，主要是体现在和第三方工具的整合上。而在与第三方整合这方面，Spring做了持久化这一块的工作，我个人的感觉是Spring希望把持久化这块内容也拿下。于是就有了Spring-data-**这一系列包。包括，Spring-data-jpa,Spring-data-template,Spring-data-mongodb,Spring-data-redis，还有个民间产品，mybatis-spring，和前面类似，这是和mybatis整合的第三方包，这些都是干的持久化工具干的事儿。