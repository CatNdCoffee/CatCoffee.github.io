# SpringBoot

## SpringInitializr

在网站Spring Initializr上填对应的表单，描述Spring Boot项目的主要信息，例如Project Metadata、Dependency等。在Project Dependencies区域，你可以根据应用程序的功能需要选择相应的starter。

Spring Boot starters可以简化Spring项目的库依赖管理，将某一特定功能所需要的依赖库都整合在一起，就形成一个starter，例如：连接数据库、springmvc、spring测试框架等等。简单来说，spring boot使得你的pom文件从此变得很清爽且易于管理。

常用的starter以及用处可以列举如下：

    - spring-boot-starter: 这是核心Spring Boot starter，提供了大部分基础功能，其他starter都依赖于它，因此没有必要显式定义它。
    - spring-boot-starter-actuator：主要提供监控、管理和审查应用程序的功能。
    - spring-boot-starter-jdbc：该starter提供对JDBC操作的支持，包括连接数据库、操作数据库，以及管理数据库连接等等。
    - spring-boot-starter-data-jpa：JPA starter提供使用Java Persistence API(例如Hibernate等)的依赖库。
    - spring-boot-starter-data-*：提供对MongoDB、Data-Rest或者Solr的支持。
    - spring-boot-starter-security：提供所有Spring-security的依赖库。
    - spring-boot-starter-test：这个starter包括了spring-test依赖以及其他测试框架，例如JUnit和Mockito等等。
    - spring-boot-starter-web：该starter包括web应用程序的依赖库。

## Spring Boot的自动配置

在Spring Boot项目中，xxxApplication.java会作为应用程序的入口，负责程序启动以及一些基础性的工作。@SpringBootApplication是这个注解是该应用程序入口的标志，然后有熟悉的main函数，通过SpringApplication.run(xxxApplication.class, args)来运行Spring Boot应用。打开SpringBootApplication注解可以发现，它是由其他几个类组合而成的：@Configuration（等同于spring中的xml配置文件，使用Java文件做配置可以检查类型安全）、@EnableAutoConfiguration（自动配置，稍后细讲）、@ComponentScan（组件扫描，大家非常熟悉的，可以自动发现和装配一些Bean）。

## Restful风格的SpringBoot配置

@RestController:功能基本等于@Controller,@RestController注解是@Controller和@ResponseBody的合集.表示这是个控制器bean，并且是将函数的返回值直接填入HTTP响应体中，是REST风格的控制器

@RepositoryRestResource注解让编程人员可以直接通过repository提供数据接口，在这个“前端负责V和C，后端负责提供数据”的时代，非常方便；并且，可以通过给该注解传入参数来改变URL