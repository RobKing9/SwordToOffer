# [剑指 Offer 52. 两个链表的第一个公共节点](https://leetcode.cn/problems/liang-ge-lian-biao-de-di-yi-ge-gong-gong-jie-dian-lcof/)

## 题目

输入两个链表，找出它们的第一个公共节点。

## 思路

方法：双指针

主要思路：两个指针遍历两条链表，当遍历到结尾的时候，把头结点换成另一条的，这样两个指针一定会相遇，相遇的时候便是公共节点的开始。

细节：如果两个链表没有公共的结点，最后走完自己和另一个的，都将会到达nil，同样退出循环；

判断条件一定得是当前结点不为空，而不是下一个结点不为空，不然走不到nil，当他们没有公共节点的时候就会进入死循环。

## 代码

```golang
/**
 * Definition for singly-linked list.
 * type ListNode struct {
 *     Val int
 *     Next *ListNode
 * }
 */
func getIntersectionNode(headA, headB *ListNode) *ListNode {
    //特殊情况
    if headA==nil || headB==nil {
        return nil
    }
    curA, curB := headA, headB
    for curA!=curB {
        //注意判断条件
        if curA != nil {
            curA = curA.Next
        }else {
            //走另一个人的路
            curA = headB
        }
        if curB != nil {
            curB = curB.Next
        }else {
            //走另一个人的路
            curB = headA
        }
    }
    return curA
}
```

