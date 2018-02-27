# 为应用指定多个 Spring 配置文件

---

在实际应用里，随着应用规模的增加，系统中 Bean 数量也大量增加，导致配置文件变得非常庞大、臃肿。为了避免这种情况的产生，提高配置文件的可读性与可维护性，可以将 Spring 配置文件分解成多个配置文件。

## 平等关系的配置文件

将配置文件分解为地位平等的多个配置文件，并将所有配置文件的路径定义为一个 String 数组，将其作为容器初始化参数出现。其将与可变参的容器构造器匹配。

![](/assets/Lusifer1514919983.png)

```
package com.lusifer.spring.test;

import com.lusifer.spring.entity.School;
import com.lusifer.spring.entity.Teacher;
import org.springframework.context.ApplicationContext;
import org.springframework.context.support.ClassPathXmlApplicationContext;

public class MyTest1 {
    public static void main(String[] args) {
        ApplicationContext applicationContext = new ClassPathXmlApplicationContext("spring-context-school.xml", "spring-context-teacher.xml");

        Teacher teacher = (Teacher) applicationContext.getBean("teacher");
        System.out.println(teacher);

        School school = (School) applicationContext.getBean("school");
        System.out.println(school);
    }
}
```

## 包含关系的配置文件

各配置文件中有一个总文件，总配置文件将各其它子文件通过 `<import/>` 引入。在 Java 代码中只需要使用总配置文件对容器进行初始化即可。

![](/assets/Lusifer1514920308.png)

```
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="
       http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">

    <import resource="classpath:spring-context.xml" />
    <import resource="classpath:spring-context-school.xml" />
    <import resource="classpath:spring-context-teacher.xml" />
</beans>
```

```
package com.lusifer.spring.test;

import com.lusifer.spring.entity.School;
import com.lusifer.spring.entity.Teacher;
import org.springframework.context.ApplicationContext;
import org.springframework.context.support.ClassPathXmlApplicationContext;

public class MyTest2 {
    public static void main(String[] args) {
        ApplicationContext applicationContext = new ClassPathXmlApplicationContext("spring-context-total.xml");

        Teacher teacher = (Teacher) applicationContext.getBean("teacher");
        System.out.println(teacher);

        School school = (School) applicationContext.getBean("school");
        System.out.println(school);
    }
}
```