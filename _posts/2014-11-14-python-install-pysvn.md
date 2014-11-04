---
layout: post
title:  一段不愉快的经历——Windows下安装pysvn
category: "编程"
tag:  "Python"
---


==
今天因为项目需要，第一次使用python的第三方库pysvn，从早晨开始安装pyvsn，到现在才装上，中间走错了很多方向。

1. windows版和linux版搞混了
2. 使用easy_install安装。
2. 下了个源代码版本，并尝试去编译


先说最终的解决方案：

去http://pysvn.tigris.org/servlets/ProjectDocumentList?folderID=1768 下载，根据安装的python版本和svn版本下载exe文件，即release文件。安装，即可！

注意，如果使用的是64位的python，pysvn安装文件会提示找不到python。换一个32位的就可以了。

坑爹有没有！！！展示下成果：

![Alt text](./2014/104d4a93-31b3-455c-949e-3a4fb09a094b.png)


使用easy_install直接下载安装python第三方库的方法，在第三方库注册机构没找到pysvn。

http://www.liaoxuefeng.com/wiki/001374738125095c955c1e6d8bb493182103fac9270762a000/0013868200214529634268c5b3b45b3a3ba1cd81a251a3b000



说下编译pysvn源代码时遇到的问题。

INSTALL.html的说明如下：

```
    Building on win32
    
    PYSVN sources have been updated to build on Windows 7 64bit version. Expect issues on older windows and 32bit windows.
    
    These instructions assume you have Microsoft Visual C++ 2008 (9.0) to compile the code and INNO 5 to create the installation kit.
    
    You must build with MSVC 2008 (9.0) for Python 2.6 or later and python 3.0 or later.
    
    Build subversion (tested with and svn 1.7.11 and 1.8.1)
    Fetch and expand the pysvn source code into extdir
    Expand pycxx-6.2.4.tar.gz into extdir\Import if not using a source kit
    Edit Builder\builder_custom_init.cmd to match the locations of the sources.
    cd Builder
    builder_custom_init.cmd
    cd ..\Source
    python setup.py configure ... (add any configure options required to make it find the required libraries).
    nmake
    cd ..\Test
    nmake
    cd ..\Kit\Win32-1.7
    nmake
    To install the built kit
    
    Uninstall any previous kit (control panel's Add/Remove programs)
    nmake -f win32.mak install
```

执行到`python setup.py configure`提示各种找不到svn中的文件，如libsvn_client-1.lib等，后来发现我安装的svn是subversionWindows 的release版本，要下一个开发版本： svn-win32-1.8.10_dev.zip，里面有需要的各种代码。网址：http://alagazam.net/
此步成功。

然后执行到nmake， 编译出现错误   “cl.EXE: 返回代码 0xc0000135”

这是环境变量的问题

解决方案是在使用nmake之前运行 vcvarsall.bat，这个必须在cmd窗口内运行，因为运行后只是临时改变当前的session内的环境变量。

然后再执行 nmake /F MakeFile

另外：nmake位置 d:\program files\Microsoft Visual Studio 10.0\VC\bin\vs的安装目录，需要加到环境变量Path中

然后，有提示错误：说unixTool啥错误，总之需要windows下的unix环境工具包。

我一看，这条路肯定是不对的了..

所以又去查其他的解决方案。





