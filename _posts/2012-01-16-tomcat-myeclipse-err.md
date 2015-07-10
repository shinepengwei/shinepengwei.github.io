---
layout: post
title: tomcat和MyEclipse不自动编译的解决方法
category: "编程"
tag: "JSP"
---
今天遇到了一些问题：

1.MyEclipse中的jsp文件改变了，自动同步到tomcat中JSP文件，但是还是显示的没有改变的。查看编译JSP后的java文件，发现没有自动编译。也就是说，TOMCAT没有自动变异。

原因：系统时间不小心被我改成前几天的了，而tomcat自动编译是基于文件的修改时间和当前系统时间比较的。

解决方法：修改系统时间。

2.解决完问题（1）发现MyEclipse不能自动同步tomcat了。

解决方法：MyEclipse-&gt;project-&gt;build automatically(~)
