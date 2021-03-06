---
title: 一个hugo+github pages静态博客是如何搭成的
date: 2020-04-20T18:25:03+08:00
draft: false
tags: ["blog","hugo","html"]
categories: ["找乐子"]
series: ["博客搭建", "踩坑"]
description: "注意！这篇文章并不是一个教程，只是把我在搭建博客的过程和中间遇到的问题记录一下。"
---

## 写在前面

------

**注意！这篇文章并不是一个教程，只是把我在搭建博客的过程和中间遇到的问题记录一下。**

------

其实之前也搭过一个静态博客，搭了两次，使用hexo框架。本来是想在博客上写点题解，记录一下自己在刷题和学习过程中遇到的坑和一些生活上面的感想，于是有了[Hello-World-again](https://masterenlu.github.io/blogSite/2019/03/hello-world-again/)这篇文章~~并不算~~。后来18年寒假进了技术部之后没了压力和方向，在加上本身懒到一种极致~~没有课绝对不会出宿舍门~~，刷题啥的也就慢慢落了下来，后来一直到现在没写过一道题，在原来博客里更新的几篇题解也全都转到了CSDN上，之后就没再管过博客的事情。

直到昨天，想用github上的一个项目开发一个东西，想了想用qt写了个图形界面，然后在实现功能过程中疯狂吃瘪，甚至一些之前踩过的坑都全忘了，又踩了一遍。于是我痛定思痛，决定以后及时更新博客，把自己踩过的坑全记下来防止再踩一遍。

于是我想起了被遗忘的博客，然后测试了一下还能不能用，然后就有了下面的情况

![image-20200420185908446](https://masterenlu.github.io/blogSite/img/image-20200420185908446.png)

进博客看了看自己仅有的两篇博~~shui~~客~~wen~~，决定从头开始搭一个新的博客，顺便把这篇作为新博客的开端，这才有了现在这篇文章。

## 关于hugo

为什么不再使用hexo作为新博客的框架了呢？一是因为hugo使用的go语言以后可能会学，现在整一下环境完事。第二个就是感觉我跟hexo水土不服（并不），这次换一个试试。

## hugo与go的安装

hugo按照[官方文档](https://gohugo.io/getting-started/installing)走就可以。

这里我看到最后才发现可以不下载chocolatey工具，直接下载hugo开源的发布版，解压缩后把添加<code>hugo/bin</code>到<code>path</code>环境变量就行了。

go的安装直接按照[官方文档](https://golang.org/doc/install)走，下载一个安装包一步到位。

安装完成之后如果要验证的话需要重启命令行，使用 `hugo help` 验证hugo的安装，`go` 来验证go的安装

## 选择主题并生成网页

然后创建要部署博客的文件夹，`hugo new site floderName` 就可以看到新部署的文件夹。

然后在[官网](https://themes.gohugo.io)上选择一个主题下载下来，解压缩到 `theme` 文件夹内，然后把目录里的配置文件复制到博客根目录。

这里主题配置文件可以是 `toml、yaml` 和 `yml` 三种格式

如果是 `toml` 格式可以直接复制内容到根目录的 `config.toml` 文件中，否则需要复制原配置文件里的内容到主题的配置文件，然后把原配置文件删除。注意配置内容格式，我选择的主题使用的是 `yml` 文件。

在新文件夹内执行 `hugo new posts/bulabula.md` 就可以在文件夹内的 `content/posts` 里看见生成的md文件。

执行 <code>hugo -D</code> 来部署静态网页，之后 <code>hugo server</code> 可以打开本地服务器预览效果。

![image-20200420221834299](https://masterenlu.github.io/blogSite/img/image-20200420221834299.png)

## 利用github仓库和github pages

首先需要在github上新建一个没有 `readme` 和 `lisence` 的空仓库，把博客的文件夹初始化并 `push` 到仓库中

这个时候按照[官方文档](https://gohugo.io/hosting-and-deployment/hosting-on-github/)来走，官方给了三个选择：

> [Deployment of Project Pages from `/docs` folder on `master` branch](https://gohugo.io/hosting-and-deployment/hosting-on-github/#deployment-of-project-pages-from-docs-folder-on-master-branch)
>
> [Deployment of Project Pages From Your `gh-pages` branch](https://gohugo.io/hosting-and-deployment/hosting-on-github/#deployment-of-project-pages-from-your-gh-pages-branch)
>
> [Deployment of Project Pages from Your `master` Branch](https://gohugo.io/hosting-and-deployment/hosting-on-github/#deployment-of-project-pages-from-your-master-branch)

第一个将在 `master` 分支下的 `/docs` 子文件夹部署网站，第二个是新建 `gh-pages` 分支来部署网站，第三个就是使用新仓库来部署网站。

第二种和第三种比较接近，当提交更改时需要提交两次，相比之下第一种只需要提交一次就可以。这里按照顺序选择了第一种方法~~并不是懒~~。

第一种方法需要更改 `hugo` 渲染网页的目录，默认是 `public` 目录，需要我们手动将 `config` 中的发布目录更改成 `docs`

```yml
publishDir: docs
```

* 如果是 `toml` 文件格式为

```toml
publishDir = "docs"
```

更改完文件之后重新在本地部署一次，确认有 `docs` 目录了之后直接一波git三连

```bash
$ git add .
$ git commit -m ""
$ git push -u origin master
```

完了之后打开仓库中的 `Settings` 往下找到 `Github Pages`，把 `Source` 一项改成 `master branch /docs floder`

![image-20200420225945814](https://masterenlu.github.io/blogSite/img/image-20200420225945814.png)

那么 `public` 文件夹实际上就不再需要了，为了避免麻烦，直接在 `.gitignore` 里加上 `\public` 以在 `push` 的时候不再更新 `public` 文件夹。

接下来要等一段时间。

今天就先到这，溜了，都11丶了，~~该摸了~~。

## 一个玄学问题

按理说按照之前的步骤建好github pages之后直接访问域名就可以看到网站了，今天早上打开之后发现是这个样子的

![image-20200421093612212](https://masterenlu.github.io/blogSite/img/image-20200421093612212.png)

然后在我一顿操作之后突然就好了，至今不清楚到底是因为啥。

## 关于图片插入的坑

在写这篇文章的时候，插入了几个图片。由于我不想使用图床，直接把要插入的图片放在 `static` 文件夹里，然后按照官方文档给的格式使用相对路径插入图片。

> By default, the `static/` directory in the site project is used for all **static files** (e.g. stylesheets, JavaScript, images). The static files are served on the site root path (eg. if you have the file `static/image.png` you can access it using `http://{server-url}/image.png`, to include it in a document you can use `![Example image](/image.png) )`.

那么问题来了，使用这种相对路径插入的时候本地是识别不出来的，md文件在 `content\posts` 文件夹，图片在 `static` 文件夹，所以在本地查看的时候需要按照完整的相对路径查看，但是在发布到page上时又要更改过来，很麻烦。

而且使用官方的相对路径，要求**发布的网页必须在 `public` 文件夹里**，`docs` 文件夹不生效，原因未知。

最后还是选择了图床的模式~~其实是懒得看官方文档了~~，把仓库当作图床，采用 `url =  baseURL/img/image` 的模式。

## 主题的问题

上面提到过需要更改根目录下的配置文件，但是在配置文件内引用的资源文件没有被更改。需要进入主题文件夹内的 `static` 和 `assets` 文件夹内更改资源文件为自己想要的，比如更改头像。

## 目录

在配置文件中配置

```yml
config:
	toc: true
```

以打开目录，但是默认是关闭的，可以更改 `themes\pure\layouts\partials\sidebar-toc.html` 里第二行为

```html
<aside collapse: class="sidebar sidebar-toc collapse in" id="collapseToc" itemscope itemtype="http://schema.org/WPSideBar">
```

就可以默认开启目录

> ✅4月22日更新
> 这个主题的手机网页支持就离谱，无法显示右侧边栏，目录是以侧边弹出窗口的形式显示的，还是默认关闭比较好

## 开启评论

因为之前用的也是gitalk，所以这次就直接搬过来了。然后就出现了问题：

![image-20200421162759418](https://masterenlu.github.io/blogSite/img/image-20200421162759418.png)

 查了一下是 repo 这个选项配置错误了，直接写仓库名称

![image-20200421164035476](https://masterenlu.github.io/blogSite/img/image-20200421164035476.png)

成功~

## 修改版权声明

自带的版权声明下面有个个人简介，其实跟左上角重复了，决定把它关掉。

找到 `themes\pure\layouts\partials\post\copyright.html`，第14行到第30行都是个人简介，可以直接删掉，不想删掉的话可以更改第14行的 `enabled` 为 `disabled`。

在4-6行添加

```html
<li>
      <strong>原创内容，转载请注明出处</strong>
</li>
```

修改版权声明

> ✅4月22日更新
> 如果想要在手机端显示原文链接需要把第7行的 `hidden-xs` 去掉

## 增加一个留言板

看了一下左侧栏的布置，其实就是一个单独的md文件。似乎作者并没有写增加新目录的功能，但是好在左右两个侧边栏都有目录和标签，造成了重复，这样我们就可以把左侧边栏里的目录和标签去掉，换上留言板，由于留言板的设置与关于一样，这里我就把留言板和关于设置在一起了。

首先在 `config` 文件里找到

```yml
menu:
	main:
		- identifier: home
		  name: Home
		  title: home
		  url: /
		  weight: 1
		...
```

将之后的三、四项删除，将第五项的 `title` 改成 `留言板&关于`，`weight` 改成 `3`

并且在 `content` 目录下创建 `about.md`  文件，重新渲染网页并点击关于，可以看到md里的内容被改成了html，但是这个时候是没有评论功能的，只能成为一个展示板，没有牌面。

然后在 `content`  目录下新建一个文件夹 `about`，并且把 `about.md` 文件剪切进去，并且将 `config` 中的 `url` 一项改成 `about/about`，把 `params: mainSections` 的作用范围改成 `["posts","about"]` 重新渲染网页就可以看到有评论功能了。

这样做带来的后果就是可以在目录中归档中看到 `about` 中的内容，不过可以通过将  `about` 中的日期提前来尽量减小影响。

## 更改上一篇和下一篇的位置

默认的上一篇是指比较新的文章，感觉有丶扯淡，想要换一下

这里不用修改代码，直接找到语言文件将下一篇和上一篇互换位置就行

在 `themes\pure\i18n\zh.yaml` 里将第5行和第7行互换位置

## 修改右侧边栏

由于我把左侧边栏的目录和标签选项给删去了，现在没有办法直接进入目录和标签的主页，所以需要修改一下右侧边栏的目录和标签。
找到 `themes\pure\layouts\partials\_widgets\category.html` 和 `tag.html`，分别将第二行的

```html
{{ T "widget_categories" }}
{{ T "widget_tags" }}
```
修改为
```html
<a href="{{ "categories" | relLangURL }}">分类</a>
<a href="{{ "tags" | relLangURL }}">标签</a>`
```
 `

## 总结

搭这个博客用了一个晚上，由于第二天课比较多，真正搭完已经是下午了，总的来说还算可以，该有的都有了。

以后必好好写博客~~也许~~
