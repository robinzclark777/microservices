

# Introduction

This document will cover:

-   Some of the basic concepts of Spring and their purpose.
-   The division between Spring Framework and Spring Boot.
-   An example application for storing/retrieving Todo notes.

Hopefully this can help to demystify some of the &ldquo;Spring Magic&rdquo;


# Spring Framework

**Spring makes it easier for developers to follow the SOLID principles**
**by eliminating much of the boilerplate code that would otherwise be**
**required.**


## SOLID

-   Single Responsibility: A class should have one and only one reason to change,
    meaning that a class should have only one job.

-   Open/Closed: Objects or entities should be open for extension but closed for
    modification.

-   Liskov Substitution: Let q(x) be a property provable about objects of x of
    type T. Then q(y) should be provable for objects y of type S where S is a
    subtype of T.

-   Interface Segregation: A client should never be forced to implement an
    interface that it doesn’t use, or clients shouldn’t be forced to depend on
    methods they do not use.

-   Dependency Inversion: Entities must depend on abstractions, not on
    concretions. It states that the high-level module must not depend on the
    low-level module, but they should depend on abstractions.

These principles have proven to help create services that are robust and
adaptable. They make our code more transparent while maintaining high levels of
comprehensibility.


## Spring Application Context

-   The Spring `ApplicationContext` is a map-like structure that associates Spring
    beans to their names. Spring provides implementations of the
    `ApplicationContext` interface for several different use-cases, often
    depending on how the beans will be defined.
    
    Examples:
    
    -   `AnnotationConfigApplicationContext`
    -   `XmlWebApplicationContext`
    -   `GenericWebApplicationContext`


## Spring Beans

-   When you create a bean using `@Bean`, `@Service`, `@Component`, etc., Spring&rsquo;s
    component scanning mechanism automatically loads that bean into the
    `ApplicationContext`.

-   Beans are **not** the object that you created using Java&rsquo;s `new` operator. Spring
    creates a proxy that wraps that object and that Proxy is loaded into the
    `ApplicationContext`. Future calls to that method will return the proxy-object
    (bean) from the `ApplicationContext`. By default, beans are singletons. This can
    be overridden using `@Scope("prototype")`.

**Caution:** Prototype beans are sometimes counter-intuitive. Making a bean a
prototype **does not** automatically make it thread-safe.


### Singleton/Prototype Configuration Class

    
    @Configuration
    public class BeanConfiguration {
        @Bean
        public Object singleton() {
            return new Object();
        }
    
        @Bean
        @Scope("prototype")
        public Object prototype() {
            return new Object();
        }
    }


### Singleton/Prototype Application

This example bends the Spring dependency injection abstraction by manipulating
the `ApplicationContext` directly, but it works for demonstration
purposes. Ideally we would stay within the high-level Spring concepts and
break them only when it&rsquo;s truly necessary.

    @SpringBootApplication
    public class BeansApplication {
        public static void main(String[] args) {
            ApplicationContext context = SpringApplication.run(BeansApplication.class);
    
            // @Configuration classes are also beans.
            // Let's pull ours from the ApplicationContext:
            BeanConfiguration beanConfiguration = context.getBean(BeanConfiguration.class);
    
            System.out.println("first call to singleton(): " + beanConfiguration.singleton());
            System.out.println("second call to singleton(): " + beanConfiguration.singleton());
            System.out.println("first call to prototype(): " + beanConfiguration.prototype());
            System.out.println("second call to prototype(): " + beanConfiguration.prototype());
        }
    }


### Singleton/Prototype Execution

    java -jar ~/Public/scripts/beans-demo-0.0.1-SNAPSHOT.jar


## Annotations

-   This is an incomplete list of annotations will create a bean:
    -   `@Service`
    -   `@Component`
    -   `@Configuration`
    -   `@Bean`

-   This is an incomplete list of annotations that apply conditions to bean
    creation:
    -   `@ConditionalOnProperty`
    -   `@ConditionalOnPropertyMissing`
    -   `@ConditionalOnBean`
    -   `@ConditionalOnExpression`


## Annotation Processing

An annotation processor is a class that takes some action based on annotations
included in our source code. While they often run at compile time, Spring&rsquo;s
annotation processor run-time. It&rsquo;s this annotation processor that parses the
source code looking for services, components, beans, etc. and acting on them
when it discovers them.


## Why Proxies?

-   So, why are Spring beans proxies? It gives Spring Framework the ability to
    handle &ldquo;cross-cutting&rdquo; concerns such as Security, Metrics collection and
    Caching.
    -   Spring security can restrict access to beans or bean-methods for role-based
        access control (RBAC).
    -   The Spring Actuator can collect metrics such as the time spent in each
        method and the number of times a method was called.


# Spring Projects

The `Spring Framework` is the basis for many other downstream projects that are
maintained by the `Spring` team. You can see these at <https://spring.io/projects>.
These projects are often wrappers around underlying libraries, making those
libraries much easier to use.


# Spring Boot

`Spring Boot` is nothing more than a set of &ldquo;opinions&rdquo; that leverage the `Spring
Framework`. It includes these opinions through the use of pre-defined, sensible
default configuration values and &ldquo;starter POMs&rdquo;.


## Configuration values

You can find a non-exhaustive list of properties that `Spring Boot` supports here:
<https://docs.spring.io/spring-boot/docs/current/reference/html/application-properties.html>.
Which properties are supported is dependent upon which `Spring Boot` modules are
included in the project.


## Starter POMs

A starter POM is simply a `pom.xml` file that contains a group of dependencies.
When the starter POM is included in your project, it will typically inherit many
`.jar` artifacts. Note that starter POMs can also include other starter POMs.
Here&rsquo;s an example of the dependencies from the `spring-boot-web` starter POM:

    <dependency>
      <groupId>org.springframework.boot</groupId>
      <artifactId>spring-boot-starter</artifactId>
      <version>2.7.2</version>
      <scope>compile</scope>
    </dependency>
    <dependency>
      <groupId>org.springframework.boot</groupId>
      <artifactId>spring-boot-starter-json</artifactId>
      <version>2.7.2</version>
      <scope>compile</scope>
    </dependency>
    <dependency>
      <groupId>org.springframework.boot</groupId>
      <artifactId>spring-boot-starter-tomcat</artifactId>
      <version>2.7.2</version>
      <scope>compile</scope>
    </dependency>
    <dependency>
      <groupId>org.springframework</groupId>
      <artifactId>spring-web</artifactId>
      <version>5.3.22</version>
      <scope>compile</scope>
    </dependency>
    <dependency>
      <groupId>org.springframework</groupId>
      <artifactId>spring-webmvc</artifactId>
      <version>5.3.22</version>
      <scope>compile</scope>
    </dependency>

This approach means that the application developer doesn&rsquo;t have to concern
themselves as much with managing dependencies - the Spring team has already done
that work for them.


## Spring Initializr

A typical development scenario will include steps something like:

1.  Visit <https://start.spring.io> which is a web-interface to create the
    scaffolding (directories, `pom.xml`, etc) for your `Spring Boot` project. This UI
    allows you to select which `Spring` projects to include in your app.
2.  Once you&rsquo;ve downloaded and unzipped the project (I typically just copy/paste
    the `pom.xml`), you can build it and execute the resulting `.jar` file. `Spring
       Boot` will start, but do nothing.
3.  Add custom `@Bean`, `@Configuration`, `@Component`, etc. classes to the project.
4.  Create an `application.properties`, `application.yaml` or some other property
    source to configure any of the include `Spring` projects.

