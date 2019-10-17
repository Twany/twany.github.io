---
layout: post
title: "PHP来获取html表单数据"
subtitle: "获取勾选项的ID"
author: "Twany"
header-img: "img/bg-person/post-bg-rwd.jpg"
header-mask: 0.4
catalog: true
tags:
  - php
  - 前端
---

今天在做文章全选的时候遇到这个问题，如何才能获得勾选文章的ID？
直接上代码：

```html
<!doctype html>
<html>
<head>
<meta charset="utf-8">
<title>获取表单数据</title>
</head>
<body>
	<form action="test.php" method="get">
		
		//单选
		<input type="radio" name="radio"  value="1">A
		<input type="radio" name="radio"  value="2">B
		<input type="radio" name="radio"  value="3">C
		
		//文本域
		姓名:<input type="text" name="name">
		
		//复选（因为复选是数组，所以name用"test[]",此处划重点）
        <input type="checkbox" name="test[]" value="1">A
        <input type="checkbox" name="test[]" value="2">B	
	
		<button type="submit">提交</button>
	</form>
</body>
</html>

<?php
$num = $_GET['radio'];

$name = $_GET['name'];

$checkbox = $_GET['test];
echo $num;
echo $name;
print_r($name);
?>
```
### 总结
其实表单提交的方法就是通过PHP来获得数据
重点在form 属性 action 和 method,以及php脚本接受数据。
