# 基于注解的 DI

---

对于 DI 使用注解，将不再需要在 Spring 配置文件中声明 Bean 实例。Spring 中使用注解， 需要在原有 Spring 运行环境基础上再做一些改变

需要在 Spring 配置文件中配置组件扫描器，用于在指定的基本包中扫描注解。

```
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:context="http://www.springframework.org/schema/context"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd http://www.springframework.org/schema/context http://www.springframework.org/schema/context/spring-context.xsd">

    <context:component-scan base-package="com.lusifer.spring"/>
</beans>
```

## @Component

需要在类上使用注解 `@Component`，该注解的 value 属性用于指定该 bean 的 id 值。

```
package com.lusifer.spring.entity;

import org.springframework.stereotype.Component;

@Component(value = "student")
public class Student {
    private Long id;
    private String name;

    public Long getId() {
        return id;
    }

    public void setId(Long id) {
        this.id = id;
    }

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    @Override
    public String toString() {
        return "Student{" +
                "id=" + id +
                ", name='" + name + '\'' +
                '}';
    }
}
```

Spring 还提供了 3 个功能基本和@Component 等效的注解：

* @Repository：用于对 DAO 实现类进行注解
* @Service：用于对 Service 实现类进行注解
* @Controller：用于对 Controller 实现类进行注解

之所以创建这三个功能与 @Component 等效的注解，是为了以后对其进行功能上的扩展，使它们不再等效。

## @Scope

需要在类上使用注解 @Scope，其 value 属性用于指定作用域。默认为 singleton。

![](/assets/Lusifer1514921149.png)

## @Value

需要在属性上使用注解 @Value，该注解的 value 属性用于指定要注入的值。

![](/assets/Lusifer1514921245.png)

使用该注解完成属性注入时，类中无需 setter。当然，若属性有 setter，则也可将其加到 setter 上。

## @Autowired

需要在域属性上使用注解 @Autowired，该注解默认使用**按类型自动装配 Bean** 的方式。

使用该注解完成属性注入时，类中无需 setter。当然，若属性有 setter，则也可将其加到 setter 上。

![](/assets/Lusifer1514921700.png)

![](/assets/Lusifer1514921733.png)

## @PostConstruct

在方法上使用 @PostConstruct 相当于初始化

![](/assets/Lusifer1514922518.png)

