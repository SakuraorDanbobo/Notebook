---
title: [bbb]
tags: [aa]
categorys: [ssb]
---
# 1. 前言

Spring Boot框架本身并没有对工程结构有特别的要求，但是按照最佳实践的工程结构可以帮助我们减少可能会遇见的坑。

尤其是Spring包扫描机制的存在，如果您使用最佳实践的工程结构，可以免去不少特殊的配置工作。

# 2. 典型示例

以下结构是比较推荐的package组织方式：

~~~java
com
  +- example
    +- myproject
      +- Application.java
      |
      +- dao
      |  +- Customer.java
      |  +- CustomerDao.java
      |
      +- service
      |  +- CustomerService.java
      |
      +- controller
      |  +- CustomerController.java
      |
~~~

**root package：**`com.example.myproject`，所有的类和其他package都在root package之下。

**Application.java : **应用主类，该类直接位于`root package`下。通常我们会在应用主类中做一些框架配置扫描等配置，我们放在root package下可以帮助程序减少手工配置来加载到我们希望被Spring加载的内容。

**com.example.myproject.dao包：**用于定义实体映射关系与数据访问相关的接口和实现。

**com.example.myproject.service包：**用于编写业务逻辑相关的接口与实现。

**com.example.myproject.controller：**用于编写Web层相关的实现，比如：Spring MVC的Controller等

**默认情况下，Spring Boot的应用主类会自动扫描`root package`以及所有子包下的所有类来进行初始化。**

包的层级目录一定要搞对，如果把CustomerController放入到`com.example`下，则spring boot就无法扫描到CustomerController，并进行初始化了。

# 3. 非典型结构下的初始化

那么如果，我们一定要加载非`root package`下的内容怎么办呢？

比如有一些通用的工具类，比如第三方的包等等。

**方法一**：使用`@ComponentScan`注解指定具体的加载包，比如：

~~~java
@SpringBootApplication
@ComponentScan(basePackages="com.example")
public class HelloApp {

    public static void main(String[] args) {
        SpringApplication.run(HelloApp.class, args);
    }

}
~~~

这种方法通过注解直接指定要扫描的包，比较直观。

**方法二**：使用`@Bean`注解来初始化，比如：

~~~java
@SpringBootApplication
public class HelloApp {

    public static void main(String[] args) {
        SpringApplication.run(HelloApp.class, args);
    }

    @Bean
    public CustomerController customerController() {
        return new CustomerController();
    }

}
~~~

@Bean这种一般用于加载第三方的jar包，比如一些框架封装场景，比如Mybaits-plus等。

# 4. 工程结构之其他

这里讨论一下，除了controller，service，dao这种典型的package组织方式之外，还有没有别的？



~~~java
com
  +- example
    +- myproject
      +- Application.java
      |
      +- dao
        +- data
    	|  +- Customer.java
        +- mapper
    	|  +- CustomerMapper.java
      |
      +- domain
        +- repository
    	|  +- CustomerRepository.java
        +- CustomerDomain.java
      |
      +- service
      |  +- CustomerService.java
      |
      +- controller
      |  +- CustomerController.java
      |
~~~

上面的结构中，多出来一层domain层，将业务逻辑操作放入到domain中进行执行，Repository和数据层打交道，service用来管理业务流程。

