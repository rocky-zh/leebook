# 通知 Advice

---

通知（Advice），切面的一种实现，可以完成简单织入功能（织入功能就是在这里完成的）。常用通知有：前置通知、后置通知、环绕通知、异常处理通知。

## 通知的用法步骤

对于通知的定义、配置与使用，主要分为以下几步：

### 定义目标类

定义目标类，就是定义之前的普通 Bean 类，也就是即将被增强的 Bean 类。

### 定义通知类

通知类是指，实现了相应通知类型接口的类。当前，实现了这些接口，就要实现这些接口中的方法，而这些方法的执行，则是根据不同类型的通知，其执行时机不同。

* 前置通知：在目标方法执行之前执行
* 后置通知：在目标方法执行之后执行
* 环绕通知：在目标方法执行之前与之后均执行
* 异常处理通知：在目标方法执行过程中，若发生指定异常，则执行通知中的方法

### 注册目标类

即在 Spring 配置文件中注册目标对象 Bean

```
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="
       http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">

    <!-- 配置目标对象 -->
    <bean id="studentServiceTarget" class="com.lusifer.aop.demo.service.impl.StudentServiceImpl" />
</beans>
```

### 注册通知切面

即在 Spring 配置文件中注册定义的通知对象 Bean

```
package com.lusifer.aop.demo.advice;

import org.springframework.aop.MethodBeforeAdvice;

import java.lang.reflect.Method;

/**
 * 方法的前置通知
 * <p>Title: MethodBeforeAdvice</p>
 * <p>Description: </p>
 *
 * @author Lusifer
 * @version 1.0.0
 * @date 2018/1/4 4:21
 */
public class MyMethodBeforeAdvice implements MethodBeforeAdvice {
    public void before(Method method, Object[] objects, Object o) throws Throwable {
        System.out.println("执行前置通知方法");
    }
}
```

```
<!-- 配置切面：通知 -->
<bean id="methodBeforeAdvice" class="com.lusifer.aop.demo.advice.MyMethodBeforeAdvice" />
```

### 注册代理工厂 Bean 类对象 ProxyFactoryBean

```
<!-- 配置代理 -->
<bean id="studentServiceProxy" class="org.springframework.aop.framework.ProxyFactoryBean">
    <property name="target" ref="studentServiceTarget" />
    <property name="interfaces" value="com.lusifer.aop.demo.service.StudentService" />
    <property name="interceptorNames" value="methodBeforeAdvice" />
</bean>
```

这里的代理使用的是 ProxyFactoryBean 类。代理对象的配置，是与 JDK 的 Proxy 代理参数是一致的，都需要指定三部分：目标类，接口，切面。

### 客户端访问动态代理对象

客户端访问的是动态代理对象，而非原目标对象。因为代理对象可以将交叉业务逻辑按照通知类型，动态的织入到目标对象的执行中。

```
package com.lusifer.aop.demo.test;

import com.lusifer.aop.demo.service.StudentService;
import org.springframework.context.ApplicationContext;
import org.springframework.context.support.ClassPathXmlApplicationContext;

public class MyTest1 {
    public static void main(String[] args) {
        ApplicationContext applicationContext = new ClassPathXmlApplicationContext("spring-context.xml");
        StudentService studentService = (StudentService) applicationContext.getBean("studentServiceProxy");
        studentService.sayHi();
    }
}
```

### 在容器中的整体配置

```
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="
       http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">

    <!-- 配置目标对象 -->
    <bean id="studentServiceTarget" class="com.lusifer.aop.demo.service.impl.StudentServiceImpl" />

    <!-- 配置切面：通知 -->
    <bean id="methodBeforeAdvice" class="com.lusifer.aop.demo.advice.MyMethodBeforeAdvice" />

    <!-- 配置代理 -->
    <bean id="studentServiceProxy" class="org.springframework.aop.framework.ProxyFactoryBean">
        <property name="target" ref="studentServiceTarget" />
        <property name="interfaces" value="com.lusifer.aop.demo.service.StudentService" />
        <property name="interceptorNames" value="methodBeforeAdvice" />
    </bean>
</beans>
```