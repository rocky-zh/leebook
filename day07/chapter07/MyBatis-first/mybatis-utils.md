# 使用工具类简化开发

---

由于每一次执行 SqlSession 的方法，均需首先获取到该对象。而 SqlSession 对象的获取又相对比较繁琐，所以，可以将获取 SqlSession 对象定义为一个工具类方法。

SqlSession 对象是通过 SqlSessionFactory 对象创建的。由于 SqlSessionFactory 类为重量级对象，且为线程安全的，所以可以将  SqlSessionFactory 对象定义为单例的。

## 创建工具类

```
package com.lusifer.mybatis.utils;

import org.apache.ibatis.io.Resources;
import org.apache.ibatis.session.SqlSession;
import org.apache.ibatis.session.SqlSessionFactory;
import org.apache.ibatis.session.SqlSessionFactoryBuilder;

import java.io.IOException;
import java.io.InputStream;

public class MyBatisUtils {
    private static SqlSessionFactory sqlSessionFactory;

    public static SqlSession getSqlSession() {
        if (sqlSessionFactory == null) {
            try {
                // 读取主配置文件
                InputStream inputStream = Resources.getResourceAsStream("mybatis-config.xml");
                // 创建 SqlSession 工厂
                sqlSessionFactory = new SqlSessionFactoryBuilder().build(inputStream);
            } catch (IOException e) {
                e.printStackTrace();
            }
        }

        return sqlSessionFactory.openSession();
    }
}
```

## 修改 Dao 接口的实现

该实现类使用 MyBatisUtils 工具类获取 SqlSession 对象。注意，由于这里没有异常需要处理，但 SqlSession 必须要关闭，所以这里的代码必须要有 `finally{}` 语句块，但无需 `catch{}` 代码块了。

```
package com.lusifer.mybatis.dao.impl;

import com.lusifer.mybatis.dao.StudentDao;
import com.lusifer.mybatis.entity.Student;
import com.lusifer.mybatis.utils.MyBatisUtils;
import org.apache.ibatis.session.SqlSession;

public class StudentDaoImpl implements StudentDao {
    private SqlSession sqlSession;

    public void insert(Student student) {
        try {
            sqlSession = MyBatisUtils.getSqlSession();
            sqlSession.insert("insert", student);
            sqlSession.commit();
        } finally {
            if (sqlSession != null) {
                sqlSession.close();
            }
        }
    }
}
```