# 删除操作

---

## 修改映射文件

```
<delete id="delete">
    DELETE FROM student WHERE id = #{id}
</delete>
```

## 修改 DAO 实现

```
@Override
public void delete(Long id) {
    try {
        sqlSession = MyBatisUtils.getSqlSession();
        sqlSession.delete("delete", id);
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
public void testDelete() {
    studentDao.delete(6L);
}
```