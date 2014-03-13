---
layout: post
title: SevletContext、SevletConfig参数和属性
category: "编程"
tag: "JSP"
---
<h2><span style="color: #000000;"><strong>SevletContext vs. SevletConfig参数</strong></span></h2>
<strong>使用原因</strong>：将一些Web应用参数设置在部署文件web.xml中，以后如果需要修改只需要修改部署文件而不需要修改源代码。

<strong>生命周期</strong>：

在Web应用启动servlet初始化时，容器会读取部署文件，并根据部署文件创建sevletConfig和SevletContent，并将读到的参数创建名/值对，同时向sevletConfig和SevletContent提供这些名/值对的引用。

完成以上步骤后容器创建servlet类的实例并在init方法中传入servletConfig引用。而所有的servlet和JSP都可以访问同一servletContext。

由上可知，初始化参数只会读一次，即容器初始化servlet的时候，或者说容器新启动的时候。所以在容器运行过程中，修改部署文件无法修改Web应用，只有重启才行。（也存在热部署方法）

区别：

? 每一个servlet一个ServletConfig而每个Web应用一个ServletContext。

? ServletConfig只能背自己的servlet访问，servlet也只能访问自己的ServletConfig；而应用中所有的servlet和JSP都可以访问ServletContext。

? 部署文件配置代码不同

? Servlet访问代码不同。
<h2><span style="color: #000000;">属性</span></h2>
属性就是一个对象，设置到ServletContext、HttpSession或HttpServletRequest上。

属性存在三种作用域，上下文、会话和请求。

上下文：

? 该作用域中属性在Web应用启动时就存在，直至应用结束。

? Web应用的所有部分都可以访问，包括servlet、JSP，linstener等等。

? 它不是线程安全的。

会话：

? 会话的生命周期从某一用户第一次开始访问Web应用开始，可以通过编程撤销，也可以通过超时自动撤销。

? 每一个会话涉及多个请求，比如用户先查看物品、然后购买物品。这些请求共享相同的会话属性。

? 他也不是线程安全的。因为同一用户可能同时发起多个请求。

请求：

? 请求的生命周期持续到servlet的service方法结束。

? 某一请求相应的servlet和JSP。

? 对于某个请求，如果应用从一个servlet开始，最后以一个JSP视图结束。Servlet和JSP通信时即使用请求属性，并使用RequesDispatcher。

&nbsp;
