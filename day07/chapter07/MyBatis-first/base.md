# 基本程序

---

创建学生表并写入数据库

## POM

```
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>

    <groupId>com.lusifer</groupId>
    <artifactId>mybatis</artifactId>
    <version>1.0.0-SNAPSHOT</version>
    <packaging>jar</packaging>

    <properties>
        <java.version>1.8</java.version>
        <project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
        <project.reporting.outputEncoding>UTF-8</project.reporting.outputEncoding>
    </properties>

    <dependencies>
        <dependency>
            <groupId>org.slf4j</groupId>
            <artifactId>slf4j-log4j12</artifactId>
            <version>1.7.25</version>
        </dependency>
        <dependency>
            <groupId>junit</groupId>
            <artifactId>junit</artifactId>
            <version>4.12</version>
            <scope>test</scope>
        </dependency>
        <dependency>
            <groupId>org.mybatis</groupId>
            <artifactId>mybatis</artifactId>
            <version>3.4.5</version>
        </dependency>
        <dependency>
            <groupId>mysql</groupId>
            <artifactId>mysql-connector-java</artifactId>
            <version>5.1.30</version>
            <scope>runtime</scope>
        </dependency>
    </dependencies>

    <build>
        <finalName>${project.artifactId}</finalName>

        <resources>
            <resource>
                <directory>src/main/resources</directory>
                <includes>
                    <include>**/*.xml</include>
                    <include>**/*.properties</include>
                </includes>
            </resource>
        </resources>

        <plugins>
            <!-- 资源文件拷贝插件 -->
            <plugin>
                <groupId>org.apache.maven.plugins</groupId>
                <artifactId>maven-resources-plugin</artifactId>
                <version>2.7</version>
                <configuration>
                    <encoding>UTF-8</encoding>
                </configuration>
            </plugin>

            <plugin>
                <groupId>org.apache.maven.plugins</groupId>
                <artifactId>maven-compiler-plugin</artifactId>
                <configuration>
                    <source>${java.version}</source>
                    <target>${java.version}</target>
                </configuration>
            </plugin>
        </plugins>
    </build>
</project>
```

## 定义实体类

```
package com.lusifer.mybatis.entity;

public class Student {
    private Long id;
    private String name;
    private Integer age;
    private Double score;

    public Long getId() {
        return id;
    }

    public void setId(Long id) {
        this.id = id;
    }

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    public Integer getAge() {
        return age;
    }

    public void setAge(Integer age) {
        this.age = age;
    }

    public Double getScore() {
        return score;
    }

    public void setScore(Double score) {
        this.score = score;
    }
}
```

## 在数据库中创建空表

![](/assets/Lusifer1514221406.png)

## 定义 Dao 接口

```
package com.lusifer.mybatis.dao;

import com.lusifer.mybatis.entity.Student;

public interface StudentDao {
    /**
     * 插入学生表
     * @param student
     */
    public void insert(Student student);
}
```

## 定义映射文件

映射文件，简称为 mapper，主要完成 Dao 层中 SQL 语句的映射。映射文件名随意，一般放在 `src/resources/mapping` 文件夹中。这里映射文件名称定为 mapper.xml。

```
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN" "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<mapper namespace="com.lusifer.mybatis.dao.StudentDao">
    <insert id="insert" parameterType="com.lusifer.mybatis.entity.Student">
        INSERT INTO student(name, age, score) VALUES (#{name}, #{age}, #{score})
    </insert>
</mapper>
```

注意，`#{ }` 中写入的是 `Student` 类的属性名。

对于 parameterType 属性，框架会自动根据用户执行的 SqlSession 方法中的参数自动检测到，所以一般我们不用指定 parameterType 属性。一般写为如下形式：

```
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN" "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<mapper namespace="com.lusifer.mybatis.dao.StudentDao">
    <insert id="insert">
        INSERT INTO student(name, age, score) VALUES (#{name}, #{age}, #{score})
    </insert>
</mapper>
```

## 定义主配置文件

主配置文件名也可以随意命名，例如本例定义为 `mybatis-config.xml`。

而对于 `<dataSource/>` 标签中 `<property/> `的 `name` 属性名称，需要从帮助文档中查找。

```
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE configuration PUBLIC "-//mybatis.org//DTD Config 3.0//EN" "http://mybatis.org/dtd/mybatis-3-config.dtd">
<configuration>
    <!-- 配置 MyBatis 运行环境 -->
    <environments default="mybatis">
        <environment id="mybatis">
            <transactionManager type="JDBC"></transactionManager>
            <dataSource type="POOLED">
                <property name="driver" value="com.mysql.jdbc.Driver"/>
                <property name="url" value="jdbc:mysql://192.168.75.147:3306/mybatis?useUnicode=true&amp;characterEncoding=utf-8&amp;useSSL=false"/>
                <property name="username" value="root"/>
                <property name="password" value="123456"/>
            </dataSource>
        </environment>
    </environments>

    <!-- 注册映射文件 -->
    <mappers>
        <mapper resource="mapping/mapper.xml"/>
    </mappers>
</configuration>
```

## 定义 Dao 实现

```
package com.lusifer.mybatis.dao.impl;

import com.lusifer.mybatis.dao.StudentDao;
import com.lusifer.mybatis.entity.Student;
import org.apache.ibatis.io.Resources;
import org.apache.ibatis.session.SqlSession;
import org.apache.ibatis.session.SqlSessionFactory;
import org.apache.ibatis.session.SqlSessionFactoryBuilder;

import java.io.IOException;
import java.io.InputStream;

public class StudentDaoImpl implements StudentDao {
    private SqlSession sqlSession;

    public void insert(Student student) {
        try {
            // 1. 读取主配置文件
            InputStream inputStream = Resources.getResourceAsStream("mybatis-config.xml");
            // 2. 创建 SqlSessionFactory 对象
            SqlSessionFactory sqlSessionFactory = new SqlSessionFactoryBuilder().build(inputStream);
            // 3. 创建 SqlSession 对象
            sqlSession = sqlSessionFactory.openSession();
            // 4. 操作
            sqlSession.insert("insert", student);
            // 5. SqlSession 提交
            sqlSession.commit();
        } catch (IOException e) {
            e.printStackTrace();
        } finally {
            if (sqlSession != null) {
                sqlSession.close();
            }
        }
    }
}
```

## 创建测试类

```
package com.lusifer.mybatis.dao.test;

import com.lusifer.mybatis.dao.StudentDao;
import com.lusifer.mybatis.dao.impl.StudentDaoImpl;
import com.lusifer.mybatis.entity.Student;
import org.junit.Before;
import org.junit.Test;

public class StudentDaoTest {
    private StudentDao studentDao;

    @Before
    public void before() {
        studentDao = new StudentDaoImpl();
    }

    @Test
    public void testInsert() {
        Student student = new Student();
        student.setName("张三");
        student.setAge(22);
        student.setScore(90D);
        studentDao.insert(student);
    }
}
```

测试时发生异常 `Field 'id' doesn't have a default value` 是因为没有将主键定义为自增列