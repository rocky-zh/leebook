# User Agent Utils

---

User Agent 中文名为用户代理，简称 UA，它是一个特殊字符串头，使得服务器能够识别客户使用的操作系统及版本、CPU 类型、浏览器及版本、浏览器渲染引擎、浏览器语言、浏览器插件等。

一些网站常常通过判断 UA 来给不同的操作系统、不同的浏览器发送不同的页面，因此可能造成某些页面无法在某个浏览器中正常显示，但通过伪装 UA 可以绕过检测。

我们使用 UserAgentUtils 工具包简化 UA 开发

## pom.xml 中增加依赖

```
<dependency>
    <groupId>eu.bitwalker</groupId>
    <artifactId>UserAgentUtils</artifactId>
    <version>1.20</version>
</dependency>
```

## UserAgent 常用方法

### 获取用户代理对象

```
/**
 * 获取用户代理对象
 *
 * @param request
 * @return
 */
public static UserAgent getUserAgent(HttpServletRequest request) {
    return UserAgent.parseUserAgentString(request.getHeader("User-Agent"));
}
```

### 获取设备类型

```
/**
 * 获取设备类型
 *
 * @param request
 * @return
 */
public static DeviceType getDeviceType(HttpServletRequest request) {
    return getUserAgent(request).getOperatingSystem().getDeviceType();
}
```

### 是否是 PC

```
/**
 * 是否是 PC
 *
 * @param request
 * @return
 */
public static boolean isComputer(HttpServletRequest request) {
    return DeviceType.COMPUTER.equals(getDeviceType(request));
}
```

### 是否是手机

```
/**
 * 是否是手机
 *
 * @param request
 * @return
 */
public static boolean isMobile(HttpServletRequest request) {
    return DeviceType.MOBILE.equals(getDeviceType(request));
}
```

### 是否是平板

```
/**
 * 是否是平板
 *
 * @param request
 * @return
 */
public static boolean isTablet(HttpServletRequest request) {
    return DeviceType.TABLET.equals(getDeviceType(request));
}
```

### 是否是平板和手机

```
/**
 * 是否是手机和平板
 *
 * @param request
 * @return
 */
public static boolean isMobileOrTablet(HttpServletRequest request) {
    DeviceType deviceType = getDeviceType(request);
    return DeviceType.MOBILE.equals(deviceType) || DeviceType.TABLET.equals(deviceType);
}
```

### 获取浏览器类型

```
/**
 * 获取浏览类型
 *
 * @param request
 * @return
 */
public static Browser getBrowser(HttpServletRequest request) {
    return getUserAgent(request).getBrowser();
}
```

### 判断 IE 版本

```
/**
 * 是否 IE 版本是否小于等于 IE8
 *
 * @param request
 * @return
 */
public static boolean isLteIE8(HttpServletRequest request) {
    Browser browser = getBrowser(request);
    return Browser.IE5.equals(browser) || Browser.IE6.equals(browser) || Browser.IE7.equals(browser) || Browser.IE8.equals(browser);
}
```