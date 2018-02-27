# Slf4j 与 Log4j

---

## 简介

slf4j 的全称是 Simple Loging Facade For Java，即它仅仅是一个为 Java 程序提供日志输出的统一接口，并不是一个具体的日志实现方案，就比如 JDBC 一样，只是一种规则而已。所以单独的 slf4j 是不能工作的，必须搭配其他具体的日志实现方案，比如 apache  的 `org.apache.log4j.Logger`，jdk自带的 `java.util.logging.Logger` 以及 `log4j` 等。

## 依赖

```
<dependency>
    <groupId>org.slf4j</groupId>
    <artifactId>slf4j-log4j12</artifactId>
    <version>1.7.25</version>
</dependency>
```

## 例子

```
package com.lusifer;

import org.slf4j.Logger;
import org.slf4j.LoggerFactory;

public class MyTest {
    public static final Logger logger = LoggerFactory.getLogger(MyTest.class);

    public static void main(String[] args) {
        logger.info("slf4j for info");
        logger.debug("slf4j for debug");
        logger.error("slf4j for error");
        logger.warn("slf4j for warn");

        String message = "Hello SLF4J";
        logger.info("slf4j message is : {}", message);
    }
}
```

## 占位符说明

打日志的时候使用了 `{}` 占位符，这样就不会有字符串拼接操作，减少了无用 `String` 对象的数量，节省了内存。并且，记住，在生产最终日志信息的字符串之前，这个方法会检查一个特定的日志级别是不是打开了，这不仅降低了内存消耗而且预先降低了 `CPU` 去处理字符串连接命令的时间。