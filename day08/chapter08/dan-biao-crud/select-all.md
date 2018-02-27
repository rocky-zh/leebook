# 查询全部返回 List

---

## 修改映射文件

```
    <select id="selectAll" resultType="com.lusifer.mybatis.entity.Student">
        SELECT id, name, age, score FROM student
    </select>
```

## 修改 DAO 实现

```
    @Override
    public List<Student> selectAll() {
        List<Student> studentList = new ArrayList<>();

        try {
            sqlSession = MyBatisUtils.getSqlSession();
            studentList = sqlSession.selectList("selectAll");
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
    public void testSelectAll() {
        List<Student> studentList = studentDao.selectAll();
        for (Student student : studentList) {
            System.out.println(student);
        }
    }
```