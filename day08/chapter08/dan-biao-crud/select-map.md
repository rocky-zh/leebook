# 查询全部返回 Map

---

## 修改映射文件

映射文件不用修改

## 修改 DAO 实现

完成 DAO 实现类的 `selectAllForMap()` 方法。使用 `SqlSession` 的 `selectMap()` 方法完成查询操作。该查询方法会将查询出的每条记录先封装为指定对象，然后再将该对象作为 `value`， 将该对象的指定属性所对应的字段名作为 `key` 封装为一个 `Map` 对象。方法原型为：

`Map<Object,Object> selectMap (String statement, String mapKey)`

* statement：映射文件中配置的 SQL 语句的 id。
* mapKey：查询出的 Map 所要使用的 key。这个 key 为数据表的字段名。查询出的结果是一个 Map，每行记录将会对应一个 Map.entry 对象，该对象的 key 为指定字段的值，value 为记录数据所封装的对象。

```
    @Override
    public Map<String, Object> selectAllForMap() {
        Map<String, Object> map = new HashMap<>();

        try {
            sqlSession = MyBatisUtils.getSqlSession();
            map = sqlSession.selectMap("selectAll", "name");
        } finally {
            if (sqlSession != null) {
                sqlSession.close();
            }
        }

        return map;
    }
```

## 修改测试类

```
    @Test
    public void testSelectAllForMap() {
        Map<String, Object> map = studentDao.selectAllForMap();
        for (String s : map.keySet()) {
            System.out.println(map.get(s));
        }

        System.out.println(map.get("王五"));
    }
```

## 说明

若指定的作为 key 的属性值在 DB 中并不唯一，则后面的记录值会覆盖掉前面的值。即指定 key 的 value 值，一定是 DB 中该同名属性的最后一条记录值。