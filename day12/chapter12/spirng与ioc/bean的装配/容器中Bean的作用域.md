# 容器中 Bean 的作用域

---

当通过 Spring 容器创建一个 Bean 实例时，不仅可以完成 Bean 的实例化，还可以通过 scope 属性，为 Bean 指定特定的作用域。Spring 支持 5 种作用域。

* singleton：单态模式。即在整个 Spring 容器中，使用 singleton 定义的 Bean 将是单例的，只有一个实例。默认为单态的。
* prototype：原型模式。即每次使用 getBean 方法获取的同一个 `<bean />` 的实例都是一个新的实例。
* request：对于每次 HTTP 请求，都将会产生一个不同的 Bean 实例。
* session：对于每个不同的 HTTP session，都将产生一个不同的 Bean 实例。
* global session：每个全局的 HTTP session 对应一个 Bean 实例。典型情况下，仅在使用 portlet 集群时有效，多个 Web 应用共享一个 session。一般应用中，global-session 与 session 是等同的。

**注意：**

* 对于 scope 的值 request、session 与 global session，只有在 Web 应用中使用 Spring 时，该作用域才有效。
* 对于 scope 为 singleton 的单例模式，该 Bean 是在容器被创建时即被装配好了。
* 对于 scope 为 prototype 的原型模式，Bean 实例是在代码中使用该 Bean 实例时才进行装配的。

```
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="
       http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">

    <bean id="studentService1" class="com.lusifer.spring.service.impl.StudentServiceImpl" scope="singleton" />
    <bean id="studentService2" class="com.lusifer.spring.service.impl.StudentServiceImpl" scope="prototype" />
</beans>
```

```
package com.lusifer.spring.test;

import com.lusifer.spring.service.StudentService;
import org.springframework.context.ApplicationContext;
import org.springframework.context.support.ClassPathXmlApplicationContext;

public class MyTest {
    public static void main(String[] args) {
        ApplicationContext applicationContext = new ClassPathXmlApplicationContext("spring-context.xml");

        StudentService studentService1 = (StudentService) applicationContext.getBean("studentService1");
        StudentService studentService2 = (StudentService) applicationContext.getBean("studentService1");
        System.out.println("studentService1 == studentService2 ：" + (studentService1 == studentService2));

        StudentService studentService3 = (StudentService) applicationContext.getBean("studentService2");
        StudentService studentService4 = (StudentService) applicationContext.getBean("studentService2");
        System.out.println("studentService3 == studentService4 ：" + (studentService3 == studentService4));
    }
}
```