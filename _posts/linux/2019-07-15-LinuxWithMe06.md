---
layout: post
title: "跟我学Linux——第六日"
subtitle: "第5章 用户身份与文件权限"
author: "Twany"
header-img: "img/bg-scene/post-bg-seven.jpg"
header-mask: 0.4
catalog: true
tags:
  - Linux
---

> Linux 是一个多用户、多任务的操作系统。
> 所以，对权限很“讲究”

## 用户身份
![](https://i.loli.net/2019/07/15/5d2c1e2bb84e068227.png)

#### 命令
- useradd   创建用户（会创建一个同名的基本用户组）
  ![](https://i.loli.net/2019/07/15/5d2c1e7f0714882415.png)
- groupadd  创建用户组（组分为**基本组**和**扩展组**，前者只有一个，后者为多个）
- usermod   修改用户属性
  ![](https://i.loli.net/2019/07/15/5d2c1f1e5943c26131.png)
- id 用户   查看用户信息
- passwd    修改密码等信息
- userdel   删除用户
  - -r      同时删除用户及对应家目录

<hr>

## 文件权限
#### 文件类型
命令 | 作用
-|-
d | 目录文件 |
l | 链接文件 |
b | 块设备文件 |
c|字符设备文件
p|管道文件

#### 权限分配
![](https://i.loli.net/2019/07/15/5d2c20785c3ce62288.png)
![](https://i.loli.net/2019/07/15/5d2c208b78a4767239.png)

- 对于文件
  - r 读取文件权限
  - w 修改文件权限
  - x 执行文件权限
- 对于目录
  - r 读取目录文件列表权限
  - w 对目录内文件编辑
  - x 进入一个目录的权限
  
<hr>

## 文件的特殊权限
```
chmod 权限 + 文件   =》 修改文件权限
chmod 属性          =》 修改所属组
```

一般我们会经常遇到无法通过单纯的设置权限来解决问题，因为有了以下三种**特殊权限位**（注意，它们是**位**，也就是通过更改9位权限来作用）
- SUID
  - 类比尚方宝剑，作用是让非管理员用户暂时拥有管理员的权利。所以，它应该是通过更改“文件所有者”的权限来作用
  - `chmod u+s 文件名`
  - `rwx` -> `rws`
  - `rw-` -> `rwS`
- SGID
  - 让执行者暂时拥有属组的权限
  - `chmod -R g+s 文件名`
  - `rwx` -> `rws`
  - `rw-` -> `rwS`
  - 设计类似于SUID，但不同的是更改的是属组
- SBIT
  - “粘滞位”，可理解为“保护位”。即确保用户只能删除自己的文件，而不能删除其他用户的文件
  - `chmod -R o+t 目录名`
  - `rwx` -> `rwt`
  - `rw-` -> `rwT`

例：求`rwSr-s-wT`的权限数字表达式

解：
```
原式：  rwSr-s-wT
化简：  rwS / r-s / -wT
还原：  rw- / r-x / -w-
数字：   6  /  5  /  2
特殊权限 4     2     0 (SUID为4，SGID为2，SBIT为1)
得：   7 6 5 2
```

<hr>

## 文件的隐藏属性
权限分三种: 一般权限、特殊权限、隐藏权限

隐藏权限不同于另两种，个人理解为一种额外的扩展的权限。

格式：
- `chattr 参数 文件`    " + " 为增加，" - "为移除
```shell
touch test.txt  //创建test.txt文件
chattr +a test.txt  //增加隐藏属性
rm test.txt     //若删除，删除失败
chattr -a test.txt  //移除
rm test.txt     、、删除成功
```
- `lsattr 参数 文件`    查看隐藏权限
![](https://i.loli.net/2019/07/15/5d2c4efa8798161359.png)

<hr>

## 文件访问列表（ACL）
上述三种权限都有一个特性————“权限是针对某一**类*成员”

所以，我们希望对某一个成员权限进行设置

- `setfacl 参数 用户 文件` 设置ACL
```shell
>setfacl -Rm u:linuxprobe:rwx /root //setfacl 参数 用户：用户名：权限 文件
//此时linuxprobe这个用户就拥有查看/root文件的权限了
```
- `getfacl /root` 查看ACL
  
参数 | 作用
-| -
-R | 针对目录文件
-m | 针对普通文件
-b | 删除ACL
u   | 用户
g   | 组
一般情况下都要加上 `m` ，如上面的例子
 
<hr>

## su和sudo
- `su` 是用来切换用户，我们使用 `su - linuxroot`
- `sudo` 作用是把特定命令的执行权限赋予给指定用户
- `visudo` 配置用户权限（只有root用户有权限使用）
  - 在99行，添加：`linuxprobe ALL=(ALL) ALL`
  - `linuxprobe ALL=NOPASSWD :usr/sbin/poweroff` 执行关机命令不用再输入密码

<hr>

## 图片笔记
![](https://i.loli.net/2019/07/15/5d2c5e759fdf685714.jpg)
![](https://i.loli.net/2019/07/15/5d2c5e7687f0129565.jpg)
![](https://i.loli.net/2019/07/15/5d2c5e77a11d570890.jpg)
![](https://i.loli.net/2019/07/15/5d2c5e78dcd6d47350.jpg)
![](https://i.loli.net/2019/07/15/5d2c5e79c880916854.jpg)
