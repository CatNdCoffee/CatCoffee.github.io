# Maven结合SpringBoot

## Spring Boot是什么？

首先Spring Boot不是一个框架，它是一种用来轻松创建具有最小或零配置的独立应用程序的方式。这是方法用来开发基于Spring的应用，但只需非常少的配置。它提供了默认的代码和注释配置，快速启动新的Spring项目而不需要太多时间。它利用现有的Spring项目以及第三方项目来开发生产就绪(投入生产)的应用程序。它提供了一组Starter Pom或gradle构建文件，可以使用它们添加所需的依赖项，并且还便于自动配置。

Spring Boot的主要目标是：

    为所有Spring开发提供一个基本的，更快，更广泛的入门体验。

    开箱即用，但随着需求开始偏离默认值，快速启动。

    提供大型项目(例如嵌入式服务器，安全性，度量，运行状况检查，外部化配置)常见的一系列非功能特性。

    绝对没有代码生成以及不需要XML配置，完全避免XML配置。

    为了避免定义更多的注释配置(它将一些现有的 Spring Framework 注释组合成一个简单的单一注释)

    避免编写大量import语句。

    提供一些默认值，以便在短时间内快速启动新项目。

未来的Spring项目不会有任何XML配置作为它的一部分，一切都将由项目Spring Boot处理。

## POM文件配置

Spring Boot 依赖的groupId是org.springframework.boot。通常情况下，您的MavenPOM文件继承自spring-boot-starter-parent项目，并且声明为一些“Starters”。Spring Boot也提供了一些创建可执行jar文件的Maven插件。

以下为一个典型pom.xml

    <?xml version="1.0" encoding="UTF-8"?>
    <project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
    xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>

    <groupId>com.example</groupId>
    <artifactId>myproject</artifactId>
    <version>0.0.1-SNAPSHOT</version>

    <parent>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-parent</artifactId>
        <version>1.4.1.BUILD-SNAPSHOT</version>
    </parent>

    <!-- Additional lines to be added here... -->

    <!-- (you don't need this if you are using a .RELEASE version) -->
    <repositories>
        <repository>
            <id>spring-snapshots</id>
            <url>http://repo.spring.io/snapshot</url>
            <snapshots><enabled>true</enabled></snapshots>
        </repository>
        <repository>
            <id>spring-milestones</id>
            <url>http://repo.spring.io/milestone</url>
        </repository>
    </repositories>
    <pluginRepositories>
        <pluginRepository>
            <id>spring-snapshots</id>
            <url>http://repo.spring.io/snapshot</url>
        </pluginRepository>
        <pluginRepository>
            <id>spring-milestones</id>
            <url>http://repo.spring.io/milestone</url>
        </pluginRepository>
    </pluginRepositories>
</project>

一个稍微详细一些的pom.xml

    <project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
    xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>

    <groupId>com.jege.spring.boot</groupId>
    <artifactId>spring-boot-hello-world</artifactId>
    <version>0.0.1-SNAPSHOT</version>
    <packaging>jar</packaging>

    <name>spring-boot-hello-world</name>
    <url>http://maven.apache.org</url>

    <!-- 公共spring-boot配置，下面依赖jar文件不用在写版本号 -->
    <parent>
        <groupId>org.springframework.boot</groupId>
        <!-- 自动包含以下信息： -->
        <!-- 1.使用Java6编译级别 -->
        <!-- 2.使UTF-8编码 -->
        <!-- 3.实现了通用的测试框架 (JUnit, Hamcrest, Mockito). -->
        <!-- 4.智能资源过滤 -->
        <!-- 5.智能的插件配置(exec plugin, surefire, Git commit ID, shade). -->
        <artifactId>spring-boot-starter-parent</artifactId>
        <!-- spring boot 1.x最后稳定版本 -->
        <version>1.4.1.RELEASE</version>
        <!-- 表示父模块pom的相对路径，这里没有值 -->
        <relativePath />
    </parent>

    <properties>
        <project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
        <java.version>1.8</java.version>
    </properties>

    <dependencies>

        <!-- web -->
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-web</artifactId>
        </dependency>

        <!-- 测试 -->
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-test</artifactId>
            <!-- 只在test测试里面运行 -->
            <scope>test</scope>
        </dependency>

    </dependencies>

    <build>
        <finalName>spring-boot-hello-world</finalName>
        <plugins>
            <!-- jdk编译插件 -->
            <plugin>
                <groupId>org.apache.maven.plugins</groupId>
                <artifactId>maven-compiler-plugin</artifactId>
                <configuration>
                    <source>${java.version}</source>
                    <target>${java.version}</target>
                </configuration>
            </plugin>
        </plugins>
    </build>

    </project>

## 一个简单程序配置实例

- 1.写一个controller

    package com.eclipsesv.HelloController;
    import org.springframework.stereotype.Controller;
    import org.springframework.web.bind.annotation.RequestMapping;

    @Controller
    public class Hello {
        @RequestMapping("/")
        public String index(){
            return "index";
        }
    }

使用@Controller和@RequestMapping配合使用就可以完成一个简单的路由映射，上边的这段代表如果访问url为”/“会返回名为”index”的视图，而这里的视图只需要写一个名为index的html 并把它放在/resources/templates中即可

- 2.写一个main函数

    package com.eclipsesv;
    import org.springframework.boot.SpringApplication;
    import org.springframework.boot.autoconfigure.SpringBootApplication;

    @SpringBootApplication
    public class Application {
        public static void main(String... arg){
            SpringApplication.run(Application.class,arg);
        }
    }

使用@SpringBootApplication可以完成项目的自动配置，它会在启动的时候找到项目中由@Controller标识的区域，并解析@RequestMapping完成自动路由映射。**注意,该配置生成的代码会被打成jar包**

应用程序的最后一部分是main方法。这只是Java规范的一个标准的应用程序入口方法。我们的main方法委托了SpringApplication类去调用run方法完成这个功能。SpringApplication会启动我们的应用程序，启动Spring去开启自配置的Tomcat Web服务器。我们需要把Example.class作为参数传送到run方法，告知SpringApplication这是一个主要是的Spring组件。args字符串数组也传入了，以处理命令行提供的参数。

## 添加classpath依赖

Spring Boot提供了一些“Starters”，让您方便把jar文件添加到您的classpath。我们的实例已经在POM文件的parent部分添加了
spring-boot-starter-parent。spring-boot-starter-parent是一个添加了一些默认Maven配置的特殊的starter。也添加了dependency-management部分，这个部分可以让您省掉某些依赖项的version标签。

其他的 “Starters”添加了在您开发其他特定类型应用程序的时候，您很可能需要添加的一些依赖项。我们目前正在开发Web应用，我们需要添加依赖项spring-boot-starter-web，不过我们先通过下面的方式看看目前的情况。

## 注解使用

### @RestController和@RequestMapping注解

Example类的第一个注解是@RestController。这叫做构造型注解。它提示我们，对Spring来说，这个类扮演了特殊的角色，现在，这个类是一个Web控制器，Spring将会使用它来处理Web请求。

@RequestMapping注解展示了 “routing”信息。它告知Spring路径为“/”的HTTP请求都会映射到home方法。@RestController告知Spring直接将结果字符串返回给请求方。

### @EnableAutoConfiguration注解

第二个类级别的注解是@EnableAutoConfiguration。 这个注解告知Spring Boot自己 “判断”您是如何配置Spring的，这基于您添加的依赖项。因为依赖项spring-boot-starter-web添加了Tomcat和Spring MVC，所以@EnableAutoConfiguration注解会判断您是在开发Web应用程序并且引入了Spring。

**Starters and Auto-Configuration**
Auto-configuration是为了和“Starters”一起良好工作而设计的，但是这两个部分并没有耦合在一起。您在starters的约束之外自由挑选jar依赖，而Spring Boot将会尽可能的自配置以适应您的应用程序。

### @SpringBootApplication

就是@SpringBootConfiguration+@EnableAutoConfiguration+@ComponentScan的组合，非常简单，使用也方便

### @SpringBootTest

Spring Boot版本1.4才出现的，具有Spring Boot支持的引导程序（例如，加载应用程序、属性，为我们提供Spring Boot的所有精华部分）
可以自动导入测试需要的类

### @ConfigurationProperties

Spring Boot @ConfigurationProperties是让开发人员比较容易地将整个文件映射成一个对象。

- 简单属性文件:@PropertySource("classpath: xxx.property") + @Value(${...})
- 复杂属性文件:

### devtools模块

devtools模块，是为开发者服务的一个模块。主要的功能就是代码修改后一般在5秒之内就会自动重新加载至服务器，相当于restart成功。

devtools可以实现页面热部署（即页面修改后会立即生效，这个可以直接在application.properties文件中配置spring.thymeleaf.cache=false来实现)
实现类文件热部署（类文件修改后不会立即生效），实现对属性文件的热部署
即devtools会监听classpath下的文件变动，并且会立即重启应用（发生在保存时机），注意：因为其采用的虚拟机机制，该项重启是很快的

配置文件application.properties:

    添加那个目录的文件需要restart
    spring.devtools.restart.additional-paths=src/main/java
    排除那个目录的文件不需要restart
    spring.devtools.restart.exclude=static/**,public/**

### main application class

`application.java`中定义main方法及基本的`@Configuration`定义
通常将application.java置于根目录下,示例工程结构如下:

    com
    +- example
        +- myproject
            +- Application.java
            |
            +- domain
            |   +- Customer.java
            |   +- CustomerRepository.java
            |
            +- service
            |   +- CustomerService.java
            |
            +- web
                +- CustomerController.java

示例appliciotn.java如下:

    package com.example.myproject;

    import org.springframework.boot.SpringApplication;
    import org.springframework.boot.autoconfigure.EnableAutoConfiguration;
    import org.springframework.context.annotation.ComponentScan;
    import org.springframework.context.annotation.Configuration;

    @Configuration
    @EnableAutoConfiguration
    @ComponentScan
    public class Application {

        public static void main(String[] args) {
            SpringApplication.run(Application.class, args);
        }

    }

### Configuration Classes

SpringBoot更推荐使用基于Java代码的配置.通常将配置置于定义main方法的类中.
同时,可以使用`@Import`注解来导入其他位置的使用`@Configuration`配置文件,或使用`@ComponentScan`来自动检索Spring定义的包含`@Configuration`的Components.
或者也可以使用`@ImportResource`来导入xml配置

## 在SpringBoot中自定义日志配置

在根目录下配置相关日志libraries,并提合适的配置文件即可
由于日志是先于ApplicationContext(@Configuration文件),因此不要在@Configuration中配置日志

## SpringBoot与SpringMVC整合

