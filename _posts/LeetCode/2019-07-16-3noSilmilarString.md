---
layout: post
title: "3. 无重复字符的最长子串"
subtitle: "LeetCode 3. 无重复字符的最长子串"
author: "Twany"
header-img: "img/bg-scene/post-bg-two.jpg"
header-mask: 0.4
catalog: true
tags:
  - 算法
  - LeetCode
---


## 题目描述
给定一个字符串，请你找出其中不含有重复字符的 最长子串 的长度。

示例 1:
```
输入: "abcabcbb"
输出: 3 
解释: 因为无重复字符的最长子串是 "abc"，所以其长度为 3。
```
示例 2:
```
输入: "bbbbb"
输出: 1
解释: 因为无重复字符的最长子串是 "b"，所以其长度为 1。
```
示例 3:
```
输入: "pwwkew"
输出: 3
解释: 因为无重复字符的最长子串是 "wke"，所以其长度为 3。
     请注意，你的答案必须是 子串 的长度，"pwke" 是一个子序列，不是子串。
```

## 思路

#### 暴力法
太过残暴，略过不提

#### 滑动窗口
1. 对于一个字符串，我们创建两个指针 `i` 、`j` 和set集合来存放不同字符串
2. 依次遍历字符串，并检验set集合是否已存在此字符串
   1. 不存在：将此字符串添加到集合中，继续向下遍历。并比较此时最长子字符串的长度
   2. 存在：删除重复元素，i++递加，直至删除重复元素

```java
class Solution {
    public int lengthOfLongestSubstring(String s) {        
        int length = s.length();    //字符串长度
        int ans = 0;    //最长子字符长度
        //滑动窗口
        int i=0;
        int j=0;
        
        //用来存放不同的子字符串
        Set<Character> set = new HashSet<>();
        
        while(i < length && j < length)
        {
            //滑动窗口内 不含有 j元素
            if(!set.contains(s.charAt(j)))
            {
                //添加新元素j
                set.add(s.charAt(j++));
                
                //更新最长子字符长度（将新的ans和旧的对比）
                ans = Math.max((j-i),ans);
            }else{  // 含有 j元素
                set.remove(s.charAt(i++));
            }
        }
        return ans;
    }
}
```
![](https://i.loli.net/2019/09/01/Cmc1yFxonsaJZEh.png)

#### 优化的滑动窗口
为什么排名还是这么靠后呢？一定还有可以大改进的地方。

对上述方法进行时间复杂度分析，可以得出最坏结果为 `o(2n)`

事实上，它可以被进一步优化为仅需要 n 个步骤。我们可以定义字符到索引的映射，而不是使用集合来判断一个字符是否存在。 当我们找到重复的字符时，我们可以立即跳过该窗口。

也就是说，如果 `s[j]`在 `[i, j)[i,j)` 范围内有与 `j'` 重复的字符，我们不需要逐渐增加 `i`。 我们可以直接跳过 [i，j'] 范围内的所有元素，并将 `i` 变为 `j' + 1`

```java
public class Solution {
    public int lengthOfLongestSubstring(String s) {
        int n = s.length(), ans = 0;
        Map<Character, Integer> map = new HashMap<>(); // current index of character
        // try to extend the range [i, j]
        for (int j = 0, i = 0; j < n; j++) {
            if (map.containsKey(s.charAt(j))) {
                i = Math.max(map.get(s.charAt(j)), i);
            }
            ans = Math.max(ans, j - i + 1);
            map.put(s.charAt(j), j + 1);
        }
        return ans;
    }
}
```

![](https://i.loli.net/2019/09/01/YXy5ZxtW97GH4Ia.png)
