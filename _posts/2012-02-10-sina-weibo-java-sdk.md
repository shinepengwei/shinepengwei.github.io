---
layout: post
title: 新浪微博开放平台java-SDK使用的基本方法
category: "编程"
tag: "JSP"
---
<ol>
	<li>
<div>创建一个应用，主要是为了获得App Key和Secret Key。具体说明请看官方文档：</div>
<a href="http://open.weibo.com/wiki/%E6%96%B0%E6%89%8B%E6%8C%87%E5%8D%97">http://open.weibo.com/wiki/%E6%96%B0%E6%89%8B%E6%8C%87%E5%8D%97</a></li>
	<li>
<div>下载新浪微薄的SDK，本文使用的是java。</div>
<a href="http://code.google.com/p/weibo4j/">http://code.google.com/p/weibo4j/</a></li>
	<li>
<div>请先填写相关配置：在Config.properties里</div>
client_ID ：appkey 创建应用获取到的appkey （App Key）

client_SERCRET ：app_secret 创建应用获取到的appsecret （Secret Key）

redirect_URI : 回调地址 OAuth2的回调地址
（这个是为了在验证后返回到我们的web地址，此篇文章只是为了测试，不创建web应用，因此可以不需要设置）

在调用/使用所有的api之前都需要进行Oath2验证。

关于OAUTH协议可以参考?http://oauth.net/2

使用OAUTH认证来获取微博数据介绍详细见：Oauth2

具体步骤：

调用example里：OAuth4Code.java。

会出现以下错误，是因为我们没有设置回调地址，没关系，复制code。

<img src="/wp-content/uploads/2012/08/081612_0449_javaSD1.png" alt="" />

在myeclipse控制台里面黏贴进去，然后回车。

<img src="/wp-content/uploads/2012/08/081612_0449_javaSD2.png" alt="" />

会显示以下结果：

<img src="/wp-content/uploads/2012/08/081612_0449_javaSD3.png" alt="" />

这里可以看到我们得到了需要的access token。复制下来，以便下面调用其他API使用。</li>
	<li>
<div>调用API</div>
经过Oath2验证以后就可以随意调用开放平台提供的各种API。

比如，我们调用获取粉丝，调用example.friendships.GetFlowers.java.

[java]
package weibo4j.examples.friendships;
import weibo4j.Friendships;
import weibo4j.Weibo;
import weibo4j.examples.oauth2.Log;
import weibo4j.model.User;
import weibo4j.model.UserWapper;
import weibo4j.model.WeiboException;
public class GetFollowers {
   public static void main(String[] args) {
   String access_token = &quot;2.00RkFknBuc4hPD05d15a59e6Yi3WRB(Access Token)&quot;;
   Weibo weibo = new Weibo();
   weibo.setToken(access_token);
   Friendships fm = new Friendships();
   String screen_name =&quot;任意用户的名字&quot;;
   try {
      UserWapper users = fm.getFollowersByName(screen_name);
      for(User u : users.getUsers()){
      Log.logInfo(u.toString());
   }
   System.out.println(users.getNextCursor());
   System.out.println(users.getPreviousCursor());
   System.out.println(users.getTotalNumber());
   } catch (WeiboException e) {
      e.printStackTrace();
   }
}
}
[/java]

运行之前设置access token 和用户名字，运行后：

<img src="/wp-content/uploads/2012/08/081612_0449_javaSD4.png" alt="" />

注意：

Example提供的都是控制它程序，也就是说都用运行main函数的demo，要运用到web程序需要响应的修改。

附：

使用Oath1验证（10年时的版本）的Web应用：

<a href="http://haolloyin.blog.51cto.com/1177454/412445">http://haolloyin.blog.51cto.com/1177454/412445</a></li>
</ol>
