---
layout: post
title: "9. 回文数 C++版"
subtitle: "LeetCode 9. 回文数"
author: "Twany"
header-img: "img/bg-scene/post-bg-one.jpg"
header-mask: 0.4
catalog: true
tags:
  - 算法
  - LeetCode
---

## 题目概述

- **题号**：[9. 回文数](https://leetcode-cn.com/problems/palindrome-number/)

- **难度**：简单

- **内容**：

  ```
  判断一个整数是否是回文数。回文数是指正序（从左向右）和倒序（从右向左）读都是一样的整数。
  
  示例 1:
  
  输入: 121
  输出: true
  示例 2:
  
  输入: -121
  输出: false
  解释: 从左向右读, 为 -121 。 从右向左读, 为 121- 。因此它不是一个回文数。
  示例 3:
  
  输入: 10
  输出: false
  解释: 从右向左读, 为 01 。因此它不是一个回文数。
  
  来源：力扣（LeetCode）
  链接：https://leetcode-cn.com/problems/palindrome-number
  著作权归领扣网络所有。商业转载请联系官方授权，非商业转载请注明出处。
  ```

  

## 第一次思路

普通的求商求余：

以 12345 为例：

1. 12345 % 10 = 5
2. 12345  / 10 = 1234
3. 0 * 0 + 5 = 5；
4. 1234 % 10 = 4
5. 1234 / 10 = 123
6. 5 * 10 + 4 = 54
7. ...

### Code

```c++
// 定义原数 用于和反转之后的数进行对比
int reversedNum = x;

// 负数直接return
if (x < 0)
    return false;
// pop 为 余数，num 为每次 num * 10 + pop，
int pop = 0,num = 0;

// 循环终止与 x 被除尽
while (x)
{
    // 溢出判断
    if (num > INT_MAX / 10 || (num == INT_MAX / 10 && pop > 7))
        return false;
    if (num < INT_MIN / 10 || (num == INT_MIN / 10 && pop < -8))
        return false;
    pop = x % 10;
    num = num * 10 + pop;
    x /= 10;
}

if (num == reversedNum)
{
    return true;
}
return false;
```



## 测试 Submit

![1573816874732](C:\Users\Mr.Y\AppData\Roaming\Typora\typora-user-images\1573816874732.png)

## 分析

很一般的成绩

看了官方给的标准解答后，发现了更为巧妙的解法

## 改进

官方给出的方法是 **折半对比**，顾名思义，就是把原数从中间拆为两半并对比。

还是对原数 `x` 进行反转，不过这次是只需要进行后一半的反转，然后和前一半进行对比，相同则 说明是回文数

此处要注意的要反转的数的**个数奇偶性**，若是奇数，则最后要用 反转数/10 余数即为中间数，无需对比。

很简单且高妙的解法！

#### 改进Code

```c++
int reversedNum = x;

/* 
	负数直接return
	如果是以0结尾的数，那么其反转数开头也要为0，只有0满足
	*/
if (x < 0 || (x % 10 == 0 && x != 0) )
    return false;

int pop = 0, num = 0, reverseNum = 0;
while (num < x)
{

    pop = x % 10;
    num = num * 10 + pop;
    x /= 10;
    //cout << num << "------" << x <<endl;
}

// 考虑奇数个字符情况
return (num == x) || (num / 10 == x);
```

## 改进Submit

![1573816766481](C:\Users\Mr.Y\AppData\Roaming\Typora\typora-user-images\1573816766481.png)

## 收获总结

每一道题的最巧妙的解法，都是在这个数据结构本身的特性下文章，体用其特质去解，事倍功半啊