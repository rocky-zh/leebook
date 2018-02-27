# JS 判断微信浏览器

---

根据判断 UA 中是否有关键字 micromessenger，有的话则是微信内置浏览器

```
/**
 * 用户代理工具
 */
var UserAgent = function () {
    var ua = window.navigator.userAgent.toLowerCase();

    /**
     * 判断是否是微信内置浏览器
     * @returns {boolean}
     */
    var handleIsWeChat = function () {
        if (ua.match(/MicroMessenger/i) == 'micromessenger') {
            return true;
        } else {
            return false;
        }
    };

    return {
        isWeChat: function () {
            return handleIsWeChat();
        }
    }
}();
```