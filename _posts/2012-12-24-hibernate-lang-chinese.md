---
layout: post
title: Hibernate中文编码问题
category: "编程"
tag: "JSP"

meta:
  _aioseop_description: 为解决Hibernate的中文编码问题，需要在三个地方设置，分别是JSP页面，Hibernate框架以及数据库中。
---
<span style="color: black; font-size: 13pt;"><span style="font-family: 宋体;">为解决</span><span style="font-family: Tahoma;">Hibernate</span><span style="font-family: 宋体;">的中文编码问题，需要在三个地方设置，分别是</span><span style="font-family: Tahoma;">JSP</span><span style="font-family: 宋体;">页面，</span><span style="font-family: Tahoma;">Hibernate</span><span style="font-family: 宋体;">框架以及数据库中。</span><span style="font-family: Tahoma;">
</span></span>

<span style="color: black; font-size: 13pt;"><span style="font-family: Tahoma;">JSP</span><span style="font-family: 宋体;">设置</span></span><span style="color: #bf5f3f; font-family: Consolas; font-size: 10pt; background-color: #e8f2fe;">&lt;%@</span><span style="color: black; font-family: Tahoma; font-size: 13pt;">?</span><span style="font-family: Consolas; font-size: 10pt;"><span style="color: #3f7f7f; background-color: #e8f2fe;">page</span><span style="color: #010101; background-color: #e8f2fe;">?</span><span style="color: #7f007f; background-color: #e8f2fe;">language</span><span style="color: black;">?<span style="background-color: #e8f2fe;">=</span></span><span style="color: #2a00ff; background-color: #e8f2fe;">?<em>"java"</em></span><span style="color: #010101; background-color: #e8f2fe;">?</span><span style="color: #7f007f; background-color: #e8f2fe;">import</span><span style="color: black;">?<span style="background-color: #e8f2fe;">=</span></span><span style="color: #2a00ff; background-color: #e8f2fe;">?<em>"java.util.*"</em></span><span style="color: #010101; background-color: #e8f2fe;">?</span><span style="color: #7f007f; background-color: #e8f2fe;">pageEncoding</span><span style="color: black;">?<span style="background-color: #e8f2fe;">=</span></span><span style="color: #2a00ff; background-color: #e8f2fe;">?<em>"utf-8"</em></span><span style="color: #bf5f3f; background-color: #e8f2fe;">%&gt;</span></span><span style="font-family: 宋体; font-size: 12pt;">
</span>

<span style="color: black; font-size: 13pt;"><span style="font-family: Tahoma;">hibernate</span><span style="font-family: 宋体;">设置：</span></span><span style="color: teal; font-family: Consolas; font-size: 10pt; background-color: #e8f2fe;">&lt;</span><span style="color: black; font-family: Tahoma; font-size: 13pt;">?</span><span style="color: #3f7f7f; font-family: Consolas; font-size: 10pt; background-color: #e8f2fe;">property</span><span style="color: black; font-family: Tahoma; font-size: 13pt;">?</span><span style="color: #7f007f; font-family: Consolas; font-size: 10pt; background-color: #e8f2fe;">name</span><span style="color: black;"><span style="font-family: Tahoma; font-size: 13pt;">?</span><span style="font-family: Consolas; font-size: 10pt; background-color: #e8f2fe;">=</span></span><span style="color: #2a00ff; font-family: Consolas; font-size: 10pt; background-color: #e8f2fe;"><em>"url"</em></span><span style="color: black; font-family: Tahoma; font-size: 13pt;">?</span><span style="font-family: Consolas; font-size: 10pt;"><span style="color: #7f007f; background-color: #e8f2fe;">value</span><span style="color: black; background-color: #e8f2fe;">=</span><span style="color: #2a00ff; background-color: #e8f2fe;"><em>"jdbc:mysql://localhost:3306/giftr?useUnicode=true&amp;amp;characterEncoding=UTF-8"</em></span></span><span style="color: black; font-family: Tahoma; font-size: 13pt;">?</span><span style="color: teal; font-family: Consolas; font-size: 10pt; background-color: #e8f2fe;">/&gt;</span><span style="color: black; font-family: Tahoma; font-size: 13pt;">
</span>

<span style="color: black; font-size: 13pt;"><span style="font-family: Consolas; background-color: #e8f2fe;">MYsql</span><span style="font-family: 宋体; background-color: #e8f2fe;">里面表和列需要设置。</span><span style="font-family: Tahoma;">
</span></span>

<img src="/wp-content/uploads/2013/01/010413_0748_hibernate1.png" alt="" /><span style="color: black; font-family: Tahoma; font-size: 13pt;">
</span>

<img src="/wp-content/uploads/2013/01/010413_0748_hibernate2.png" alt="" /><span style="color: black; font-family: Tahoma; font-size: 13pt;">
</span>
