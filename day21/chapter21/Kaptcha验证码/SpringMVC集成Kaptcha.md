# Spring MVC 集成 Kaptcha

---

## POM

```
<google-kaptcha.version>0.0.9</google-kaptcha.version>

<dependency>
    <groupId>com.github.axet</groupId>
    <artifactId>kaptcha</artifactId>
    <version>${google-kaptcha.version}</version>
</dependency>
```

## Spring 注入 Kaptcha

在 `src/main/resources` 目录下创建 `spring-context-kaptcha.xml` 文件

```
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">

    <bean id="defaultKaptcha" class="com.google.code.kaptcha.impl.DefaultKaptcha">
        <property name="config">
            <bean class="com.google.code.kaptcha.util.Config">
                <!--通过构造函数注入属性值 -->
                <constructor-arg type="java.util.Properties">
                    <props>
                        <!-- 验证码宽度 -->
                        <prop key="kaptcha.image.width">80</prop>
                        <!-- 验证码高度 -->
                        <prop key="kaptcha.image.height">34</prop>
                        <!-- 生成验证码内容范围 -->
                        <prop key="kaptcha.textproducer.char.string">abcde2345678gfynmnpwx</prop>
                        <!-- 验证码个数 -->
                        <prop key="kaptcha.textproducer.char.length">4</prop>
                        <!-- 是否有边框 -->
                        <prop key="kaptcha.border">no</prop>
                        <!-- 验证码字体颜色 -->
                        <prop key="kaptcha.textproducer.font.color">red</prop>
                        <!-- 验证码字体大小 -->
                        <prop key="kaptcha.textproducer.font.size">22</prop>
                        <!-- 验证码所属字体样式 -->
                        <prop key="kaptcha.textproducer.font.names">Arial, Courier</prop>
                        <prop key="kaptcha.background.clear.from">white</prop>
                        <prop key="kaptcha.background.clear.to">white</prop>
                        <prop key="kaptcha.obscurificator.impl">com.google.code.kaptcha.impl.ShadowGimpy</prop>
                        <prop key="kaptcha.noise.impl">com.google.code.kaptcha.impl.NoNoise</prop>
                        <!-- 干扰线颜色 -->
                        <prop key="kaptcha.noise.color">red</prop>
                        <!-- 验证码文本字符间距 -->
                        <prop key="kaptcha.textproducer.char.space">4</prop>
                    </props>
                </constructor-arg>
            </bean>
        </property>
    </bean>

</beans>
```

## ValidateCodeController

```
package com.lusifer.myshop.module.sys.web;

import com.google.code.kaptcha.impl.DefaultKaptcha;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Controller;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.RequestMethod;

import javax.imageio.ImageIO;
import javax.servlet.ServletOutputStream;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import java.awt.image.BufferedImage;
import java.io.IOException;

/**
 * 验证码
 * <p>Title: ValidateCodeController</p>
 * <p>Description: </p>
 *
 * @author Lusifer
 * @version 1.0.0
 * @date 2018/1/14 11:20
 */
@Controller
public class ValidateCodeController {
    @Autowired
    private DefaultKaptcha defaultKaptcha;

    /**
     * 获取验证码
     * @param request
     * @param response
     * @throws IOException
     */
    @RequestMapping(value = "validate/code", method = RequestMethod.GET)
    public void validateCode(HttpServletRequest request, HttpServletResponse response) throws IOException {
        response.setDateHeader("Expires", 0);
        response.setHeader("Cache-Control", "no-store, no-cache, must-revalidate");
        response.addHeader("Cache-Control", "post-check=0, pre-check=0");
        response.setHeader("Pragma", "no-cache");
        response.setContentType("image/jpeg");
        String capText = defaultKaptcha.createText();
        // Session 中的验证码 K V
        request.getSession().setAttribute(com.google.code.kaptcha.Constants.KAPTCHA_SESSION_KEY, capText);
        BufferedImage bi = defaultKaptcha.createImage(capText);
        ServletOutputStream out = response.getOutputStream();
        ImageIO.write(bi, "jpg", out);
        try {
            out.flush();
        } finally {
            out.close();
        }
    }
}
```

## JSP 调用

```
<div class="form-group">
    <input id="validateCode" name="validateCode" type="text" class="form-control pull-left" placeholder="验证码" style="width: 170px;">
    <img id="imgValidateCode" src="/validate/code" style="cursor: pointer;" />
</div>
```

## JS 刷新

```
// 验证码切换
$("#imgValidateCode").bind("click", function () {
    $(this).attr("src", "/validate/code?" + Math.random());
});
```