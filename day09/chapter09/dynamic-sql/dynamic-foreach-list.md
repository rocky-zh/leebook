# foreach 标签-遍历集合

---

## 遍历泛型为基本类型的 List

### 定义 DAO 接口

```
    /**
     * 使用 foreach 标签以 list 基本类型的形式查询
     * @param ids
     * @return
     */
    public List<Student> selectByForeachWithListBase(List<Long> ids);
```

### 定义映射文件

```
    <!-- foreach -->
    <select id="selectByForeachWithListBase" resultType="com.lusifer.mybatis.entity.Student">
        <!-- select * from student where id in (2, 4) -->
        SELECT
            id,
            name,
            age,
            score
        FROM
          student
        <if test="list != null and list.size > 0">
            WHERE id IN
            <foreach collection="list" open="(" close=")" item="id" separator=",">
                #{id}
            </foreach>
        </if>
    </select>
```

### 修改测试类

```
    @Test
    public void selectByForeachWithListBase() {
        List<Long> ids = new ArrayList<>();
        ids.add(2L);
        ids.add(4L);
        List<Student> studentList = dynamicStudentDao.selectByForeachWithListBase(ids);
        for (Student student : studentList) {
            System.out.println(student);
        }
    }
```

## 遍历泛型为自定义类型的 List

### 定义 DAO 接口

```
    /**
     * 使用 foreach 标签以 list 自定义类型的形式查询
     * @param students
     * @return
     */
    public List<Student> selectByForeachWithListCustom(List<Student> students);
```

### 定义映射文件

```
    <!-- foreach -->
    <select id="selectByForeachWithListCustom" resultType="com.lusifer.mybatis.entity.Student">
        <!-- select * from student where id in (2, 4) -->
        SELECT
            id,
            name,
            age,
            score
        FROM
          student
        <if test="list != null and list.size > 0">
            WHERE id IN
            <foreach collection="list" open="(" close=")" item="student" separator=",">
                #{student.id}
            </foreach>
        </if>
    </select>
```

### 修改测试类

```
    @Test
    public void selectByForeachWithListCustom() {
        List<Student> params = new ArrayList<>();
        Student param1 = new Student();
        param1.setId(2L);
        Student param2 = new Student();
        param2.setId(4L);
        params.add(param1);
        params.add(param2);

        List<Student> studentList = dynamicStudentDao.selectByForeachWithListCustom(params);
        for (Student student : studentList) {
            System.out.println(student);
        }
    }
```