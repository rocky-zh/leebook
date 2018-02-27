# AOP 编程的例子

---

## 定义接口

```
package com.lusifer.aop.demo.service;

public interface StudentService {
    public void sayHi();
}
```

## 定义实现

```
package com.lusifer.aop.demo.service.impl;

import com.lusifer.aop.demo.service.StudentService;

public class StudentServiceImpl implements StudentService {
    public void sayHi() {
        System.out.println("执行 sayHi() 方法");
    }
}
```

## 定义测试类

```
package com.lusifer.aop.demo.test;

import com.lusifer.aop.demo.service.StudentService;
import com.lusifer.aop.demo.service.impl.StudentServiceImpl;

import java.lang.reflect.InvocationHandler;
import java.lang.reflect.Method;
import java.lang.reflect.Proxy;

public class MyTest {
    public static void main(String[] args) {
        // 定义目标类
        final StudentService target = new StudentServiceImpl();
        StudentService studentService = (StudentService) Proxy.newProxyInstance(
                target.getClass().getClassLoader(),
                target.getClass().getInterfaces(),
                new InvocationHandler() {
                    public Object invoke(Object proxy, Method method, Object[] args) throws Throwable {
                        System.out.println("前置增强");
                        Object invoke = method.invoke(target, args);
                        System.out.println("后置增强");
                        return invoke;
                    }
                });

        studentService.sayHi();
    }
}
```