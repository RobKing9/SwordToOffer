# [剑指 Offer 56 - II. 数组中数字出现的次数 II](https://leetcode.cn/problems/shu-zu-zhong-shu-zi-chu-xian-de-ci-shu-ii-lcof/)

## 题目

在一个数组 nums 中除一个数字只出现一次之外，其他数字都出现了三次。请找出那个只出现一次的数字。

```
示例 1：

输入：nums = [3,4,3,3]
输出：4
示例 2：

输入：nums = [9,1,7,9,7,9,7]
输出：1
```


限制：

- 1 <= nums.length <= 10000
- 1 <= nums[i] < 2^31

## 思路

方法：位运算

主要思路：将数组中的每一个数变为二进制，每个数的每一位相加，数字出现三次的相加对3取余还是零，所以都对3取余，最后得到的二进制数就是只出现一次的那个数，将其转换为二进制即可。这一题主要有两个地方，一个是将每个数转化为二进制，一个是将二进制数转化为十进制。

## 代码

```golang
func singleNumber(nums []int) int {
    //思路：将每个数的二进制的每一位进行 相加
    //因为只有一个只有一个数，那么其他加起来一定是三的倍数
    //对每一位相加的结果 对3取余 得到的 最后数一定是那个只出现一次的
    //用一个数组来记录 每位出现的总次数
    bits := make([]int, 32)
    //遍历数组
    for _, v := range nums {
        //j 用来移动 每个数字的每一位
        j := 0
        for v!=0 {
            //模拟 一个数 化为二进制的过程
            bits[j] += v%2
            v /= 2
            j++
        }
    }
    //经过上面的操作 我们就可以得到一个 位次数 数组
    result := 0
    for i:=0; i<32; i++ {
        result += (1<<i) * (bits[i]%3)
    }
    return result
}
```

时间复杂度：O(n)

空间复杂度：O(1)