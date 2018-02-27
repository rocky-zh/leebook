# 单表 CRUD 操作

---

## 什么是 CRUD

CURD 操作，即指对数据库中实体对象的增 **C**reate、改 **U**pdate、查 **R**ead、删 **D**elete 操作。

## 自定义接口实现类

### 修改 DAO 接口

```
package com.lusifer.mybatis.dao;

import com.lusifer.mybatis.entity.Student;

import java.util.List;
import java.util.Map;

public interface StudentDao {
    /**
     * 插入学生表
     * @param student
     */
    public void insert(Student student);

    /**
     * 插入后直接获取 ID
     * @param student
     */
    public void insertForId(Student student);

    /**
     * 根据 ID 删除学生信息
     * @param id
     */
    public void delete(Long id);

    /**
     * 更新学生信息
     * @param student
     */
    public void update(Student student);

    /**
     * 查询所有学生信息
     * @return
     */
    public List<Student> selectAll();

    /**
     * 查询所有学生信息为 Map 集合
     * @return
     */
    public Map<String, Object> selectAllForMap();

    /**
     * 查询指定学生信息
     * @param id
     * @return
     */
    public Student selectById(Long id);

    /**
     * 使用 Map 查询指定学生信息
     * @param map
     * @return
     */
    public Student selectByMap(Map<String, Object> map);

    /**
     * 使用姓名查询学生信息（模糊查询）
     * @param name
     * @return
     */
    public List<Student> selectByName(String name);
}
```

### 修改 DAO 实现

```
package com.lusifer.mybatis.dao.impl;

import com.lusifer.mybatis.dao.StudentDao;
import com.lusifer.mybatis.entity.Student;
import com.lusifer.mybatis.utils.MyBatisUtils;
import org.apache.ibatis.session.SqlSession;

import java.util.List;
import java.util.Map;

public class StudentDaoImpl implements StudentDao {
    private SqlSession sqlSession;

    @Override
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

    @Override
    public void insertForId(Student student) {

    }

    @Override
    public void delete(Long id) {

    }

    @Override
    public void update(Student student) {

    }

    @Override
    public List<Student> selectAll() {
        return null;
    }

    @Override
    public Map<String, Object> selectAllForMap() {
        return null;
    }

    @Override
    public Student selectById(Long id) {
        return null;
    }

    @Override
    public Student selectByMap(Map<String, Object> map) {
        return null;
    }

    @Override
    public List<Student> selectByName(String name) {
        return null;
    }
}
```