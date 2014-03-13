---
layout: post
title: 会话管理和cookie
category: "编程"
tag: "JSP"
---
在Web应用中，由于HTTP协议是无状态链接，即每一次请求对Web应用来说都无法获知与该客户以前的历史信息，而Web应用往往需要使用会话信息。

会话信息往往是上一节所说的会话属性，可以将某些变量设置在session对象里。

<strong>会话实现方法</strong>：

在某客户的一次请求中，如果Web应用对于该用户第一次希望为该客户建立会话以保存以后需要使用的信息，则容器生成一个会话ID，并通过响应把ID返回给客户；客户在以后的每一次请求中都同时发回这个会话ID。容器看到这个ID后，就根据这个ID找到匹配的会话。

在客户端和服务器交换会话ID信息是，最常用的方式是通过cookie交换ID信息。对于用户会话ID交换的cookie工作，容器可以完全自己实现而对编程人员透明。

使用HttpSession session=request.getSession();?? （1）创建一个新的session对象或者根据已存在的会话ID找到相应的Sesssion对象。

当在servlet使用语句（1）创建session对象时，容器就会自动的建立一个新的HttpSession对象，生成唯一的会话ID，创建新的cookie对象，将会话ID与cookie以及session对象相关联，并在响应中设置cookie。以上一切都是容器自动完成的。总之，使用语句（1）容器就会返回一个可直接使用session对象，其他编程人员无需关注（不代表可以不理解这些过程）。

<strong>URL</strong><strong>重写</strong>

虽然大多数浏览器都支持cookie，但是如果万一不支持cookie的话，容器也可以使用URL重写来实现会话管理。

URL重写的概念就是在动态生成页面的时候将会话ID添加到所有的URL后面，这样就可以作为一个参数在用户的下次请求中被服务器获得。

一般容器在第一次为用户创建session对象时，同时使用cookie和URL重写，如果容器发现客户端支持cookie，在以后的交互中就不再使用URL重写了。

注：容器创建会话并不是在用户第一次请求的时候，而是在用户第一次请求需要使用会话的时候。即用户的请求的servlet中调用了request.getSession();

&nbsp;

<strong>删除会话</strong>

当用户离开时，会话对象仍占用内存，而容器无法知道用户是否离去。因此容器一般根据超时撤销会话对象。也可以调用绘画对象的invalidate()手动撤销。

超时的时间一般默认为20分钟，可以设置。

&nbsp;

<strong>Cookie</strong><strong>的其他作用</strong>

Cookie原先设计是为了支持会话状态，但是也可以用其完成其他工作。默认的，cookie寿命和会话寿命一样，当客户离开浏览器时，cookie就会消失。支持会话ID的cookie就是这样子的。

但是cookie作为键值队可以保存在本地，即使浏览器关闭也不撤销并保存一段时间。当用户下次访问网站时，同时将这些cookie发送给服务器。比如网站上的下次自动登录就是这样实现的。

使用的例子如下：

//服务器端设置cookie返回给客户端

Cookie cookie=new Cookie(“username”,name);//创建cookie

cookie.setMaxAge(30*60);//设置cookie在客户端生存多久。单位：秒

response.addCookie(cookie);//将cookie设置在响应对象里以返回给客户端。

//当客户端提出一个请求并将保存的cookie顺便发送给服务器

Cookie[] cookies=request.getCookies();

for(int i=0;i&lt;cookies.length;i++)

{

Cookie cookie=cookies[i];
<p style="padding-left: 30px;">If(cookie.getName.equals(“username”)){</p>
<p style="padding-left: 60px;">String username= cookie.getValue();</p>
<p style="padding-left: 30px;">break;</p>
<p style="padding-left: 30px;">}</p>
}
