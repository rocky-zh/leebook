# 顾问 Advisor

---

通知（Advice）是 Spring 提供的一种切面（Aspect）。但其功能过于简单：只能将切面织入到目标类的所有目标方法中，无法完成将切面织入到指定目标方法中。

顾问（Advisor）是 Spring 提供的另一种切面。其可以完成更为复杂的切面织入功能。

**PointcutAdvisor 是顾问的一种，可以指定具体的切入点。顾问将通知进行了包装，会根据不同的通知类型，在不同的时间点，将切面织入到不同的切入点。**

PointcutAdvisor 接口有两个较为常用的实现类：

* NameMatchMethodPointcutAdvisor 名称匹配方法切入点顾问
* RegexpMethodPointcutAdvisor 正则表达式匹配方法切入点顾问

## 名称匹配方法切入点顾问

NameMatchMethodPointcutAdvisor，即名称匹配方法切入点顾问。容器可根据配置文件中指定的方法名来设置切入点。

代码不用修改，只在配置文件中注册一个顾问，然后使用通知属性 advice 与切入点的方法名 mappedName 对其进行配置。代理中的切面，使用这个顾问即可。

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
    <bean id="methodReturningAdvice" class="com.lusifer.aop.demo.advice.MyAfterReturningAdvice" />

    <!-- 配置切面：顾问 -->
    <bean id="myAdvisor" class="org.springframework.aop.support.NameMatchMethodPointcutAdvisor">
        <property name="advice" ref="methodBeforeAdvice" />
        <property name="mappedNames" value="sayHi, sayHello" />
    </bean>

    <!-- 配置代理 -->
    <bean id="studentServiceProxy" class="org.springframework.aop.framework.ProxyFactoryBean">
        <property name="target" ref="studentServiceTarget" />
        <property name="interfaces" value="com.lusifer.aop.demo.service.StudentService" />
        <property name="interceptorNames" value="myAdvisor" />
    </bean>
</beans>
```

![](/assets/Lusifer1515014699.png)

## 正则表达式方法切入点顾问

RegexpMethodPointcutAdvisor，即正则表达式方法顾问。容器可根据正则表达式来设置切入点。注意，与正则表达式进行匹配的对象是接口中的方法名，而非目标类（接口的实现类）的方法名。

```
<!-- 配置切面：顾问 -->
<bean id="myAdvisor" class="org.springframework.aop.support.RegexpMethodPointcutAdvisor">
    <property name="advice" ref="methodBeforeAdvice" />
    <property name="pattern" value=".*say.*|.*do.*" />
</bean>
```

![](/assets/Lusifer1515015153.png)

这里的正则表达式常用的运算符有三个，如下表：

| 运算符 | 名称 | 说明                            |
|--------|------|---------------------------------|
| .      | 点号 | 表示任意单个字符                |
| +      | 加号 | 表示前一个字符出现一次或者多次  |
| &#42;      | 星号 | 表示前一个字符出现 0 次或者多次 |

| 举例                | 分析                                                                                                                                                                       |
|---------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| aa&#92;.bb&#92;.Hello&#92;.do.&#42; | 表示 aa.bb.Hello 类中所有以 do 开头的方法作为切入点方法。由于点号既是包与包间的连接符，又是正则表达式运算符。为了指明这里的点号不是正则表达式运算符。所以要使用转义字符—`\.` |
| .&#42;do.&#42;             | 表示方法全名（包括报名、接口名、方法名）中包含 do 的方法作为切入点方法                                                                                                     |
| .&#42;do.&#42;&#166;.&#42;S.&#42;        | 表示方法名中包含 do 或 S 的方法均为切入点方法                                                                                                                              |
| .&#42;do.&#42;S.&#42;           | 表示方法全名中即包含 do 又包含 S 的方法，作为切入点方法                                                                                                                    |