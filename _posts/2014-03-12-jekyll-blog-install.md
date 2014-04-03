---
layout: post
title:  基于jekyll在github pages安装静态博客
date:   2014-03-12 13:46:11
category: "编程"
tag: "git&github"
---

我之前一直使用wordpress作为博客系统，但是wordpress臃肿的架构让我厌烦，而且由于对PHP不熟悉，无法获得随心所欲的感觉。

此外，一直还怕自己在wordpress上的东西丢失，虽然可以备份，但是备份wordpress实在是复杂，而且天知道会出现什么问题。

考虑再三，决定使用静态页面创建博客并将其放在github pages上面，我认为有以下有点：

- 技术、页面简洁优雅。
- 可以使用markdown写博客。
- 修改博客极其方便，只要修改md文件然后同步即可。（这一点极其重要，温故而知新，对旧的知识进行修改去网站上修改挺麻烦的，而且需要修改本地的md文件）
- 不怕内容丢失，因为本地会存放一个站点。
- 可以将更多的精力和时间放在写博客上，而不是折腾博客网站

当然，也有一些缺点：
- 静态，无法实现复杂的一些功能。


本文的读者需要具备的知识：

- 基本的html语法
- 曾搭建过网站
- 基本的git&github操作

如果你还不会使用github，请看教程：http://www.liaoxuefeng.com/wiki/0013739516305929606dd18361248578c67b8067c8c017b000

##静态博客的运行机制
什么叫静态博客网站。

就是我们存放在网上的文件不是php文件，也不是jsp文件等，动态网站需要根据用户访问运行后台程序为用户产生一个html文件包，而是直接把html放在网上，用户访问直接把html和相关文件传给用户浏览器解析。

什么是github pages。

github为每个用户或者每个project都可以生成一个网站空间，这个网站空间只可以存放静态页面。原本是用于介绍用户信息或者project信息，但是也可以用做一个博客。因为这些网站的本质都是和其他人share思想。

什么是jekyll。

jekyll是一个程序，它可以根据配置将md等文件解析为html文件，同时加上静态模版，使一个个独立的html文件可以构成一个网站。

综上所述，构建静态博客网站的整体思想是：1.使用markdown等编写博客，2.在自己的电脑上使用jekyll转为静态网站页面，3.把这些静态网站页面放在服务器（github pages）上。

幸运的是，github pages支持jekyll，因此如果不需要在本地构建一个静态网站，可以把第2步和第3步合为一步，将jekyll处理前的文件夹push到github pages空间里面，github pages自动会转为静态页面供用户访问。

##安装jekyll
jekyll是用于将md文件解析为一个完整的静态网站，解析结束以后，这个静态网站在本地已经可以访问。

因为github pages支持jekyll，这一步其实是不需要的，我们只要把md文件push到github pages对应的工程里面，github会帮我们解析。

这一步我一直看其他的文章都会写应该怎么安装，但是我觉得这一块写了反而误导广大人民群众。所以，我不把jekyll的安装写到这篇文章里面，大家只要知道github pages背后有一个jekyll支持就好了。

安装jekyll是为了在本地也搭建一个静态博客网站，以后会写怎么做，但是大家现在只要知道：github pages自动将你的md文件转为html文件就好了。

大家还可能会疑问，github怎么知道我们那些文件是需要jekyll解析的，哪些不是呢？因为jekyll的文件夹和文件都以下划线`-`开头，遇到了这种就是用jekyll处理，其他的文件和文件夹就按照静态文件处理。

```

//TODO:jekyll安装步骤

```
##创建一个github pages工程

github pages有两种模式：个人或公司站点，project pages项目站点。

一般我们使用第一个，我这里也只介绍第一个。

###第一步：创建个人站点
注意，这种站点必须符合这样的规则：username/username.github.com；而且，每个用户名下面只能建立一个。

![](/images/2014-3-12-jekyll-blog-install-1.png)

###第二步：设置站点主题
进入资源-setting
![进入资源-setting](/images/2014-3-12-jekyll-blog-install-2.png)

更新你的站点
![](/images/2014-3-12-jekyll-blog-install-3.png)


最后选择主题并发布。

由于我创建工程的时候没有截图，图片来源：http://www.cnblogs.com/purediy/archive/2013/03/07/2948892.html

##创建一个符合jekyll规则的文件夹并同步到github pages工程
我们现在已经在github端创建了一个空间，现在只要在本地创建一个符合jekyll规范的文件夹，并且使其与github pages工程同步。从此以后，你只要在本地写md文件，然后同步到github pages工程上去。

我创建的github pages工程为：git@github.com:shinepengwei/shinepengwei.github.io.git

步骤如下：

1 创建一个文件夹。

2 进入这个文件夹，然后把创建的shinepengwei.github.io工程clone下来：

```
git clone git@github.com:shinepengwei/shinepengwei.github.io.git

```

3 创建一个符合jekyll规范的文件夹。这一步可以通过clone一个现有的github pages博客的文件，clone下来以后，修改下_config.yml的配置参数，然后把文件复制到shinepengwei.github.io里面。

注意：不要把clone下来的工程里面的.git文件夹复制进去。

我们这一步clone下来一个jekyll主题的源文件。

```
git clone git@github.com:pizn/kunka.git
```

_posts文章可以不删除，不影响使用。但是_config.yml配置文件需要修改url参数，代表着网站的url地址。

```
url: http://shinepengwei.github.io/
```

4 同步shinepengwei.github.io工程。通过以上三步，本地的shinepengwei.github.io工程下面应该已经包含了jekyll所需要的所有文件，现在使用git同步上去即可。
```
git add .
git commit -m"my blog"
git push
```

##写博客
写博客我一般使用markdown格式，写完了以后就是md文件。将文件放在_posts文件夹里面，文件名要求为“year-month-day-title.md”。

然后再同步到github的shinepengwei.github.io工程上即可。
