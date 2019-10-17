---
layout: post
title: "Hux博客详解"
subtitle: "Hux博客配置详解"
author: "Twany"
header-img: "img/bg-person/post-bg-rwd.jpg"
header-mask: 0.4
catalog: true
tags:
  - 博客
---


> Hux的博客是 jekyll`的一套模板，托管在 `GitPages` 上

[Hux博客 GitHub地址](https://github.com/Huxpro/huxpro.github.io)

上述的GitHub是Hux的博客，他提供了一套纯净的模板，我们 `Clone` 时最好选择这个，避免删除原博客内容。地址为：[Hux模板——纯净版](https://github.com/Huxpro/huxblog-boilerplate)

模板样式：

![](https://i.loli.net/2019/10/17/jB4VXLU1OneIwth.png)

具体如何将这套模板放在自己的gitpage上，此处不再赘述，百度会有一大堆详细教程

本篇博客将具体讲解这套模板的文件配置以及如何配置这套模板



因为我的博客功能比较全面，所以以我的为例进行讲解

![](https://i.loli.net/2019/10/17/e5tny8kHO1Uhgou.png)

## 1. 目录结构

```html
- twany.github.io	
	- _includes
		... (页面包含组件，如header,footer，其中ABOUT页面的中英文内容也在)
	- _layouts （在 1.1 详细讲解）
		... (布局格式，有四种：defaul, keynote, page, post)
	- _posts   （在 1.2 详细讲解）
		... (博客文章，详解见)
	- css
		... (css文件)
	- font
		...	(字体文件)
	- img
		... (图片资源，包括文章需要的标题图等)
	- js
		... (js文件)
	- less
		...	(less文件)
	- portfolio
		...	(Portfolio页面，用来展示自己的作品 你可以把这个文件夹理解为一个独立的页面，它有自己的 css,js,img资源，当然 也可以使用公共资源)
	- pwa
		...	(手机APP配置文件)
	- _config.yml	(核心配置与文件，详细配置见https://github.com/Huxpro/huxpro.github.io/blob/master/README.zh.md)
	- .gitignore (git上传文件，不用管)
	- 404.html	(404界面)
	- about.html	(ABOUT界面)
	- archive.html 	(ARCHIVE界面)
	- CNAME	(域名映射，初始为 https://xxxx.github.io)
	- feed.html	(配置文件，不用管)
	- Gruntfile.js	(配置文件，不用管)
	- index.html (首页)
	- knowTree.md	（KNOWTREE页面）
	- LICENSE	（开源许可）
	- offline.html	（离线阅读界面）
	- package.json	（打包配置信息）
	- REAMDE.md
	- sw.js	(配置文件，不用管)

```



#### 1.1 _layout 文件夹详细讲解

_layout内有四种布局格式文件，分别适用于不同场景

- defalut

  默认格式，以一篇Markdown文章为例：

  ![](https://i.loli.net/2019/10/17/YFciUJMVznsEHtB.png)

- keynote

  幻灯片格式

  ![](https://i.loli.net/2019/10/17/KyTj3haoAHcrU5Y.png)

  其主要原理是添加一个 `iframe`，在里面加入外部链接。你可以直接写到头文件里面去，详情请见下面的yaml头文件的写法。

  ```html
  ---
  layout:     keynote
  iframe:     "http://huangxuan.me/js-module-7day/"
  ---
  ```

- post

  正确的文章打开姿势：

  ![](https://i.loli.net/2019/10/17/XhQwNian5Tly2qI.png)

  一般写文章都用这个格式

- page

  page是首页的格式，含有左侧信息栏

  ![](https://i.loli.net/2019/10/17/lBVOMxi2LhAWjEN.png)

#### 1.2 _posts 文件夹详细讲解

_posts内存放博客文章，要按照固定格式来，否则无法提交

##### 博客命名

![](https://i.loli.net/2019/10/17/UkTveZhXpc1bjVQ.png)

按照这种格式来进行命名：一定不能出现中文，不支持GBK编码，默认 UTF-8

##### 博客格式

![](https://i.loli.net/2019/10/17/xid1ZKCO8sWkaHm.png)

- layout 为 1.1 的四种布局，文章为 post 格式

- title 文章标题

- subtitle 文章简述

- author 作者

- catelog 是否开启右侧目录（true false） （此处未写）

- header-img 头图片

- header-mask 灰度 0为原图，1为全黑（应该是为了突出标题，因为标题字体的颜色为白色）

- tags 标签名，用与检索文章 ，可多个

- 博客内容 推荐Markdown编写，支持HTML，css，JavaScript

  ![](https://i.loli.net/2019/10/17/kg5eDxQnVSRF9Jc.png)

#### _config.yml

配置文件的配置可参考[作者博客](https://github.com/Huxpro/huxpro.github.io/blob/master/README.zh.md)



