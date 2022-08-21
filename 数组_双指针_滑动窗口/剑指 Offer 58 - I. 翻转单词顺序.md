# [剑指 Offer 58 - I. 翻转单词顺序](https://leetcode.cn/problems/fan-zhuan-dan-ci-shun-xu-lcof/)

## 题目

输入一个英文句子，翻转句子中单词的顺序，但单词内字符的顺序不变。为简单起见，标点符号和普通字母一样处理。例如输入字符串"I am a student. "，则输出"student. a am I"。

```
示例 1：

输入: "the sky is blue"
输出: "blue is sky the"
示例 2：

输入: "  hello world!  "
输出: "world! hello"
解释: 输入字符串可以在前面或者后面包含多余的空格，但是反转后的字符不能包括。
示例 3：

输入: "a good   example"
输出: "example good a"
解释: 如果两个单词间有多余的空格，将反转后单词间的空格减少到只含一个。
```


说明：

- 无空格字符构成一个单词。

- 输入字符串可以在前面或者后面包含多余的空格，但是反转后的字符不能包括。
- 如果两个单词间有多余的空格，将反转后单词间的空格减少到只含一个。

## 思路

方法：双指针，优化时间复杂度

主要思路：三种不一样的思路得到不一样的时间复杂度

- 直接使用库函数，先用`strings.Split()`按照空格进行分离成一个数组，然后遍历这个数组，如果是空格跳过，否则加入到结果并在后面加一个空格；最后用`strings.TrimSpace()`将最后一个多余的空格去掉

```golang
func reverseWords(s string) string {
    //先存放到一个 单词数组中
    str := strings.Split(s, " ")
    var res string
    for j:=len(str)-1; j>=0; j-- {
        if str[j]=="" {
            continue
        }else {
            res = res + str[j] + " "
        }
    }
    return strings.TrimSpace(res)
}
```



- 第二种是双指针，用一个数组保存单词，一开始还是把首尾空格用`strings.TrimSpace()`去掉，从最后开始遍历，左指针一直往左移动，直到遇到空格，然后把这个单词加入到单词数组（最后还是需要追加一个空格）；接着需要跳过空格，直到遇到非空格，然后将右指针直接等于左指针的位置，最后遍历单词数组，拿出来，去掉最后的空格

```golang
func reverseWords(s string) string {
    //去掉首尾空格
    str := strings.TrimSpace(s)
    //双指针法
    left, right := len(str)-1, len(str)-1
    res := make([]string, 0)
    for left>=0 {
        //移动左指针 
        for left>=0 && str[left] != ' ' {
            left--
        }
        //遇到空格的时候 先把结果保存
        res = append(res, str[left+1:right+1]+" ")
        for left>=0 && str[left] == ' ' {
                left--
        }
        //移动右指针
        right = left
    }
    var result string
    for i := range res {
        result += res[i]
    }
    return strings.TrimSpace(result)
}
```



- 第三种还是双指针，但是执行时间为0，我们用一个字节数组保存结果。我们开始也不需要去首尾空格，直接开始遍历，一开始如果是空格的话就先让左指针跳过，那么这个位置就可以给右指针，接着移动左指针直到为空格，加入到字节数组，我们加入的方式很特别，`res = append(res, s[i+1:tail+1]...)`，最后我们判断右指针是否等于左指针，不相等的话说明还没结束，此时在最后添加一个空格，相等的话以为着到最后一个单词了，没有必要再添加一个空格了

```golang
func reverseWords(s string) string {
    //字节数组
    res := []byte{}

	for i := len(s) - 1; i >= 0; {
        //跳过空格
		for i >= 0 && s[i] == ' ' {
			i--
		}
        //双指针
        //记录右指针的位置
		tail := i
        //移动左指针 直到不为空格
		for i >= 0 && s[i] != ' ' {
			i--
		}
        //字符串加入到字节数组
		res = append(res, s[i+1:tail+1]...)
        //避免最后 还多加了 一个空格
		if tail != i {
			res = append(res, ' ')
		}
	}
    if len(res) == 0 {
        return ""
    }
    //字节数组转换为 字符串
	return string(res[:len(res)-1])
}
```

细节：不管怎么在移动左指针的时候都要保证大于等于0

## 总结

我们在致力于提高算法性能的时候，不是说用一种高效的算法思维就好，而且还要求我们用对数据结构，节省空间，提高效率。