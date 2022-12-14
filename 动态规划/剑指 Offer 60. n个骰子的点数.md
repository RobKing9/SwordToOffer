# [剑指 Offer 60. n个骰子的点数](https://leetcode.cn/problems/nge-tou-zi-de-dian-shu-lcof/)

## 题目

把n个骰子扔在地上，所有骰子朝上一面的点数之和为s。输入n，打印出s的所有可能的值出现的概率。

你需要用一个浮点数数组返回答案，其中第 i 个元素代表这 n 个骰子所能掷出的点数集合中第 i 小的那个的概率。

```golang
示例 1:

输入: 1
输出: [0.16667,0.16667,0.16667,0.16667,0.16667,0.16667]
示例 2:

输入: 2
输出: [0.02778,0.05556,0.08333,0.11111,0.13889,0.16667,0.13889,0.11111,0.08333,0.05556,0.02778]
```


限制：

- 1 <= n <= 11

## 思路

方法：动态规划

主要思路

- dp[i] 的定义：n个骰子 所有值出现的概率

- 初始化：n为1的时候，意味着 1-6 出现的概率都为 1/6。此时 dp[1] = [1/6, 1/6, 1/6, 1/6, 1/6, 1/6]

- 状态转移：之后从第二颗骰子开始，每次增加一颗；用一个temp[i] 保存当前的值，含义和dp[n] 一样，因为 第 i 个骰子的取值范围是 i-6i，取值的数量是 5i+1，我们就可以初始长度。

- 之后第 i-1 颗骰子的每个值都会对 第 i 颗骰子的值产生影响，一共有 len(dp) 也就是上一个的所有值。我们使用的是下标，所有每个值影响的 是 第i个骰子的六个值，从0-6。

  比如说 dp[1] = [1/6, 1/6, 1/6, 1/6, 1/6, 1/6]；那么当我们遍历到第二颗骰子的时候，temp[j] 相等于表示的是第一颗骰子的概率值，j每次取值的时候，k从0-6意味着影响这六个值

- 每次都要重新更新dp，代表的是上一颗骰子的所有值出现的概率

## 代码

```golang
func dicesProbability(n int) []float64 {
    //动态规划
    //dp代表的含义是 n个 骰子  所有出现的值的概率
    dp := make([]float64, 6)
    //初始一个骰子的时候
    for i:=0; i<6; i++ {
        dp[i] = 1.0/6.0
    }
    //i 代表第几个骰子
    for i:=2; i<=n; i++ {
        //5i+1 代表的是 取值数量 范围是从 i-6i
        temp := make([]float64, 5*i+1)
        //之前的值 对 后面的影响
        //temp[j] 表示 i-1个骰子 出现的所有值的概率
        for j:=0; j<len(dp); j++ {
            //每次影响的 六个值
            for k:=0; k<6; k++ {
                temp[j+k] += dp[j] * (1.0/6.0)
            }
        }
        dp = temp
    }
    return dp
}
```

