# Sturts2概览

## Struts2原理

核心点:
1.拦截器 interceptor
2.Action
3.ognl与valueStack

执行流程:
1.当通过浏览器发送一个请求
2.会被StrutsPrepareAndExecuteFilter拦截
3.会调用strtus2框架默认的拦截器(interceptor)完成部分功能
4.在执行Action中操作
5.根据Action中方法的执行结果来选择来跳转页面Resutl视图

一般管StrutsPrepareAndExecuteFilter 叫做前端控制器(核心控制器)，只有配置了这个filter我们的strtus2框架才能使用。
Strtus2的默认拦截器(interceptor)它们是在struts-default.xml文件中配置
注意:这上xml文件是在strtus-core.jar包中。
默认的拦截器是在defaultStack中定义的。

## Sturts2配置文件加载顺序

- 1.default.properties:位于Struts2-core.jar中,主要声明struts2框架的常量
- 2.Struts-default.xml:位于struts2-core.jar中,声明了interceptor/bean/result
- Struts-plugin.xml:位于struts2插件包中,主要用于声明配置
- 自定义struts.properties:位于自建工程src下,用于定制常量
- 自定义配置
- web.xml
- bean配置

## Action类的创建方式

在Action接口中定义了五个常量，一个execute方法
五个常量:它们是默认的五个结果视图<result name=””>:
ERROR : 错误视图
INPUT: 它是struts2框架中interceptor中发现问题后会访问的一个视图
LOGIN:它是一个登录视图，可以在权限操作中使用
NONE:它代表的是null,什么都不做（也不会做跳转操作）
SUCCESS:这是一个成功视图

## Struts2数据封装

主要有两种方式:
1.属性驱动
a.直接在action类中提供与请求参数匹配属性，提供get/set方法
b.在action类中创始一个javaBean,对其提供get/set ，在请求时页面上要进行修改
例如 user.username  user.password ,要使用ognl表达式
以上两种方式的优缺点:
第一种比较简单，在实际操作我们需要将action的属性在赋值给模型(javaBean)去操作
第二种:不需要在直接将值给javaBean过程，因为直接将数据封装到了javaBean中。它要求在页面上必须使用ognl表达式，就存在页面不通用问题。

2.模型驱动
步骤:
1.让Action类要实现一个指定接口ModelDriven
2.实例化模型对象(就是要new出来javaBean)
3.重写getModel方法将实例化的模型返回。

## 在Struts中获取ServletAPI

有两种方法:

- 1.静态类ServletActionContext:

    HttpServletRequest request = 
    ServletAcitonContext.getRequest();
    HttpServletResponse response =               ServletActionContext.getResponse();
    ServletContext servletContext = ServletActionContext.getServletContext();

- 2.采用注入, 让需要使用ServletAPI的action实现ServletRequestAware/ServletResponseAware/ServletContextAwarek来获取HttpServletRequest/HttpServletResponse/ServletContext

## OGNL表达式

OGNL是对象图导航语言(Object-Graph-Navigation-Language).它是一种功能强大的表达式语言，通过它简单一致的表达式语法，可以存取对象的任意属性，调用对象的方法，遍历整个对象的结构图，实现字段类型转化等功能。它使用相同的表达式去存取对象的属性.**Struts2框架内置OGNL**

### Struts框架中OGNL表达式的使用方法

在struts2框架中我们使用ognl表达式的作用是从valueStack中获取数据。
我们在struts2框架中可以使用ognl+valueStack达到在页面(jsp)上来获取相关的数据。
要想在jsp页面上使用ognl表达式，就需要结合struts2框架的标签
`<s:property value=”表达式”>`来使用

### 值栈

我们使用valueStack的主要目的是为我将我们action中产生的数据携带到页面上，也就是说valueStack它就是一个容器
在Struts2框架中将valueStack设计成一个接口。
com.opensymphony.xwork2.util.ValueStack
我们主要使用的是它的实现类
com.opensymphony.xwork2.ognl.OgnlValueStack。
当客户端向我们发送一个请求，服务器就会创始一个Action来处理请求，struts2中的action是一个多例，每一次请求都会有一个新的action对应。所以它不存在线程安全问题

***

在struts2框架中我们通过ognl表达式来获取valueStack中数据，没有使用#就会从CompoundRoot中获取数据，如果使用#来获取，这时就会从context中来获取

#### ApplicationContext

ActionContext它是action上下文，strtus2框架它使用actionContext来保存Action在执行过程中所需要的一些对象，例如 session, application… 
ActionContext的获取  是通过它的静态方法getContext()得到。
Struts2会根据每一次的http请求来创建对应的ActionContext,它是与当前线程绑定的。
每一次请求，就是一个线程，对应着一个request,每一次请求，会创建一个Action,每一个action对应一个ActionContext.每一次请求也对应着一个valueStack.
request---ActionContext----Action-----ValueStack它们都对应着一次请求(一个线程).
valueStack与ActionContext本质上是可以相互获取

ActionContext.getContext().getValueStack();

#### 值栈中的常用方法

Object peek():在不改变栈的情况下,获取栈顶对象
Object pop():获取栈顶对象,并且把它从栈顶移除
push(Object o):把对象推入值栈
Object findValue(String expr):通过用默认的搜索顺序对堆栈计算给定的表达式找到一个值
void set(String key, Object o):如果一个对象被 findValue(key,...) 检索，则在堆栈上使用给定的键设置它
int size():获取堆栈中对象的数量

#### 手动向valueStack存储数据

valueStack.push("itcast"); 存储到root中
valueStack.set("username", "tom");将参数封装到一个hashMap中,存入root

#### 自动向valueStack存储数据

每次请求,Struts会将实现ModelDriven的action下的model(getModel)存入值栈中

#### EL表达式从valueStack中获取数据

Struts2框架对request进行了增强，重写了getAttribute方法，如果在request域中查找不到数据，就会在valueStack中获取

#### OGNL表达式中的特殊字符

`#`:结合<s:>标签,从非root中获取数据
    <s:property value="#request.itcast">
`%`:用于选择是否强制解析OGNL表达式
    <s:property value="%{#reqeust.itcast}"/> 会强制解析成ognl
    <s:property value="%{`#request.itcast`}"> 不会强制解析成ognl
`$`:主要是在配置文件中获取valueStack的数据

#### 一些标签

    <s:iterator value="ps" var="p">
        <tr>
            <td><s:property value="#p.name" /></td>
            <td><s:property value="#p.price" /></td>
            <td><s:property value="p.count" /></td>
        </tr>

## Strut2框架下的Ajax开发

需要使用Json工具:Fastjson
String json = JSONObject.toJSONString(user); //对`User`对象
String json = JSONArray.toJSONString(users); //对`List<User>`]

## 注解开发

`<package name=”” namespace=””  extedns=”>`
`<action name=”” class=”” method>`
`<result name=”” type=””>`

    @Namespace来代替<package  namespace=””>
    @ParentPackage来代替<package extends=””>
    @Action来描述关于<action>配置
    value属性<action name=””>
    使用@Action的results来描述关于结果类型的配置<result>
    <result name=”” type=””>
    @Action(results={@Result(name=””,type=””,location=””)})
    