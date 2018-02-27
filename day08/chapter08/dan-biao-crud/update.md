# 更新操作

---

## 修改映射文件

```
    <update id="update">
        UPDATE
          student
        SET
          NAME = #{name}, age = #{age}, score = #{score}
        WHERE id = #{id}
    </update>
```

## 修改 DAO 实现

```
    @Override
    public void update(Student student) {
        try {
            sqlSession = MyBatisUtils.getSqlSession();
            sqlSession.update("update", student);
            sqlSession.commit();
        } finally {
            if (sqlSession != null) {
                sqlSession.close();
            }
        }
    }
```

## 修改测试类

```
    @Test
    public void testUpdate() {
        Student student = new Student();
        student.setId(5L);
        student.setName("孙七");
        student.setAge(32);
        student.setScore(84D);
        studentDao.update(student);
    }
```