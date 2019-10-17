---
layout: post
title: "关于Adminer登录时乱码的问题"
subtitle: "Adminer登录乱码，phpMyAdmin报错#2002，Navicat报错代码10038"
author: "Twany"
header-img: "img/bg-scene/psot-bg-nine.jpg"
header-mask: 0.4
catalog: true
tags:
  - Mysql
---

# 一个很恶心的数据库连接问题


***

今天下午，打开电脑。照常的打开了php工具箱，结果，出现了惊喜：
<img class="shadow" src="/img/in-post/mysql-mess/phpStudy.png" width="260">


## 开始查错：
***

### 1.打开Adminer
<img class="shadow" src="/img/in-post/mysql-mess/adminer.png">


#### 解决：==下载Chorme的charset插件==

#### 结果：**依旧无法更改编码**
***

### 2.打开phpMyAdmin
<img class="shadow" src="/img/in-post/mysql-mess/phpMyAdmin.png">

#### 解决： ==百度错误代码#2002==

修改phpMyAdmin/libraries”文件夹下的config.default.php文件内的

`$cfg['Servers'][$i]['host'] = 'localhost';·`

改为

` $cfg['Servers'][$i]['host'] =‘127.0.0.1’';` 

#### 结果：**无效**

***
### 3.打开Navicat for mysql
<img class="shadow" src="/img/in-post/mysql-mess/navicat.png">

#### 解决： ==百度错误代码10038==
Windows安全规则增加了一个3306新端口

#### 结果：**无效**

***

> 于是，打开cmd，试图通过命令行连接数据库

### 操作过程：

#### 1. 进入到mysql下的bin文件夹 
`cd F:\myphp_www\PHPTutorial\MySQL\bin`

此时遇到了一个小坑---路径不提示更改！
<img class="shadow" src="/img/in-post/mysql-mess/cmd.png">

继续百度：
输入 `F:` ok了！


#### 2.连接数据库
`mysql.exe -h 127.0.1 -u root -p`

根据提示：输入数据库密码root

提示：

`
190219 17:52:12 [Note] --secure-file-priv is set to NULL. Operations related to importing and exporting data are disabled
190219 17:52:12 [Note] mysqld.exe (mysqld 5.5.53) starting as process 9408 ...
`

ok,又提取到一个错误代码：百度：`mysqld.exe (mysqld 5.5.53) starting as process 9408`
出现了：
<img class="shadow" src="/img/in-post/mysql-mess/solve.png">

## 大功告成！

我的理解是自己之前不小心更改了系统的某个设置，使服务器指向错误。

然后去百度`bind-address=127.0.0.1`作用为使本地访问

如果把项目拿到线上，则应该把这个注释掉，使不再只允许本地访问，外网可以访问。

感谢[jecz](https://www.cnblogs.com/jecyhw/p/3512504.html)

## 总结：
一直以来，每当遇到层级比较浅的问题，是可以通过时间去解决的。而如果遇到一些底层的问题，就真的是望洋兴叹了。当接触的越多，越发的发掘底层基础的重要性，不说了，去学离散了~~~


## 小甜点
#### 1. Markdown支持HTML

以导入图片为例子

<img class="shadow" src="/img/in-post/mysql-mess/img.png">



则会产生本篇博文banner图,方便之处在于可以随便调整样式

#### 2. 一个Markdown编辑器和图床

[Markdown编辑器](https://ch957869975.github.io/md-editor/dist/)

里面的上传图片可以当做图床用，看样子是新浪的，应该很稳定。






