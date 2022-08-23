# [剑指 Offer 65. 不用加减乘除做加法](https://leetcode.cn/problems/bu-yong-jia-jian-cheng-chu-zuo-jia-fa-lcof/)

## 题目

写一个函数，求两个整数之和，要求在函数体内不得使用 “+”、“-”、“*”、“/” 四则运算符号。

```
示例:

输入: a = 1, b = 1
输出: 2
```


提示：

- a, b 均可能是负数或 0

- 结果不会溢出 32 位整数

## 思路

方法：位运算

## 代码

```golang
func add(a int, b int) int {
    c := 0
    for b!=0 {	//非进位
        c = a&b << 1	//进位
        a ^= b	//非进位
        b = c	//进位
    }
    return a
}
```

