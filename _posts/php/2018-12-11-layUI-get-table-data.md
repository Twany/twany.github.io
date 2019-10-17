---
layout: post
title: "获取layui复选框数据"
subtitle: "关于layUI获取表格中复选框对应数据的方法"
author: "Twany"
header-img: "img/bg-scene/home-bg-o.jpg"
header-mask: 0.4
tags:
  - 前端
  - layUI
  - js
---

## 关于layUI获取表格中复选框对应数据的方法
今天在做博客后台项目最后一个功能，就是使用layUI的多选功能进行删除勾选数据
如：
![在这里插入图片描述](https://img-blog.csdnimg.cn/20181211211532468.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80MzIwMjM2NA==,size_8,color_FFFFFF,t_70)
去网上百度了各种方法，无一不是特别麻烦。就想了另外的一种方案：


**复选框用的是layUI的标签：**

```
 <div class="layui-unselect header layui-form-checkbox" lay-skin="primary">
 	<i class="layui-icon">&#xe605;</i>
 </div>
```

**红色批量删除按钮标签：**

```
<button class="layui-btn layui-btn-danger" onclick="delAll()">
	<i class="layui-icon"></i>批量删除
</button>
```

button标签内置一个deall()函数；
下面是deall()代码：

```
      function delAll (argument) {
		  var data = tableCheck.getData();
       		
		  //选中的个数
		  var long = data.length;
		  var arr = [];
		  
		  //给数组赋值选中的id
		  for(var i=0;i<long;i++){
		  	  //children('.td)为选中class为td的值，这个值为我们要选中的id.
			  arr[i] = $(".layui-form-checked").not('.header').parents('tr').children('.td').eq(i).html();
		  }

		
        layer.confirm('确认要删除吗？'+data,function(index){

            layer.msg('删除成功', {icon: 1});
		    //	将数据arr发送给php脚本处理
            $.post('http://blog.com/admin/server/acticleMove.php',{cid:arr},function(res){},'json');
			$(".layui-form-checked").not('.header').parents('tr').remove();
        });
      }
```

## ****总结**：**

	难点在于如何获得到勾选的值
	

```
  arr[i] = $(".layui-form-checked").not('.header').parents('tr').children('.td').eq(i).html();
```
因为数据是遍历出来的，所以arr是一个数组，关键就在于给arr赋值
最开始我用的是：

```
  arr[i] = $(".layui-form-checked").not('.header').parents('tr').children('.td').html();
```
与上处相比少了一个eq(i)，这就导致arr始终是一个值
只有通过遍历才能把每个值都遍历出来并赋给arr数组
（
此处有一个小知识点那就是js中数组的创建：

```
var arr = [];
//或者
var arr = new Array();
```

）

