# 从属性文件读取数据库连接配置

---

为了方便对数据库连接的管理，DB 连接四要素数据一般都是存放在一个专门的属性文件中的。MyBatis 主配置文件需要从这个属性文件中读取这些数据。

## 定义属性文件

在 `src/main/resources` 目录下创建 `jdbc.properties` 文件

```
jdbc.driver=com.mysql.jdbc.Driver
jdbc.url=jdbc:mysql://192.168.75.147:3306/mybatis?useUnicode=true&characterEncoding=utf-8&useSSL=false
jdbc.username=root
jdbc.password=123456
```

## 修改注配置文件

对主配置文件，第一，需要注册属性文件。第二，需要从属性文件中通过 key，将其 value 读取出来。

```
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE configuration PUBLIC "-//mybatis.org//DTD Config 3.0//EN" "http://mybatis.org/dtd/mybatis-3-config.dtd">
<configuration>
    <properties resource="jdbc.properties"/>

    <!-- 配置 MyBatis 运行环境 -->
    <environments default="mybatis">
        <environment id="mybatis">
            <transactionManager type="JDBC"></transactionManager>
            <dataSource type="POOLED">
                <property name="driver" value="${jdbc.driver}"/>
                <property name="url" value="${jdbc.url}"/>
                <property name="username" value="${jdbc.username}"/>
                <property name="password" value="${jdbc.password}"/>
            </dataSource>
        </environment>
    </environments>

    <!-- 注册映射文件 -->
    <mappers>
        <mapper resource="mapping/mapper.xml"/>
    </mappers>
</configuration>
```