# 使用 Map 进行查询

---

mapper 中 SQL 语句的动态参数也可以是 Map 的 key。

## 修改映射文件

```
    <select id="selectByMap" resultType="com.lusifer.mybatis.entity.Student">
        SELECT id, name, age, score FROM student WHERE id = #{student.id}
    </select>
```

## 修改 DAO 实现

```
    @Override
    public Student selectByMap(Map<String, Object> map) {
        Student student = null;

        try {
            sqlSession = MyBatisUtils.getSqlSession();
            student = sqlSession.selectOne("selectByMap", map);
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
    public void testSelectByMap() {
        Student param = new Student();
        param.setId(5L);

        Map<String, Object> params = new HashMap<>();
        params.put("student", param);
        Student student = studentDao.selectByMap(params);
        System.out.println(student);
    }
```