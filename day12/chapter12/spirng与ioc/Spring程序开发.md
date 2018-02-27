# Spring 程序开发

---

## 官方网站

https://spring.io

## 添加依赖

```
<dependency>
    <groupId>org.springframework</groupId>
    <artifactId>spring-context</artifactId>
    <version>4.3.13.RELEASE</version>
</dependency>
```

## 定义接口与实现

### 接口

```
package com.lusifer.spring.service;

public interface StudentService {
    public void sayHi();
}
```

### 实现

```
package com.lusifer.spring.service.impl;

import com.lusifer.spring.service.StudentService;

public class StudentServiceImpl implements StudentService {
    public void sayHi() {
        System.out.println("执行 sayHi() 方法");
    }
}
```

## 创建 Spring 配置文件

在 `src/main/resources` 目录下创建 `spring-context.xml` 配置文件

```
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="
       http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">

    <bean id="studentService" class="com.lusifer.spring.service.impl.StudentServiceImpl" />
</beans>
```

* `<bean />`：用于定义一个实例对象。一个实例对应一个 bean 元素。
* id：该属性是 Bean 实例的唯一标识，程序通过 id 属性访问 Bean，Bean 与 Bean 间的依赖关系也是通过 id 属性关联的。
* class：指定该 Bean 所属的类，注意这里只能是类，不能是接口。

## 定义测试类

```
package com.lusifer.spring.service.test;

import com.lusifer.spring.service.StudentService;
import org.junit.Test;
import org.springframework.context.ApplicationContext;
import org.springframework.context.support.ClassPathXmlApplicationContext;

public class StudentServiceTest {
    @Test
    public void testSayHi() {
        // 获取容器
        ApplicationContext context = new ClassPathXmlApplicationContext("spring-context.xml");

        // 从容器中获取对象
        StudentService studentService = (StudentService) context.getBean("studentService");
        studentService.sayHi();
    }
}
```

