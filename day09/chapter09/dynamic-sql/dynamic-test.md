# 搭建测试环境

---

## 定义数据库表

![](/assets/Lusifer1514409735.png)

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

    @Override
    public String toString() {
        return "Student{" +
                "id=" + id +
                ", name='" + name + '\'' +
                ", age=" + age +
                ", score=" + score +
                '}';
    }
}
```

## 定义测试类

```
package com.lusifer.mybatis.dao.test;

import com.lusifer.mybatis.dao.DynamicStudentDao;
import com.lusifer.mybatis.utils.MyBatisUtils;
import org.apache.ibatis.session.SqlSession;
import org.junit.After;
import org.junit.Before;

public class StudentDaoDynamicTest {
    private DynamicStudentDao dynamicStudentDao;
    private SqlSession sqlSession;

    @Before
    public void before() {
        sqlSession = MyBatisUtils.getSqlSession();
        dynamicStudentDao = sqlSession.getMapper(DynamicStudentDao.class);
    }
    
    @After
    public void after() {
        if (sqlSession != null) sqlSession.close();
    }
}
```

## 注意事项

在 mapper 的动态 SQL 中若出现大于号（>）、小于号（<）、大于等于号（>=），小于等于号（<=）等符号，最好将其转换为实体符号。否则，XML 可能会出现解析出错问题。

**特别是对于小于号（<），在 XML 中是绝对不能出现的。否则，一定出错。**

![](/assets/Lusifer1514409933.png)