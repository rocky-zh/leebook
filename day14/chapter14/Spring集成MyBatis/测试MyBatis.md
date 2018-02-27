# 测试 MyBatis

---

```
package com.lusifer.myshop.module.user.mapper.test;

import com.lusifer.myshop.module.user.entity.TbUser;
import com.lusifer.myshop.module.user.mapper.TbUserMapper;
import org.junit.Test;
import org.junit.runner.RunWith;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.test.context.ContextConfiguration;
import org.springframework.test.context.junit4.SpringJUnit4ClassRunner;

import java.util.Date;

@RunWith(SpringJUnit4ClassRunner.class)  //使用junit4进行测试
@ContextConfiguration({"classpath:spring-context.xml"})
public class UserMapperTest {
    @Autowired
    private TbUserMapper tbUserMapper;

    @Test
    public void testInsert() {
        TbUser tbUser = new TbUser();
        tbUser.setId(1L);
        tbUser.setUsername("admin");
        tbUser.setPassword("admin");
        tbUser.setEmail("admin@admin.com");
        tbUser.setPhone("15888888888");
        tbUser.setCreated(new Date());
        tbUser.setUpdated(new Date());
        tbUserMapper.insert(tbUser);
    }

    @Test
    public void testGetById() {
        TbUser tbUser = tbUserMapper.selectByPrimaryKey(1L);
        System.out.println(tbUser);
    }
}
```