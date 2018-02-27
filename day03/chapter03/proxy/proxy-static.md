# 静态代理

---

## 概念

静态代理是指，代理类在程序运行前就已经定义好，其与目标类的关系在程序运行前就已经确立。

## 实现

### 创建目标接口

```
package com.lusifer.proxy.service;

/**
 * 目标接口
 * <p>Title: SomeService</p>
 * <p>Description: </p>
 *
 * @author Lusifer
 * @version 1.0.0
 * @date 2017/12/20 1:52
 */
public interface SomeService {
    public String sayHi();
    public void sayHello();
}
```

### 创建目标实现类

```
package com.lusifer.proxy.service.impl;

import com.lusifer.proxy.service.SomeService;

/**
 * 目标类
 * <p>Title: SomeServiceImpl</p>
 * <p>Description: </p>
 *
 * @author Lusifer
 * @version 1.0.0
 * @date 2017/12/20 1:52
 */
public class SomeServiceImpl implements SomeService {
    public String sayHi() {
        System.out.println("执行 sayHi() 方法");
        return "hi";
    }

    public void sayHello() {
        System.out.println("执行 sayHello() 方式");
    }
}
```

### 创建增强代理类

```
package com.lusifer.proxy.service.impl;

import com.lusifer.proxy.service.SomeService;

/**
 * 增强代理类
 * <p>Title: SomeServiceProxy</p>
 * <p>Description: </p>
 *
 * @author Lusifer
 * @version 1.0.0
 * @date 2017/12/20 1:52
 */
public class SomeServiceProxy implements SomeService {

    private SomeService someService;

    public SomeServiceProxy() {
        someService = new SomeServiceImpl();
    }

    public String sayHi() {
        String result = someService.sayHi();
        return result.toUpperCase();
    }

    public void sayHello() {
        someService.sayHello();
    }
}
```

### 创建测试类

```
package com.lusifer.proxy.test;

import com.lusifer.proxy.service.SomeService;
import com.lusifer.proxy.service.impl.SomeServiceProxy;

public class MyTest {
    public static void main(String[] args) {
        SomeService someService = new SomeServiceProxy();
        String result = someService.sayHi();
        System.out.println("result = " + result);
        someService.sayHello();
    }
}
```