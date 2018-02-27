# CGLIB 动态代理

---

## 概念

使用 JDK 的 Proxy 实现代理，要求目标类与代理类实现相同的接口。若目标类不存在接口，则无法使用该方式实现。

但对于无接口的类，要为其创建动态代理，就要使用 CGLIB 来实现。CGLIB 代理的生成原理是生成目标类的子类，而子类是增强过的，这个子类对象就是代理对象。所以，使用 CGLIB 生成动态代理，要求目标类必须能够被继承，即不能是 final 的类。

> CGLIB(Code Generation Library)是一个开源项目，是一个强大的、高性能的、高质量的代码生成类库。它可以在运行期扩展和增强 Java 类。Hibernate 用它来实现持久对象的字节码的动态生成，Spring 用它来实现 AOP 编程。

CGLIB 包的底层是通过使用一个小而快的字节码处理框架 ASM(Java 字节码操控框架)，来转换字节码并生成新的类。CGLIB 是通过对字节码进行增强来生成代理的。

## 实现 CGLIB 的动态代理

### pom.xml

```
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>

    <groupId>com.lusifer</groupId>
    <artifactId>proxy-dynamic-cglib</artifactId>
    <version>1.0.0-SNAPSHOT</version>
    <packaging>jar</packaging>

    <dependencies>
        <dependency>
            <groupId>cglib</groupId>
            <artifactId>cglib</artifactId>
            <version>3.2.5</version>
        </dependency>
    </dependencies>
</project>
```

### 创建目标类

```
package com.lusifer.proxy.service;

public class SomeService {
    public String sayHi() {
        System.out.println("执行 sayHi() 方法");
        return "hi";
    }

    public void sayHello() {
        System.out.println("执行 sayHello() 方式");
    }
}
```

### 创建 CGLIB 工厂类

```
package com.lusifer.proxy.factoryt;

import com.lusifer.proxy.service.SomeService;
import net.sf.cglib.proxy.Enhancer;
import net.sf.cglib.proxy.MethodInterceptor;
import net.sf.cglib.proxy.MethodProxy;

import java.lang.reflect.Method;

public class CglibFactory implements MethodInterceptor {
    private SomeService target;

    public CglibFactory() {
        target = new SomeService();
    }

    public SomeService creator() {
        // 创建增强器
        Enhancer enhancer = new Enhancer();
        // 指定目标类，即父类
        enhancer.setSuperclass(SomeService.class);
        // 设置回调函数
        enhancer.setCallback(this);

        return (SomeService) enhancer.create();
    }

    // 回调方法
    public Object intercept(Object o, Method method, Object[] objects, MethodProxy methodProxy) throws Throwable {
        // 调用目标方法
        Object result = method.invoke(target, objects);
        result = String.valueOf(result).toUpperCase();
        return result;
    }
}
```

### 使用工厂创建目标类

```
package com.lusifer.proxy.test;

import com.lusifer.proxy.factoryt.CglibFactory;
import com.lusifer.proxy.service.SomeService;

public class MyTest {
    public static void main(String[] args) {
        SomeService someService = new CglibFactory().creator();
        String result = someService.sayHi();
        System.out.println("result = " + result);
        someService.sayHello();
    }
}
```