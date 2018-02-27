# 查询单个对象

---

## 修改映射文件

```
    <select id="selectById" resultType="com.lusifer.mybatis.entity.Student">
        SELECT id, name, age, score FROM student WHERE id = #{id}
    </select>
```

## 修改 DAO 实现类

使用 SqlSession 的 selectOne() 方法。其会将查询的结果记录封装为一个指定类型的对象。方法原型为：Object selectOne (String statement, Object parameter)

* statement：映射文件中配置的 SQL 语句的 id
* parameter：查询条件中动态参数的值

```
    @Override
    public Student selectById(Long id) {
        Student student = null;

        try {
            sqlSession = MyBatisUtils.getSqlSession();
            student = sqlSession.selectOne("selectById", id);
        } finally {
            if (sqlSession != null) {
                sqlSession.close();
            }
        }

        return student;
    }
```

## 修改测试类

```
    @Test
    public void testSelectById() {
        Student student = studentDao.selectById(5L);
        System.out.println(student);
    }
```