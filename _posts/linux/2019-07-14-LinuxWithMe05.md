---
layout: post
title: "跟我学Linux——第五日"
subtitle: "第4章 Vim编辑器与Shell命令脚本"
author: "Twany"
header-img: "img/bg-scene/post-bg-nine.jpg"
header-mask: 0.4
catalog: true
tags:
  - Linux
---

## Vim文本编辑器
> Vim的重要性不再赘述

Vim有三种模式：**命令模式**、**输入模式**、**冒号模式**

关系如下图：
![](https://i.loli.net/2019/07/14/5d2b020307bbb36462.png)

切记：**输入模式和冒号模式不能相互转换**

命令模式下命令：
![](https://i.loli.net/2019/07/14/5d2b024abaf2110651.png)

冒号模式下：
![](https://i.loli.net/2019/07/14/5d2b02720be7758535.png)

<hr>

## 编写Shell脚本
#### 基本概念
- **脚本要以`#!/bin/bash`开头**
- `#` 为注释

#### 参数接受
 - `$0` 当前脚本名称
 - `$#` 接受参数个数
 - `$*` 所有参数
 - `$?` 上次命令的执行返回值
 - `$1,$2,...` 第N个参数值

#### 参数判断
格式：**[ 条件表达式 ]**,必须**加空格**
![文件测试所用的参数](https://i.loli.net/2019/07/14/5d2b03edeb25820668.png)
![可用的整数比较运算符](https://i.loli.net/2019/07/14/5d2b03fb9aa5e17921.png)

#### 流程控制语句
**if语句**
```
if [ ... ]
then
    ...

elif
then
    ...

else
    ...
fi
```
![](https://i.loli.net/2019/07/14/5d2b0594cf88d62537.png)

**for循环**
```
for 变量名 in 取值列表
do
    命令序列
done
```

**while循环**
```
while [ ... ]
do
    ...
done
```
![](https://i.loli.net/2019/07/14/5d2b0629cd81852872.png)


**cookie**
- 切记定义常量时，不能加空格： `PRICE=${expr $RANDOM % 10}`
- 如上所示，`expe` 的作用
- `echo $?` 0为true，1位false
- 常量定义是不用加`$`, 但使用时必须要加

## 计划任务服务程序
#### 一次性计划任务
```
> at 23.30
> reboot
# 按Ctrl + D 结束编写
```
以上命令作用是，在23.30重启（一次性）
#### 长期命令
- `crontab -e`  创建计划
- `crontab -l`  查看当前计划
- `crontab -r`  删除计划

**时间周期**
![](https://i.loli.net/2019/07/14/5d2b07cedb23b80839.png)

注意：**分 字段必须有数值**

<hr>

## 图片笔记
![](https://i.loli.net/2019/07/14/5d2b08798a25a35585.jpg)
![](https://i.loli.net/2019/07/14/5d2b087aa76d546331.jpg)
![](https://i.loli.net/2019/07/14/5d2b087b5b40988527.jpg)
![](https://i.loli.net/2019/07/14/5d2b087c068d079946.jpg)
![](https://i.loli.net/2019/07/14/5d2b087c9bb7369079.jpg)
