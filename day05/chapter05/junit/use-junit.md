# JUnit 示例

---

## 引入依赖

```
<dependency>
    <groupId>junit</groupId>
    <artifactId>junit</artifactId>
    <version>4.12</version>
    <scope>test</scope>
</dependency>
```

## 单元测试的例子

```
package com.lusifer.use.junit.test;

import org.junit.After;
import org.junit.Before;
import org.junit.Test;

public class MyTest {
    /**
     * 执行测试方法前执行
     */
    @Before
    public void before() {
        System.out.println("执行 before() 方法");
    }

    /**
     * 执行测试方法后执行
     */
    @After
    public void after() {
        System.out.println("执行 after() 方法");
    }

    /**
     * 测试 sayHi
     */
    @Test
    public void testSayHi() {
        System.out.println("hi");
    }

    /**
     * 测试 sayHello
     */
    @Test
    public void testSayHello() {
        System.out.println("hello");
    }
}
```