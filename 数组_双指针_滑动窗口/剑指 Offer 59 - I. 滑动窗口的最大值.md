# [剑指 Offer 59 - I. 滑动窗口的最大值](https://leetcode.cn/problems/hua-dong-chuang-kou-de-zui-da-zhi-lcof/)

## 题目

给定一个数组 nums 和滑动窗口的大小 k，请找出所有滑动窗口里的最大值。

```
示例:

输入: nums = [1,3,-1,-3,5,3,6,7], 和 k = 3
输出: [3,3,5,5,6,7] 
解释: 

  滑动窗口的位置                最大值

---------------               -----

[1  3  -1] -3  5  3  6  7       3
 1 [3  -1  -3] 5  3  6  7       3
 1  3 [-1  -3  5] 3  6  7       5
 1  3  -1 [-3  5  3] 6  7       5
 1  3  -1  -3 [5  3  6] 7       6
 1  3  -1  -3  5 [3  6  7]      7
```


提示：

- 你可以假设 k 总是有效的，在输入数组 不为空 的情况下，1 ≤ k ≤ nums.length。


## 思路

方法：单调队列

主要思路：根据我们之前写过的最小栈的灵感，如果我们要找到最小值，直接取栈头的话，时间复杂度为O(1)。那么这里最大值是不是也一样呢？我们可以维护一个单调递减的队列，让队头总是最大值，我们需要判断当前值，先和队头比较，如果大于队头的话，意味着队列里面所有的元素都需要出队，然后和队尾进行比较，将比当前值小的队尾全部出队；其实这算是一个双端队列，两端都可以出队。最后我们判断一下左指针是否大于等于0，是否构成一个滑动窗口，构成的话把最大值加入到结果即可。

细节：我们还需要将队列过期的队头出队，因为再大的值也不能保证一直在窗口中，只需要比较左指针左边的那个值和队头是否相等即可，因为左指针左边那个值已经不在窗口内了，相等就意味着队头也不在窗口内了，需要踢出去。

## 代码

```golang
func maxSlidingWindow(nums []int, k int) []int {
    left, right := 1-k, 0
    result := make([]int, 0)
    //维持一个递减队列
    queue := make([]int, 0)
    for right<len(nums) {
        //淘汰不属于窗口中的值
        if left>=1 && nums[left-1] == queue[0]{
            queue = queue[1:]
        }
        //出队的时候要保证 队列不为空
        //队头 小于当前值的 出队
        for len(queue)!=0 && nums[right] > queue[0] {
            queue = queue[1:]
        }
        //队尾 小于当前值的 出队
        for len(queue)!=0 && nums[right] > queue[len(queue)-1] {
            queue = queue[:len(queue)-1]
        }
        //当前值入队
        queue = append(queue, nums[right])
        //形成 滑动窗口的时候 
        if left>=0 {
            //最大值就是队头
            result = append(result, queue[0])
        }
        left++
        right++
    }
    return result
}

```

