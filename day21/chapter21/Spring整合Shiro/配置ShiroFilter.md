# 配置 Shiro Filter

---

## web.xml

DelegatingFilterProxy 会自动到 Spring 容器中查找名字为 shiroFilter 的 bean 并把 filter 请求交给它处理

```
<filter>
    <filter-name>shiroFilter</filter-name>
    <filter-class>org.springframework.web.filter.DelegatingFilterProxy</filter-class>
    <init-param>
        <param-name>targetFilterLifecycle</param-name>
        <param-value>true</param-value>
    </init-param>
</filter>
<filter-mapping>
    <filter-name>shiroFilter</filter-name>
    <url-pattern>/*</url-pattern>
</filter-mapping>
```

## 创建 Spring 配置文件

在 `src/main/resources` 目录下创建 `spring-context-shiro.xml` 文件

```
<!-- Shiro 权限过滤器定义 -->
<bean name="shiroFilterChainDefinitions" class="java.lang.String">
    <constructor-arg>
        <value>
            /static/** = anon
            /login = authc
            /logout = logout
            /** = user
        </value>
    </constructor-arg>
</bean>

<!-- 安全认证过滤器 -->
<bean id="shiroFilter" class="org.apache.shiro.spring.web.ShiroFilterFactoryBean">
    <property name="securityManager" ref="securityManager"/>
    <property name="loginUrl" value="/login"/>
    <property name="filters">
        <map>
            <entry key="authc" value-ref="formAuthenticationFilter"/>
        </map>
    </property>
    <property name="filterChainDefinitions">
        <ref bean="shiroFilterChainDefinitions"/>
    </property>
</bean>
```

1. **formAuthenticationFilter：** 为基于 Form 表单的身份验证过滤器；此处可以再添加自己的 Filter bean 定义
2. **shiroFilter：** 此处使用 ShiroFilterFactoryBean 来创建 ShiroFilter 过滤器；filters 属性用于定义自己的过滤器；filterChainDefinitions 用于声明 url 和 filter 的关系

## Shiro 内置过滤器说明

### 认证过滤器

anon，authcBasic，auchc，user

* **anon：** 例子 `/static/**=anon` 没有参数，表示可以匿名使用
* **authcBasic：** 例子 `/admins/user/**=authcBasic` 没有参数，表示 httpBasic 认证
* **auchc：** 例子 `/login=authc` 没有参数，表示需要认证才能使用
* **user：** 例子 `/**=user` 没有参数，表示必须存在用户，当登入操作时不做检查

### 授权过滤器

perms，roles，ssl，rest，port

* **perms：** 例子 `/user/**=perms[user:add:*]`，参数可以写多个，多个时必须加上引号，并且参数之间用逗号分割，例如 `/user/**=perms["user:add:*,user:modify:*"]`，当有多个参数时必须每个参数都通过才通过
* **roles：** 例子 `/user/**=roles[admin]`，参数可以写多个，多个时必须加上引号，并且参数之间用逗号分割，当有多个参数时，例如 `/user/**=roles["admin,guest"]`，每个参数通过才算通过
* **ssl：** 例子 `/user/**=ssl`，没有参数，表示安全的 url 请求，协议为 https
* **rest：** 例子 `/user/**=rest[user]`，根据请求的方法，相当于 `/user/**=perms[user:method]`，其中 method 为 post，get，delete 等
* **port：** 例子 `/user/**=port[8081]`，当请求的 url 的端口不是 8081 是跳转到 schemal://serverName:8081?queryString，其中 schemal 是协议 http 或 https 等，serverName 是你访问的 host，8081 是 url 配置里 port 的端口，queryString 是你访问的 url 里的 ? 后面的参数