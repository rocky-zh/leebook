# 集成 Druid

---

## Dependencies 新增依赖

```
<alibaba-druid.version>1.1.6</alibaba-druid.version>

<!-- Alibaba Begin -->
<dependency>
    <groupId>com.alibaba</groupId>
    <artifactId>druid</artifactId>
    <version>${alibaba-druid.version}</version>
</dependency>
<!-- Alibaba End -->
```

## Module 新增依赖

```
<!-- Alibaba Begin -->
<dependency>
    <groupId>com.alibaba</groupId>
    <artifactId>druid</artifactId>
</dependency>
<!-- Alibaba End -->
```

## 配置 log4j.properties

```
log4j.rootLogger=DEBUG, console, file

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

## 配置 jdbc.properties

### 开发环境

在 `src/main/resources/profiles/development` 目录下创建 `jdbc.properties` 文件

```
jdbc.driverClass=com.mysql.jdbc.Driver
jdbc.connectionURL=jdbc:mysql://192.168.75.147:3306/myshop?useUnicode=true&characterEncoding=utf-8&useSSL=false
jdbc.username=root
jdbc.password=123456
```

### 测试环境

在 `src/main/resources/profiles/testing` 目录下创建 `jdbc.properties` 文件

```
jdbc.driverClass=com.mysql.jdbc.Driver
jdbc.connectionURL=jdbc:mysql://10.3.134.116:3306/myshop?useUnicode=true&characterEncoding=utf-8&useSSL=false
jdbc.username=root
jdbc.password=123456
```

## 配置 Druid

在 `src/main/resources` 目录下创建 `spring-context-druid.xml`

```
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:context="http://www.springframework.org/schema/context"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd http://www.springframework.org/schema/context http://www.springframework.org/schema/context/spring-context.xsd">

    <!-- 加载配置属性文件 -->
    <context:property-placeholder ignore-unresolvable="true" location="classpath:jdbc.properties"/>

    <!-- 数据源配置, 使用 Druid 数据库连接池 -->
    <bean id="dataSource" class="com.alibaba.druid.pool.DruidDataSource" init-method="init" destroy-method="close">
        <!-- 数据源驱动类可不写，Druid默认会自动根据URL识别DriverClass -->
        <property name="driverClassName" value="${jdbc.driverClass}"/>

        <!-- 基本属性 url、user、password -->
        <property name="url" value="${jdbc.connectionURL}"/>
        <property name="username" value="${jdbc.username}"/>
        <property name="password" value="${jdbc.password}"/>

        <!-- 配置初始化大小、最小、最大 -->
        <property name="initialSize" value="${jdbc.pool.init}"/>
        <property name="minIdle" value="${jdbc.pool.minIdle}"/>
        <property name="maxActive" value="${jdbc.pool.maxActive}"/>

        <!-- 配置获取连接等待超时的时间 -->
        <property name="maxWait" value="60000"/>

        <!-- 配置间隔多久才进行一次检测，检测需要关闭的空闲连接，单位是毫秒 -->
        <property name="timeBetweenEvictionRunsMillis" value="60000"/>

        <!-- 配置一个连接在池中最小生存的时间，单位是毫秒 -->
        <property name="minEvictableIdleTimeMillis" value="300000"/>

        <property name="validationQuery" value="${jdbc.testSql}"/>
        <property name="testWhileIdle" value="true"/>
        <property name="testOnBorrow" value="false"/>
        <property name="testOnReturn" value="false"/>

        <!-- 配置监控统计拦截的filters -->
        <property name="filters" value="stat"/>
    </bean>
</beans>
```

## 升级 jdbc.properties

```
# JDBC

jdbc.driverClass=com.mysql.jdbc.Driver
jdbc.connectionURL=jdbc:mysql://192.168.75.147:3306/myshop?useUnicode=true&characterEncoding=utf-8&useSSL=false
jdbc.username=root
jdbc.password=123456

# JDBC Pool
jdbc.pool.init=1
jdbc.pool.minIdle=3
jdbc.pool.maxActive=20

# JDBC Test
jdbc.testSql=SELECT 'x' FROM DUAL
```

## 导入 Spring 配置文件

```
<import resource="classpath:spring-context-druid.xml" />
```