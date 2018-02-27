# 一对多关联查询

---

这里的一对多关联查询是指，在查询一方对象的时候，同时将其所关联的多方对象也都查询出来。

下面以国家 Country 与省 Province 间的一对多关系进行演示。

## 定义数据库表

![](/assets/Lusifer1514417182.png)

![](/assets/Lusifer1514417212.png)

## 定义实体类

在定义实体时，若定义的是双向关联，即双方的属性中均有对方对象作为域属性出现， 那么它们在定义各自的 toString()方法时需要注意，只让某一方可以输出另一方即可，不要让双方的 toString()方法均可输出对方。这样会形成递归调用，程序出错。

### Country

```
package com.lusifer.mybatis.entity;

import java.util.List;

public class Country {
    private Long id;
    private String name;
    private List<Province> provinces;

    public Long getId() {
        return id;
    }

    public void setId(Long id) {
        this.id = id;
    }

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    public List<Province> getProvinces() {
        return provinces;
    }

    public void setProvinces(List<Province> provinces) {
        this.provinces = provinces;
    }
}
```

### Province

```
package com.lusifer.mybatis.entity;

public class Province {
    private Long id;
    private String name;

    public Long getId() {
        return id;
    }

    public void setId(Long id) {
        this.id = id;
    }

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }
}
```

## 定义 DAO 接口

```
package com.lusifer.mybatis.dao;

import com.lusifer.mybatis.entity.Country;

public interface CountryDao {
    /**
     * 使用 ID 查询国家信息
     * @param id
     * @return
     */
    public Country selectCountryById(Long id);
}
```

## 定义测试类

```
package com.lusifer.mybatis.dao.test;

import com.lusifer.mybatis.dao.CountryDao;
import com.lusifer.mybatis.utils.MyBatisUtils;
import org.apache.ibatis.session.SqlSession;
import org.junit.After;
import org.junit.Before;

public class CountryDaoTest {
    private CountryDao countryDao;
    private SqlSession sqlSession;

    @Before
    public void before() {
        sqlSession = MyBatisUtils.getSqlSession();
        countryDao = sqlSession.getMapper(CountryDao.class);
    }

    @After
    public void after() {
        if (sqlSession != null) sqlSession.close();
    }
}
```

## 定义映射文件

### 多表连接查询方式

```
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN" "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<mapper namespace="com.lusifer.mybatis.dao.CountryDao">
    <resultMap id="countryMapper" type="com.lusifer.mybatis.entity.Country">
        <id column="id" property="id" />
        <result column="name" property="name" />

        <!-- 关联属性的映射关系 -->
        <collection property="provinces" ofType="com.lusifer.mybatis.entity.Province">
            <id column="pid" property="id" />
            <result column="pname" property="name" />
        </collection>
    </resultMap>

    <!-- 多表连接查询 -->
    <select id="selectCountryById" resultMap="countryMapper">
        SELECT
          a.id,
          a.name,
          b.id AS pid,
          b.name AS pname
        FROM
          country AS a,
          province AS b
        WHERE a.id = #{id}
          AND a.id = b.country_id
    </select>
</mapper>
```

注意，此时即使字段名与属性名相同，在 `<resultMap/>` 中也要写出它们的映射关系。因为框架是依据这人 `<resultMap/>` 封装对象的。

另外，在映射文件中使用 `<collection/>` 标签体现出两个实体对象间的关联关系。其两个属性的意义为：

* property：指定关联属性，即 Country 类中的集合属性
* ofType：集合属性的泛型类型

### 修改测试类

```
    @Test
    public void selectCountryById() {
        Country country = countryDao.selectCountryById(1L);
        System.out.println("国家：" + country.getName());
        System.out.println("省份：");
        for (Province province : country.getProvinces()) {
            System.out.println(province.getName());
        }
    }
```

### 多表单独查询方式

多表连接查询方式是将多张表进行连接，连为一张表后进行查询。其查询的本质是一张表。而多表单独查询方式是多张表各自查询各自的相关内容，需要多张表的联合数据了，则将主表的查询结果联合其它表的查询结果，封装为一个对象。

当然，这多个查询是可以跨越多个映射文件的，即是可以跨越多个 namespace 的。在使用其它 namespace 的查询时，添加上其所在的 namespace 即可。

```
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN" "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<mapper namespace="com.lusifer.mybatis.dao.CountryDao">
    <select id="selectProvinceByCountry" resultType="com.lusifer.mybatis.entity.Province">
        SELECT id, name FROM province WHERE country_id = #{countryId}
    </select>
    
    <resultMap id="countryMapper" type="com.lusifer.mybatis.entity.Country">
        <id column="id" property="id" />
        <result column="name" property="name" />

        <!-- 关联属性的映射关系 -->
        <!--
            集合的数据来自指定的 select 查询。
            而该 select 查询的动态参数来自 column 指定的字段值
        -->
        <collection
                property="provinces"
                ofType="com.lusifer.mybatis.entity.Province"
                select="selectProvinceByCountry"
                column="id" />
    </resultMap>

    <!-- 根据 id 查询 country 表 -->
    <select id="selectCountryById" resultMap="countryMapper">
        SELECT
          id,
          NAME
        FROM
          country
        WHERE id = #{id}
    </select>
</mapper>
```

![](/assets/Lusifer1514420052.png)

关联属性 `<collection/>` 的数据来自于另一个查询 `<selectProvinceByCountry/>`。而该查询 `<selectProvinceByCountry/>` 的动态参数 `country_id=#{country_id}` 的值来自于查询 `<selectCountryById/>` 的查询结果字段 `id`。