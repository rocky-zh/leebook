# 日志输出控制文件

---

## 简介

Log4j 的日志输出控制文件，主要由三个部分构成：

* 日志信息的输出位置：控制日志信息将要输出的位置，是控制台还是文件等。
* 日志信息的输出格式：控制日志信息的显示格式，即以怎样的字符串形式显示。
* 日志信息的输出级别：控制日志信息的显示内容，即显示哪些级别的日志信息。

有了日志输出控制文件，代码中只要设置好日志信息内容及其级别即可，通过控制文件便可控制这些日志信息的输出了。

## 在 `src/main/resources` 目录下创建 `log4j.properties` 配置文件

```
log4j.rootLogger=INFO, console, file

log4j.appender.console=org.apache.log4j.ConsoleAppender
log4j.appender.console.layout=org.apache.log4j.PatternLayout
log4j.appender.console.layout.ConversionPattern=%d %p [%c] - %m%n

log4j.appender.file=org.apache.log4j.DailyRollingFileAppender
log4j.appender.file.File=logs/log.log
log4j.appender.file.layout=org.apache.log4j.PatternLayout
log4j.appender.A3.MaxFileSize=1024KB
log4j.appender.A3.MaxBackupIndex=10
log4j.appender.file.layout.ConversionPattern=%d %p [%c] - %m%n
```

## 日志输出控制文件分析

日志属性文件 log4j.properties 是专门用于控制日志输出的。其主要进行三方面控制：

* 输出位置：控制日志将要输出的位置，是控制台还是文件等。
* 输出布局：控制日志信息的显示形式
* 输出级别：控制要输出的日志级别。

日志属性文件由两个对象组成：日志附加器与根日志。

根日志，即为 Java 代码中的日志记录器，其主要由两个属性构成：日志输出级别与日志附加器。

日志附加器，则由日志输出位置定义，由其它很多属性进行修饰，如输出布局、文件位置、文件大小等。

## 定义日志附加器 appender

所谓日志附加器，就是为日志记录器附加上很多其它设置信息。附加器的本质是一个接口，其定义语法为：log4j.appender.appenderName = 输出位置

appenderName 为自定义名称。

## 常用的附加器实现类

* org.apache.log4j.ConsoleAppender：日志输出到控制台
* org.apache.log4j.FileAppender：日志输出到文件
* org.apache.log4j.RollingFileAppender：当日志文件大小到达指定尺寸的时候将产生一个新的日志文件
* org.apache.log4j.DailyRollingFileAppender：每天产生一个日志文件

## 修饰日志附加器

所谓修饰日志附加器，就是为定义好的附加器添加一些属性，以控制到指定位置的输出。