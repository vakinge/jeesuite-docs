## 介绍
统一认证平台，提供了直接登录、oauth2.0登录、第三方平台(微博、微信、QQ)登录，jsonp ajax方式登录。

## 接入说明java版

### 添加依赖

```
<dependency>
	<groupId>com.aygframework.support</groupId>
	<artifactId>ayg-passport-support</artifactId>
	<version>${ayg.support.version}</version>
</dependency>
```

### 添加配置
```
#应用id，接入统一分配
auth.client.id=taxmobile
#接入密匙
auth.client.secret=43c8a42943a422c8b177b4256436fda
#认证中心地址
auth.server.baseurl=https://passport.aiyuangong.com
#登录成功默认跳转页面,优先跳转Referer
auth.login-success.default.redirect.uri=http://zcyqtest94.aiyuangong.com=/index.html
```

### 登录入口
1. 直接登录
  * ${auth.server.baseurl}/auth/login
  * ${auth.server.baseurl}/auth/logout
2. oauth2.0协议登录
  * ${your-base-url}/oauth2/login
3. 第三方开放平台登录
  * 微信登录:${your-base-url}/snslogin/weixin
  * 微博登录:${your-base-url}/snslogin/weibo
  * QQ登录:${your-base-url}/snslogin/qq
4. 微信公众号平台登录
  * ${your-base-url}/snslogin/wxgzh?scope=snsapi_base
  * ${your-base-url}/snslogin/wxgzh?scope=snsapi_userinfo
5. jsonp ajax登录

```
$.ajax({
    url: 'https://passporttest94.aiyuangong.com/jsonp/auth/login?username=15920558210&password=123456&redirect_uri=/login_callback',
    type: "get",
    async: false,
    dataType: "jsonp",
    jsonp: "callback",
    jsonpCallback: "jsonpLoginCallback",
    success: function(json) {
        alert("code:" + json.code + ",msg:" + json.msg);
    },
    error: function() {
        alert('Error');
    }
});
```
 - **redirect_uri**:登录成功回调地址，默认为/login_callback
>  回调完整url，如：http://${you-base-url}/login_callback?session_id=xxxxx&expires_in=3600&login_type=jsonp

### 跨域设置cookies
```
//获取需要跨域设置cookies列表
$.getJSON('http://passporttest94.aiyuangong.com/sso/get_setcookie_list',function(json){
    for (var i in json) {
       setSSOCookies(json[i]);
    }
});

//通过jsonp设置cookies
function setSSOCookies(ticket){
  $.ajax({
            url: 'http://passporttest94.aiyuangong.com/sso/setcookie?ticket='+ticket+'&session_id='+sessionId,
            type: "get",
            cache: false,
            async: false,
            dataType: "jsonp",
            jsonp: "callback",
            jsonpCallback: "jsonpSetCookieCallback",
            success: function(json) {
               if(json.code == 200){
                 alert("跨域setCookies finish,ticket="+ticket);
               }else{
                  alert(json.msg);
               }
            },
            error: function(a, b, c) {
                alert(a + b + c);
            }
        });
}
```

 ---
 ## 附
 [用户API接口](http://192.168.1.94:8010/swagger-ui.html#/user-controller)
 [php接入demo](../example/passport)
 
 
  