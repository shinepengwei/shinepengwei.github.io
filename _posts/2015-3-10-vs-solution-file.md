---
layout: post
title:  工程/编译配置文件解析
category: "编程"
tag:  "编译和连接"
---

在vs以及xcode这种IDE，他们的工程文件都包括了编译信息以及相关的配置信息，其实如果不用vs的话，也需要生成一个编译信息文件，如android.mk等。

以[makefile](http://scc.qibebt.cas.cn/docs/linux/base/%B8%FA%CE%D2%D2%BB%C6%F0%D0%B4Makefile-%B3%C2%F0%A9.pdf)为例，makefile关系到整个工程的编译规则。一个工程源文件按照类型、功能、模块存放在若干目录，而makefile定义一系列编译规则。基于makefile可以自动化编译，因为它定义了一系列的编译规则。

而IDE中扩展了makefile功能，以vs为例，sln文件由文件版本、工程信息以及全局设置三部分构成。

**slv文件**

其中最重要的就是工程信息，比如一条工程信息如下：

```
Project("{8BC9CEB8-8B4A-11D0-8D11-00A0C91BC942}") = "N2", "N2.vcxproj", "{4E6A7A0E-DDD8-4BAA-8B22-C964069364ED}"
	ProjectSection(ProjectDependencies) = postProject
		{21B2C324-891F-48EA-AD1A-5AE13DE12E28} = {21B2C324-891F-48EA-AD1A-5AE13DE12E28}
	EndProjectSection
EndProject
```

其中`ProjectSection(ProjectDependencies)`表示工程依赖关系。


**vcxproj**

vcxproj文件是工程文件，描述一个工程的编译、链接及配置信息。

如果我们把一个cpp或者.h文件复制到工程目录，工程是不会自动去编译该文件的，需要我们手动的在vs中添加该文件到工程。

而vcxproj文件中就定义了该工程中包括哪些源文件，在编译的时候就回去编译这些文件。

当我们把文件导入到工程， vcxproj文件就会有这些文件路径的信息。

```
 <ItemGroup>
    <ClCompile Include="..\..\external\ConvertUTF\ConvertUTF.c" />
    <ClCompile Include="..\..\external\ConvertUTF\ConvertUTFWrapper.cpp" />
    <ClCompile Include="..\..\external\edtaa3func\edtaa3func.cpp" />
    <ClCompile Include="..\..\external\tinyxml2\tinyxml2.cpp" />
    <ClCompile Include="..\..\external\unzip\ioapi.cpp" />
    <ClCompile Include="..\..\external\unzip\unzip.cpp" />
    <ClCompile Include="..\..\external\xxhash\xxhash.c" />
    <ClCompile Include="..\3dlayer\CCHorde3DEngine.cpp" />
    <ClCompile Include="..\3dlayer\CCLayer3D.cpp" />
    <ClCompile Include="..\3d\CCMesh.cpp" />
</ItemGroup>
```