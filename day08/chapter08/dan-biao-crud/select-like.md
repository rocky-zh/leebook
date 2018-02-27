# 模糊查询

---

## 修改映射文件

在进行模糊查询时，需要进行字符串的拼接。SQL 中的字符串的拼接使用的是函数 `concat(arg1, arg2, …)` 。注意不能使用 Java 中的字符串连接符 +。

```
    <select id="selectByName" resultType="com.lusifer.mybatis.entity.Student">
        SELECT
          id,
          NAME,
          age,
          score
        FROM
          student
        WHERE NAME LIKE CONCAT('%', #{name}, '%')
    </select>
```

## 修改 DAO 实现类

```
    @Override
    public List<Student> selectByName(String name) {
        List<Student> studentList = new ArrayList<>();

        try {
            sqlSession = MyBatisUtils.getSqlSession();
            studentList = sqlSession.selectList("selectByName", name);
        } finally {
            if (sqlSession != null) {
                sqlSession.close();
            }
        }

        return studentList;
    }
```

## 修改测试类

```
    @Test
    public void testSelectByName() {
        List<Student> studentList = studentDao.selectByName("张");
        for (Student student : studentList) {
            System.out.println(student);
        }
    }
```