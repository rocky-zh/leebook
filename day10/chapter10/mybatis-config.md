# 创建 mybatis 配置文件

---

在 `src/main/resources` 目录下创建 `mybatis-config.xml` 文件

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