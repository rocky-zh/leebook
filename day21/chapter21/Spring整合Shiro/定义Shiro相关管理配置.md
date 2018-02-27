# 定义 Shiro 相关管理配置

---

## 增加 Spring Shiro 配置

```
<!-- 定义 Shiro 安全管理配置 -->
<bean id="securityManager" class="org.apache.shiro.web.mgt.DefaultWebSecurityManager">
    <property name="realm" ref="systemAuthorizingRealm"/>
    <property name="sessionManager" ref="sessionManager"/>
    <property name="cacheManager" ref="shiroCacheManager"/>
</bean>

<!-- 定义 Shiro 会话管理配置 -->
<bean id="sessionManager" class="org.apache.shiro.web.session.mgt.DefaultWebSessionManager">
    <!-- 会话超时时间，单位：毫秒  -->
    <property name="globalSessionTimeout" value="1800000"/>
    <!-- 定时清理失效会话, 清理用户直接关闭浏览器造成的孤立会话   -->
    <property name="sessionValidationInterval" value="120000"/>
    <property name="sessionValidationSchedulerEnabled" value="true"/>
    <property name="sessionIdCookie" ref="sessionIdCookie"/>
    <property name="sessionIdCookieEnabled" value="true"/>
</bean>

<!-- 指定本系统SESSIONID, 默认为: JSESSIONID 问题: 与SERVLET容器名冲突, 如JETTY, TOMCAT 等默认JSESSIONID,
    当跳出SHIRO SERVLET时如ERROR-PAGE容器会为JSESSIONID重新分配值导致登录会话丢失! -->
<bean id="sessionIdCookie" class="org.apache.shiro.web.servlet.SimpleCookie">
    <constructor-arg name="name" value="myshop.session.id"/>
</bean>

<!-- 定义授权缓存管理器 -->
<bean id="shiroCacheManager" class="org.apache.shiro.cache.ehcache.EhCacheManager">
    <property name="cacheManager" ref="cacheManager"/>
</bean>

<!-- 保证实现了 Shiro 内部 lifecycle 函数的 bean 执行 -->
<bean id="lifecycleBeanPostProcessor" class="org.apache.shiro.spring.LifecycleBeanPostProcessor"/>

<!-- AOP 式方法级权限检查  -->
<bean class="org.springframework.aop.framework.autoproxy.DefaultAdvisorAutoProxyCreator" depends-on="lifecycleBeanPostProcessor">
    <property name="proxyTargetClass" value="true"/>
</bean>
<bean class="org.apache.shiro.spring.security.interceptor.AuthorizationAttributeSourceAdvisor">
    <property name="securityManager" ref="securityManager"/>
</bean>
```

## 创建 SystemAuthorizingRealm 系统安全实现类

该类为系统安全实现类，主要包括“认证与授权”两个部分

```
package com.lusifer.myshop.module.sys.security;

import org.apache.shiro.authc.AuthenticationException;
import org.apache.shiro.authc.AuthenticationInfo;
import org.apache.shiro.authc.AuthenticationToken;
import org.apache.shiro.authz.AuthorizationInfo;
import org.apache.shiro.realm.AuthorizingRealm;
import org.apache.shiro.subject.PrincipalCollection;
import org.springframework.stereotype.Service;

/**
 * 系统安全认证实现类
 * <p>Title: SystemAuthorizingRealm</p>
 * <p>Description: </p>
 *
 * @author Lusifer
 * @version 1.0.0
 * @date 2018/1/14 21:48
 */
@Service
public class SystemAuthorizingRealm extends AuthorizingRealm {

    /**
     * 授权查询回调函数, 进行鉴权但缓存中无用户的授权信息时调用
     * @param principalCollection
     * @return
     */
    @Override
    protected AuthorizationInfo doGetAuthorizationInfo(PrincipalCollection principalCollection) {
        return null;
    }

    /**
     * 认证回调函数, 登录时调用
     * @param authenticationToken
     * @return
     * @throws AuthenticationException
     */
    @Override
    protected AuthenticationInfo doGetAuthenticationInfo(AuthenticationToken authenticationToken) throws AuthenticationException {
        UsernamePasswordToken usernamePasswordToken = (UsernamePasswordToken) authenticationToken;
        return null;
    }
}
```