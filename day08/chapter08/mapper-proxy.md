# Mapper 动态代理

---

在前面例子中自定义 Dao 接口实现类时发现一个问题：Dao 的实现类其实并没有干什么实质性的工作，它仅仅就是通过 SqlSession 的相关 API 定位到映射文件 mapper 中相应 id 的SQL 语句，真正对 DB 进行操作的工作其实是由框架通过 mapper 中的 SQL 完成的。

所以，MyBatis 框架就抛开了 Dao 的实现类，直接定位到映射文件 mapper 中的相应 SQL 语句，对 DB 进行操作。这种对 Dao 的实现方式称为 Mapper 的动态代理方式。

Mapper 动态代理方式无需程序员实现 Dao 接口。接口是由 MyBatis 结合映射文件自动生成的动态代理实现的。

## 映射文件的 namespace 属性值

一般情况下，一个 Dao 接口的实现类方法使用的是同一个 SQL 映射文件中的 SQL 映射 id。所以，MyBatis 框架要求，将映射文件中 `<mapper/>` 标签的 `namespace` 属性设为 Dao 接口的全类名，则系统会根据方法所属 Dao 接口，自动到相应 namespace 的映射文件中查找相关的 SQL 映射。

![](/assets/Lusifer1514316274.png)

## DAO 接口方法名

MyBatis 框架要求，接口中的方法名，与映射文件中相应的 SQL 标签的 id 值相同。系统会自动根据方法名到相应的映射文件中查找同名的 SQL 映射 id。

![](/assets/Lusifer1514316375.png)

## DAO 对象的获取

使用时，只需调用 SqlSession 的 getMapper()方法，即可获取指定接口的实现类对象。该方法的参数为指定 Dao 接口类的 class 值。

```
sqlSession = MyBatisUtils.getSqlSession();
studentDao = sqlSession.getMapper(StudentDao.class);
```

## 修改测试类

```
package com.lusifer.mybatis.dao.test;

import com.lusifer.mybatis.dao.StudentDao;
import com.lusifer.mybatis.entity.Student;
import com.lusifer.mybatis.utils.MyBatisUtils;
import org.apache.ibatis.session.SqlSession;
import org.junit.After;
import org.junit.Before;
import org.junit.Test;

/**
 * Mapper 动态代理测试
 * <p>Title: StudentDaoMapperProxyTest</p>
 * <p>Description: </p>
 *
 * @author Lusifer
 * @version 1.0.0
 * @date 2017/12/27 3:16
 */
public class StudentDaoMapperProxyTest {
    private StudentDao studentDao;
    private SqlSession sqlSession;

    @Before
    public void before() {
        sqlSession = MyBatisUtils.getSqlSession();
        studentDao = sqlSession.getMapper(StudentDao.class);
    }

    @Test
    public void insert() {
        Student student = new Student();
        student.setName("孙七");
        student.setAge(32);
        student.setScore(84D);
        studentDao.insert(student);
        sqlSession.commit();
    }

    @Test
    public void selectById() {
        Student student = studentDao.selectById(5L);
        System.out.println(student);
    }

    @After
    public void after() {
        if (sqlSession != null) sqlSession.close();
    }
}
```