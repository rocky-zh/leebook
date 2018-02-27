# JDK 动态代理

---

动态代理是指，程序在整个运行过程中根本就不存在目标类的代理类，目标对象的代理对象只是由代理生成工具（如代理工厂类）在程序运行时由 JVM 根据反射等机制动态生成的。代理对象与目标对象的代理关系在程序运行时才确立。

对比静态代理，静态代理是指在程序运行前就已经定义好了目标类的代理类。代理类与目标类的代理关系在程序运行之前就确立了。

## 概念

动态代理的实现方式常用的有两种：使用 JDK 的 Proxy，与通过 CGLIB 生成代理。

通过 JDK 的 `java.lang.reflect.Proxy` 类实现动态代理 ， 会使用其静态方法 `newProxyInstance()`，依据目标对象、业务接口及业务增强逻辑三者，自动生成一个动态代理对象。

```
public static newProxyInstance ( ClassLoader loader, Class<?>[] interfaces,InvocationHandler handler) 
loader：目标类的类加载器，通过目标对象的反射可获取
interfaces：目标类实现的接口数组，通过目标对象的反射可获取
handler：业务增强逻辑，需要再定义。
```

## InvocationHandler

InvocationHandler 是个接口，实现了 InvocationHandler 接口的类用于加强目标类的主业务逻辑。这个接口中有一个方法 invoke()，具体加强的代码逻辑就是定义在该方法中的。程序调用主业务逻辑时，会自动调用 invoke() 方法。

```
public Object invoke ( Object proxy, Method method, Object[] args)
proxy：代表生成的代理对象
method：代表目标方法
args：代表目标方法的参数
```

由于该方法是由代理对象自动调用的，所以这三个参数的值不用程序员给出。

第二个参数为 Method 类对象，该类有一个方法也叫 invoke()，可以调用目标类的目标方法。

```
public Object invoke ( Object obj, Object... args)
obj：表示目标对象
args：表示目标方法参数
```

该方法的作用是：调用执行 obj 对象所属类的方法，这个方法由其调用者 Method 对象确定。

在代码中，一般的写法为 `method.invoke(target, args);`

## 实现

```
package com.lusifer.proxy.test;

import com.lusifer.proxy.service.SomeService;
import com.lusifer.proxy.service.impl.SomeServiceImpl;
import com.lusifer.proxy.service.impl.SomeServiceProxy;

import java.lang.reflect.InvocationHandler;
import java.lang.reflect.Method;
import java.lang.reflect.Proxy;

public class MyTest {
    public static void main(String[] args) {
        final SomeService target = new SomeServiceImpl();

        SomeService someService = (SomeService) Proxy.newProxyInstance(
                // 目标类加载器
                target.getClass().getClassLoader(),
                // 目标类的所有接口
                target.getClass().getInterfaces(),
                // 匿名内部类
                new InvocationHandler() {
                    /**
                     *
                     * @param proxy 代理对象
                     * @param method 目标方法
                     * @param args 目标方法的参数列表
                     * @return
                     * @throws Throwable
                     */
                    public Object invoke(Object proxy, Method method, Object[] args) throws Throwable {
                        Object result = method.invoke(target, args);
                        result = String.valueOf(result).toUpperCase();
                        return result;
                    }
                });

        String result = someService.sayHi();
        System.out.println("result = " + result);
        someService.sayHello();
    }
}
```
