# [剑指 Offer 62. 圆圈中最后剩下的数字](https://leetcode.cn/problems/yuan-quan-zhong-zui-hou-sheng-xia-de-shu-zi-lcof/)

## 题目

0,1,···,n-1这n个数字排成一个圆圈，从数字0开始，每次从这个圆圈里删除第m个数字（删除后从下一个数字开始计数）。求出这个圆圈里剩下的最后一个数字。

例如，0、1、2、3、4这5个数字组成一个圆圈，从数字0开始每次删除第3个数字，则删除的前4个数字依次是2、0、4、1，因此最后剩下的数字是3。

```
示例 1：

输入: n = 5, m = 3
输出: 3
示例 2：

输入: n = 10, m = 17
输出: 2
```


限制：

- 1 <= n <= 10^5
- 1 <= m <= 10^6

## 思路

[解题参考](https://leetcode.cn/problems/yuan-quan-zhong-zui-hou-sheng-xia-de-shu-zi-lcof/solution/javajie-jue-yue-se-fu-huan-wen-ti-gao-su-ni-wei-sh/)

![](https://raw.githubusercontent.com/RobKing9/Blog_Pic/master/Git/20220823150042.png)

## 代码

```golang
func lastRemaining(n int, m int) int {
    //递推 从后往前
    //最后 只有一个值 下标肯定是0
    result := 0
    //上一轮 开始第一次递推 有两个元素
    //我们需要一步一步得到 最后的值 在上一轮的下标，下标和值是一样的
    for i:=2; i<=n; i++ {
        //先往右 移动m位 然后对上一轮元素个数 取模 
        //得到的结果就是 在上一轮的下标
        result = (result+m)%i
    }
    return result
}
```

