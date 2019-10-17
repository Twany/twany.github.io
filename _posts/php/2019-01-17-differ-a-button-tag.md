---
layout: post
title: "a标签和button标签"
subtitle: "a标签和button标签的区别（地址跳转）"
author: "Twany"
header-img: "img/bg-scene/psot-bg-two.jpg"
header-mask: 0.4
catalog: true
tags:
  - 前端
---

## 我们以下载一个phpexcel文件为例
首先看，button标签：

html代码
```
 <button class="layui-btn layui-btn-fluid" onclick="download()">下载</button>
```
js代码：

```
		  function download(){
			  $.post("{:url('download')}",{},function(res){
				  if(res.status == 0){
					  layer.msg(res.msg); 
				  }
			  });
		  }
```
（注意此处有一个关于layUI知识点：layUI是js最外层一个总体函数，如果onclick函数定义到这个里面，则显示onclick未定义，只有定义在在外面才可以）
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190117203832535.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80MzIwMjM2NA==,size_16,color_FFFFFF,t_70)

tp对应控制器代码是负责下载

**点击button是显示出下载页面，只有再点击进入这个页面才开始下载！**

而a标签：
html代码：

```
<a class="layui-btn" href="{:url('grade/product')}" style="100%">生成标准模板文件</a>
```
作用是直接跳转到下载页面

## 总结：
	button是时间，不跳转
	a作用是跳转页面
