# 自定义 FormAuthenticationFilter

---

自定义 Form 表单的身份验证过滤器

## 创建 UsernamePasswordToken 类

创建该类的主要目的是为了扩展 Shiro 的 UsernamePasswordToken，前端会传“用户名、密码、记住我、验证码”等表单数据，但 Shiro 并不具备验证码功能

```
package com.lusifer.myshop.module.sys.security;

/**
 * 用户名密码令牌（含验证码）
 * <p>Title: UsernamePasswordToken</p>
 * <p>Description: </p>
 *
 * @author Lusifer
 * @version 1.0.0
 * @date 2018/1/14 21:04
 */
public class UsernamePasswordToken extends org.apache.shiro.authc.UsernamePasswordToken {
    /**
     * 验证码
     */
    private String validateCode;

    /**
     * 调用父类无参构造函数
     */
    public UsernamePasswordToken() {
        super();
    }

    /**
     * 扩展父类无参构造函数，增加需要的属性
     * @param username
     * @param password
     * @param rememberMe
     * @param host
     * @param validateCode
     */
    public UsernamePasswordToken(String username, String password, boolean rememberMe, String host, String validateCode) {
        super(username, password, rememberMe, host);
        this.validateCode = validateCode;
    }

    public String getValidateCode() {
        return validateCode;
    }

    public void setValidateCode(String validateCode) {
        this.validateCode = validateCode;
    }
}
```

## 创建 FormAuthenticationFilter 类

自定义的表单验证过滤器，重写登录的处理逻辑，包括登录成功和登录失败两种情况

注：此处代码并未完成，只是简单的重写，后续工作完成后再补全代码

```
package com.lusifer.myshop.module.sys.security;

import com.lusifer.myshop.common.utils.StringUtils;
import org.apache.shiro.authc.AuthenticationException;
import org.apache.shiro.authc.AuthenticationToken;
import org.apache.shiro.web.util.WebUtils;
import org.springframework.stereotype.Service;

import javax.servlet.ServletRequest;
import javax.servlet.ServletResponse;
import javax.servlet.http.HttpServletRequest;

/**
 * 自定义表单验证过滤类（含验证码）
 * <p>Title: FormAuthenticationFilter</p>
 * <p>Description: </p>
 *
 * @author Lusifer
 * @version 1.0.0
 * @date 2018/1/14 21:09
 */
@Service
public class FormAuthenticationFilter extends org.apache.shiro.web.filter.authc.FormAuthenticationFilter {

    /**
     * 创建令牌
     *
     * @param request
     * @param response
     * @return
     */
    @Override
    protected AuthenticationToken createToken(ServletRequest request, ServletResponse response) {
        String username = WebUtils.getCleanParam(request, "loginId");
        String password = DigestUtils.md5DigestAsHex(WebUtils.getCleanParam(request, "loginPwd").getBytes());
        boolean rememberMe = "on".equals(WebUtils.getCleanParam(request, "isRemember"));
        String host = StringUtils.getRemoteAddress((HttpServletRequest) request);
        String validateCode = WebUtils.getCleanParam(request, "validateCode");
        return new UsernamePasswordToken(username, password, rememberMe, host, validateCode);
    }

    /**
     * 登录成功后跳转
     * @param request
     * @param response
     * @throws Exception
     */
    @Override
    protected void issueSuccessRedirect(ServletRequest request, ServletResponse response) throws Exception {

    }

    /**
     * 登录失败调用事件
     * @param token
     * @param e
     * @param request
     * @param response
     * @return
     */
    @Override
    protected boolean onLoginFailure(AuthenticationToken token, AuthenticationException e, ServletRequest request, ServletResponse response) {
        return true;
    }
}
```