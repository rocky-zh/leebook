# Spring MVC 集成 Shiro

---

修改 `spring-mvc.xml` 配置文件，增加如下配置：

```
<!-- 支持 Shiro 对 Controller 的方法级 AOP 安全控制 begin-->
<bean class="org.springframework.aop.framework.autoproxy.DefaultAdvisorAutoProxyCreator"
      depends-on="lifecycleBeanPostProcessor">
    <property name="proxyTargetClass" value="true"/>
</bean>

<bean class="org.springframework.web.servlet.handler.SimpleMappingExceptionResolver">
    <property name="exceptionMappings">
        <props>
            <prop key="org.apache.shiro.authz.UnauthorizedException">error/403</prop>
            <prop key="java.lang.Throwable">error/500</prop>
        </props>
    </property>
</bean>
<!-- 支持 Shiro 对 Controller 的方法级 AOP 安全控制 end -->
```

## 权限注解

### @RequiresAuthentication

表示当前 Subject 已经通过 login 进行了身份验证；即 Subject. isAuthenticated() 返回true。

### @RequiresUser

表示当前 Subject 已经身份验证或者通过记住我登录的

### @RequiresGuest

表示当前 Subject 没有身份验证或通过记住我登录过，即是游客身份

### @RequiresRoles

`@RequiresRoles(value={“admin”, “user”}, logical= Logical.AND)`

表示当前 Subject 需要角色 admin 和 user

### @RequiresPermissions

`@RequiresPermissions(value={“user:a”, “user:b”}, logical= Logical.OR)`

表示当前 Subject 需要权限 user:a 或 user:b