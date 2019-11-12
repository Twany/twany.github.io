---
layout: post
title: "7. 整数反转 C++版"
subtitle: "LeetCode 7. 整数反转"
author: "Twany"
header-img: "img/bg-scene/post-bg-two.jpg"
header-mask: 0.4
catalog: true
tags:
  - 算法
  - LeetCode
---


## 题目概述

- **题号**：[1. 两数之和](https://leetcode-cn.com/problems/two-sum/)

- **难度**：简单

- **内容**：

  ```
  给出一个 32 位的有符号整数，你需要将这个整数中每位上的数字进行反转。
  
  示例 1:
  
  输入: 123
  输出: 321
   示例 2:
  
  输入: -123
  输出: -321
  示例 3:
  
  输入: 120
  输出: 21
  注意:
  
  假设我们的环境只能存储得下 32 位的有符号整数，则其数值范围为 [−231,  231 − 1]。请根据这个假设，如果反转后整数溢出那么就返回 0。
  
  来源：力扣（LeetCode）
  链接：https://leetcode-cn.com/problems/reverse-integer
  著作权归领扣网络所有。商业转载请联系官方授权，非商业转载请注明出处。
  ```

  

## 第一次思路

直接循环求商求余，反向乘！

要做此题，最基本的就是对 `%` 和 `/` 以及数的位关系的理解。

- 以365为例，第一次求余数：`365%10 = 5`，那么这个5就应该是反转数的首位。
- 将`365/10 = 36`,再次`36%10 = 6`，这次我们应该 把 5 和 6 组建成 56，也就是`5*10 + 6 = 56`
- 将`36/10 = 3`,再次`3%10 = 3`，这次我们应该 把 56 和 3 组建成 563，也就是`56*10 + 3 = 563`

由上述过程我们可以总结归纳出，每次先求出`原数的10的余数`，然后将`原数除以10得到商`，结果和余数相乘并加上商，就是我们要的反转数

### Code

```c++
int reverse7(int x)
{
	int revNum = 0;
	int pop = 0;
	while (x != 0)
	{
		pop = x % 10;
		revNum = revNum * 10 + pop;
		x /= 10;
	}

	return revNum;
}
```



## 测试 Submit

![](https://i.loli.net/2019/11/12/mIag1GBik82KOMn.png)

## 分析

看报错信息可以晓得：**溢出了** ，题中给出了要求：假设我们的环境只能存储得下 32 位的有符号整数，则其数值范围为 [−2^31,  2^31 − 1]。请根据这个假设，如果反转后整数 **溢出那么就返回 0**。

## 改进

此处我们要注意溢出的问题

当 `temp=rev⋅10+pop` 时会导致溢出。

幸运的是，事先检查这个语句是否会导致溢出很容易。

为了便于解释，我们假设 rev 是正数。

- 如果 `temp=rev⋅10+pop` 使得溢出，那么一定会有 `rev > INTMAX/10`;

至于 `7` 和 `-8` 是怎么产生的？

以 7 为例：题中要求的最大值是 `2^31 − 1`,这是int类型的最大值，为 `2147483648`,当 `rev = INTMAX/10`，此时 `rev = 2147483640`,在加上8就溢出了，所以进行控制。

#### 改进Code

```c++
int reverse7(int x)
{
	int revNum = 0;
	int pop = 0;
	while (x != 0)
	{
		pop = x % 10;
		revNum = revNum * 10 + pop;
		x /= 10;

		// 溢出判断
		if (revNum > INT_MAX / 10 || (revNum == INT_MAX / 10 && pop > 7))
			return 0;
		if (revNum < INT_MIN / 10 || (revNum == INT_MIN / 10 && pop < -8))
			return 0;
	}

	return revNum;
}
```

## 改进Submit

![](https://i.loli.net/2019/11/12/ulmBvt6HSbIGqKQ.png)

## 收获总结

题很简单，需要注意的是溢出问题。而判断是否溢出有需要去考虑关键位的大小，很细节