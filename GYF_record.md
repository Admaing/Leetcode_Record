

# 15

三数之和

排序加双指针逼近

碰到相同直接jump

# 16

三树之和最接近

排序加双指针

# 星期四

git config --local user.email "471218449@qq.com"

git config --global user.name "Adaming"



# 86 分割链表

一条链表储存小的值，一条链表储存比x大的值

最后两条链表拼接
代码

/**

 * Definition for singly-linked list.

 * type ListNode struct {

 * Val int

 * Next *ListNode

 * }
   */
   func partition(head *ListNode, x int) *ListNode {
   bhead :=&ListNode{}
   fhead :=&ListNode{}
   last :=bhead
   b := bhead
   f :=fhead
   c :=fhead
   for head!=nil {
       if head.Val<x{
           b.Next = head
           b = b.Next
       }else{
           f.Next = head
           f = f.Next
       }
       head = head.Next
   }

   f.Next = nil
   b.Next = c.Next

   return last.Next
   }

# 滑动窗口

首先初始化窗口的左右边界，
右边界不断增加，每当进入一个新的字符时，计算当前字符所在的坑（freq[s[right]-‘a’],计算当前字符是否存在）是否被占，即是否有重复字符，如果存在重复字符，则滑动窗口左边右移，并将坑位的左面置为0，直到重复的字符移出了左边界，以此类推，最终最大的值为题目所解。

```go
func lengthOfLongestSubstring(s string) int {
    var freq [256]int
    if len(s)==0{
        return 0
    }
    
```

```go
left,right,result:=0, 0, 0
for left<len(s){
    if right<len(s)&&freq[s[right]-'a']==0{
        //判断当前坑是否被占
        freq[s[right]-'a']++
        right++
    }else{
        freq[s[left]-'a']--
        left++
    }
    result = max(result,right-left)
}
return result}
```

```go
func max(a int,b int) int{
    if a>b{
        return a
    }
    return b
}


```





# 最长回文子串

动态规划

特判：小于二的时候一定是回文串

状态方程：

```go
func longestPalindrome(s string) string {
	if len(s) < 2 {
		return s
	}
	println(len(s))

	maxlen := 1

	dp := make([][]bool, len(s))
	for i := 0; i < len(s); i++ {
		dp[i] = make([]bool, len(s))
	}


	begin := 0
	for L := 2; L <= len(s); L++ {
		for i := 0; i < len(s); i++ {
			j := L + i - 1
			if j >= len(s) {
				break
			}

			if s[i] != s[j] {
				dp[i][j] = false
			} else {
				if j-i < 3 {
					dp[i][j] = true
				} else {
					dp[i][j] = dp[i+1][j-1]
				}
			}

			if dp[i][j] == true && j-i+1 > maxlen {
				maxlen = j - i + 1
				begin = i
			}

		}

	}

	return s[begin : begin+maxlen]
}

```

# 61 闭环链表

解题思路

此处撰写解题思路
同官方题解
闭环链表

如果k大于链表的长度，则唯一的次数应该为k%n,当该数等于0时，说明正好位移一个完整的轮次，或者根本没动，

因此根据上述，首先计算链表长度，然后将链表头尾连接，进行长度位移，最后将位移的最后一位指向null，
即将链表打开，返回下一个节点
代码

/**

 * Definition for singly-linked list.
 * type ListNode struct {
 * Val int
 * Next *ListNode
 * }
   */

func rotateRight(head *ListNode, k int) *ListNode {
    if head==nil || head.Next==nil || k==0{
        return head

    }
    
    a :=head
    num := 1
    for a.Next!=nil{
        a = a.Next
        num++
    }


    add := num - k%num
    if add == num{
        return head
    }
    a.Next = head
    
    for add>0{
        a = a.Next
        add--

# 80 删除重复元素原地

解题思路

此处撰写解题思路

看到原地删除想到使用双指针

使用双指针的关键：移动条件



快指针用来遍历数组

满指针在倒数第二个重复元素才移动

并且新的数组始终和慢指针保持进度

不会超过两个重复元素

代码



    func removeDuplicates(nums []int) int {
       slow:=0
       for fast,v:=range nums{
            if fast<2 || nums[slow-2]!=v{
                nums[slow] = v
                slow ++
        }
    }
    
    return slow

}

# 82 删除排序列表中重复元素

同官方题解

解释后续补充

```go
func deleteDuplicates(head *ListNode) *ListNode {

  if head == nil {

​      return nil

​    }



  dummy := &ListNode{0, head}



  cur := dummy

  for cur.Next != nil && cur.Next.Next != nil {

​    if cur.Next.Val == cur.Next.Next.Val {

​      x := cur.Next.Val

​      for cur.Next != nil && cur.Next.Val == x {

​        cur.Next = cur.Next.Next

​      }

​    } else {

​      cur = cur.Next

​    }

  }



  return dummy.Next



}
```

# 142.环形链表 二

> 给定一个链表，返回链表开始入环的第一个节点。 如果链表无环，则返回 null。

> 如果链表中有某个节点，可以通过连续跟踪 next 指针再次到达，则链表中存在环。 为了表示给定链表中的环，评测系统内部使用整数 pos 来表示链表尾连接到链表中的位置（索引从 0 开始）。如果 pos 是 -1，则在该链表中没有环。注意：pos 不作为参数进行传递，仅仅是为了标识链表的实际情况。不允许修改 链表。

 ![img](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2018/12/07/circularlinkedlist.png)

示例 1：

输入：head = [3,2,0,-4], pos = 1
输出：返回索引为 1 的链表节点
解释：链表中有一个环，其尾部连接到第二个节点。

### medium







思路：

建立哈希表保存节点，判断节点是否在哈希表当中，不在则当前节点为环入口,直接返回该节点位置



> go代码

```go

// map初始化为空时要加大括号
func detectCycle(head *ListNode) *ListNode {
    a := map[*ListNode]int{}
	// 创建哈希表
    for head!=nil{
        //当不在哈希表时返回false
        if _,ok := a[head]; ok{
            return head
        }
        //将元素存入哈希表
        a[head] = 1
        head = head.Next
    }
    return nil
}

```





> python代码

```python
class Solution:
    def detectCycle(self, head: ListNode) -> ListNode:
        hash_map = {}
        while head:
            if head in hash_map:
                return head
            
            hash_map[head] = 1
            head = head.next
        return None
```



> 双指针方法待补充（使用空间O(1)）



# 143.重排链表

> 给定一个单链表 L 的头节点 head ，单链表 L 表示为：
>
> L0 → L1 → … → Ln - 1 → Ln
>
> 请将其重新排列后变为：
>
> L0 → Ln → L1 → Ln - 1 → L2 → Ln - 2 → …
>
> 不能只是单纯的改变节点内部的值，而是需要实际的进行节点交换。
>
> 
>
> 示例 1：
>
> 输入：head = [1,2,3,4]
> 输出：[1,4,2,3]



### medium

思路：

由于链表不能按照下标访问，因此将其转换成下标可以访问的线性表

- 将链表存入线性表
- 然后根据线性表，首尾连接重建表

> go

```go
/**
 * Definition for singly-linked list.
 * type ListNode struct {
 *     Val int
 *     Next *ListNode
 * }
 */
func reorderList(head *ListNode)  {

    // 特判
    if head==nil{
        return
    }
    nodes := []*ListNode{}

    for node:=head;node!=nil; node=node.Next{
        // 将节点放入线性表,第二个循环直接重建
        nodes = append(nodes,node)
    }
    i,j:=0,len(nodes)-1
    for i<j{
        nodes[i].Next = nodes[j]
        i++

        if i==j{
            break
        }
        nodes[j].Next = nodes[i]
        j--
    }
    // 将最后一个节点指向nil
    nodes[i].Next = nil    
}
```

## 2 两数之和

>给你两个 非空 的链表，表示两个非负的整数。它们每位数字都是按照 逆序 的方式存储的，并且每个节点只能存储 一位 数字。
>
>请你将两个数相加，并以相同形式返回一个表示和的链表。
>
>你可以假设除了数字 0 之外，这两个数都不会以 0 开头。
>
>![img](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2021/01/02/addtwonumber1.jpg)
>
>```
>输入：l1 = [2,4,3], l2 = [5,6,4]
>输出：[7,0,8]
>解释：342 + 465 = 807.
>```



思路：

直接遍历两个链表，按照遍历的先后顺序第一位乘以十的零次幂，第二位十的一次幂，依次类推

求和之后，按照对顺序依次递减

CV官方题解

```go

func addTwoNumbers(l1, l2 *ListNode) (head *ListNode) {
    var tail *ListNode
    carry := 0
    for l1 != nil || l2 != nil {
        n1, n2 := 0, 0
        if l1 != nil {
            n1 = l1.Val
            l1 = l1.Next
        }
        if l2 != nil {
            n2 = l2.Val
            l2 = l2.Next
        }
        sum := n1 + n2 + carry
        sum, carry = sum%10, sum/10
        if head == nil {
            head = &ListNode{Val: sum}
            tail = head
        } else {
            tail.Next = &ListNode{Val: sum}
            tail = tail.Next
        }
    }
    if carry > 0 {
        tail.Next = &ListNode{Val: carry}
    }
    return
}
```

# 周赛 5938 [找出数组排序后的目标下标](https://leetcode-cn.com/problems/find-target-indices-after-sorting-array/)

> 给你一个下标从 0 开始的整数数组 nums 以及一个目标元素 target 。
>
> 目标下标 是一个满足 nums[i] == target 的下标 i 。
>
> 将 nums 按 非递减 顺序排序后，返回由 nums 中目标下标组成的列表。如果不存在目标下标，返回一个 空 列表。返回的列表必须按 递增 顺序排列。
>
> 示例 1：
>
> 输入：nums = [1,2,5,2,3], target = 2
> 输出：[1,2]
> 解释：排序后，nums 变为 [1,2,2,3,5] 。
> 满足 nums[i] == 2 的下标是 1 和 2 。



难度：easy

思路：二分查找后遍历数组，返回目标值

# 128 最长连续序列

> 给定一个未排序的整数数组 nums ，找出数字连续的最长序列（不要求序列元素在原数组中连续）的长度。
>
> 请你设计并实现时间复杂度为 O(n) 的算法解决此问题。
>
>  
>
> 示例 1：
>
> 输入：nums = [100,4,200,1,3,2]
> 输出：4
> 解释：最长数字连续序列是 [1, 2, 3, 4]。它的长度为 4。



暴力：排序去重搜索 

并查集，看不懂

# 面试题01.01 判断字符是否唯一

> #### [面试题 01.01. 判定字符是否唯一](https://leetcode-cn.com/problems/is-unique-lcci/)
>
> 实现一个算法，确定一个字符串 `s` 的所有字符是否全都不同。
>
> **示例 1：**
>
> ```
> 输入: s = "leetcode"
> 输出: false 
> ```

方法一：哈希

```python
class Solution:
    def isUnique(self, astr: str) -> bool:
        hashMap = {}
        for i in astr:
            if i in hashMap:
                return False
            hashMap[i] = 1

        return True
```



方法二：暴力双循环

```python
class Solution:
    def isUnique(self, astr: str) -> bool:
        for i in range(len(astr)):
            for j in range(len(astr)):
                if i==j:
                    continue

                if astr[i] == astr[j]:
                    return False


        return True
```

如果题目限定至小写字母，可使用位运算

# 面试题01.02 判定是否为字符重排

> 给定两个字符串 s1 和 s2，请编写一个程序，确定其中一个字符串的字符重新排列后，能否变成另一个字符串。
>
> 示例 1：
>
> 输入: s1 = "abc", s2 = "bca"
> 输出: true 

哈希表，将两个字符串分别存入哈希表，然后比较两个哈希表是否相同

```python
class Solution:
    def CheckPermutation(self, s1: str, s2: str) -> bool:
        # 第一思路 哈希表
        hashMap = {}
        for i in s1:
            if i in hashMap:
                hashMap[i] = hashMap[i] +1
            else:
                hashMap[i] = 1

        hashMap2 = {}
        for q in s2:
            if q in hashMap2:
                hashMap2[q] = hashMap2[q] +1
            else:
                hashMap2[q] = 1

        return hashMap==hashMap2
```



# 剑指offer II 006.排序数组中的两个数字之和

> 给定一个已按照 升序排列  的整数数组 numbers ，请你从数组中找出两个数满足相加之和等于目标数 target 。
>
> 函数应该以长度为 2 的整数数组的形式返回这两个数的下标值。numbers 的下标 从 0 开始计数 ，所以答案数组应当满足 0 <= answer[0] < answer[1] < numbers.length 。
>
> 假设数组中存在且只存在一对符合条件的数字，同时一个数字不能使用两次。
>
> 示例 1：
>
> 输入：numbers = [1,2,4,6,10], target = 8
> 输出：[1,3]
> 解释：2 与 6 之和等于目标数 8 。因此 index1 = 1, index2 = 3 。



标准双指针写法，

解题思路

标准双指针解法
两个指针分别放在头尾，当两数之和大于目标值时，尾指针向前走
小于目标值时，头指针向后走



代码

```python
 class Solution:
    def twoSum(self, numbers: List[int], target: int) -> List[int]:
        i = len(numbers)-1
        j = 0
        while True:
            if numbers[i]+numbers[j]<target:
                j = j + 1
            elif numbers[i]+numbers[j]>target:
                i = i - 1
            else:
                return [j,i]
```



# 剑指offer II 004只出现一次的数字

> 给你一个整数数组 nums ，除某个元素仅出现 一次 外，其余每个元素都恰出现 三次 。请你找出并返回那个只出现了一次的元素。
>
>  
>
> 示例 1：
>
> 输入：nums = [2,2,3,2]
> 输出：3



思路：哈希表，哈希表存储数据，返回为零的key



哈希解法

```python
class Solution:
    def singleNumber(self, nums: List[int]) -> int:
        # 第一眼思路哈希
        hashmap = {}
        for i in nums:
            if i in hashmap:
                hashmap[i] = hashmap[i]+1
            else:
                hashmap[i] = 0

        for i in nums:
            if hashmap[i] == 0:
                return i
```



进阶，时间复杂度为o(n)并且不使用额外空间，应该想到位运算，想不到

异或运算，二进制相同异或为零，不同异或

剑指offer II 005单词长度的最大乘积



# 剑指offer II 007 数组中和为0的三个数

>  内容基本同三数之和
>
> 给定一个包含 n 个整数的数组 nums，判断 nums 中是否存在三个元素 a ，b ，c ，使得 a + b + c = 0 ？请找出所有和为 0 且 不重复 的三元组。
>
>  
>
> 示例 1：
>
> 输入：nums = [-1,0,1,2,-1,-4]
> 输出：[[-1,-1,2],[-1,0,1]]



思路：固定头指针，然后中间双指针逼近，跳过相同元素



```python
from typing import List


class Solution:
    def threeSum(self, nums: List[int]) -> List[List[int]]:
        if len(nums) < 3:
            return []

        nums.sort()

        res = []
        for i in range(0, len(nums) - 2):
            if i > 0 and nums[i] == nums[i - 1]: continue
            target = -nums[i]
            left, right = i + 1, len(nums) - 1
            while left < right:
                s = nums[left] + nums[right]
                if s == target:
                    res.append([nums[i], nums[left], nums[right]])
                    # 去重
                    while left < right:
                        left = left + 1
                        if nums[left - 1] != nums[left]: break
                    while left < right:
                        right = right - 1
                        if nums[right + 1] != nums[right]: break
                elif s < target:
                    left = left + 1
                else:
                    right = right - 1
        return res

```



# 457 环形数组是否存在循环

> 存在一个不含 0 的 环形 数组 nums ，每个 nums[i] 都表示位于下标 i 的角色应该向前或向后移动的下标个数：
>
> 如果 nums[i] 是正数，向前（下标递增方向）移动 |nums[i]| 步
> 如果 nums[i] 是负数，向后（下标递减方向）移动 |nums[i]| 步
> 因为数组是 环形 的，所以可以假设从最后一个元素向前移动一步会到达第一个元素，而第一个元素向后移动一步会到达最后一个元素。
>
> 数组中的 循环 由长度为 k 的下标序列 seq 标识：
>
> 遵循上述移动规则将导致一组重复下标序列 seq[0] -> seq[1] -> ... -> seq[k - 1] -> seq[0] -> ...
> 所有 nums[seq[j]] 应当不是 全正 就是 全负
> k > 1
> 如果 nums 中存在循环，返回 true ；否则，返回 false 。
>
> 示例 1：
>
> 输入：nums = [2,-1,1,2,2]
> 输出：true
> 解释：存在循环，按下标 0 -> 2 -> 3 -> 0 。循环长度为 3 。

python 代码

```python
class Solution:
    def circularArrayLoop(self, nums: List[int]) -> bool:
        n = len(nums)
        # 数组长度
        def next(cur):
            # 返回指针下一步要到的下标的位置
            return (cur+nums[cur])%n

   
        for i,num in enumerate(nums):
            if num==0:
                continue 
            # 定义快慢指针，快指针每次走两步，起步快指针在慢指针的下一步
            slow,fast = i, next(i)
            
            #要判断方向是否相同，因为条件是环的元素不是全正就是全负，因此应该相乘大于零
            while nums[slow]*nums[next(slow)]>0 and nums[fast]*nums[next(fast)]>0:
                if slow == fast: 
                    if slow ==next(slow):
                        break
                    return True
                slow = next(slow)
                fast = next(next(fast))
            add = i
            while nums[add]*nums[next(add)] >0:
                tmp = add
                add = next(add)
                nums[tmp] = 0

        return False
```

# [剑指 Offer II 008. 和大于等于 target 的最短子数组](https://leetcode-cn.com/problems/2VG8Kg/)



> 给定一个含有 n 个正整数的数组和一个正整数 target 。
>
> 找出该数组中满足其和 ≥ target 的长度最小的 连续子数组 [numsl, numsl+1, ..., numsr-1, numsr] ，并返回其长度。如果不存在符合条件的子数组，返回 0 。
>
>  
>
> 示例 1：
>
> 输入：target = 7, nums = [2,3,1,2,4,3]
> 输出：2
> 解释：子数组 [4,3] 是该条件下的长度最小的子数组。



自己的思路：使用滑动窗口，判断当前窗口的和的大小，如果大于目标值之后，判断窗口左端右划是否满足条件，记录满足条件的最小窗口

```python
#官方题解代码
class Solution:
    def minSubArrayLen(self, s: int, nums: List[int]) -> int:
        if not nums:
            return 0
        
        n = len(nums)
        ans = n + 1
        start, end = 0, 0
        total = 0
        while end < n:
            total += nums[end]
            while total >= s:
                ans = min(ans, end - start + 1)
                total -= nums[start]
                start += 1
            end += 1
        
        return 0 if ans == n + 1 else ans

```



```python
#自己的代码
class Solution:
      
    def minSubArrayLen(self, target: int, nums: List[int]) -> int:
        def sum(i,j,nums):
            num = 0
            while i<=j:
                num = num+nums[i]
                i = i+1
            return num  
        i = 0
        j = 0
        minn = len(nums)
        while j<=len(nums)-1:
            num = 0
            
            num = sum(i,j,nums)
            if num>=target and i<=j:
                
                while num>=target and i<=j:
                    i = i+1
                    num = sum(i,j,nums)
                i = i-1
                minx = j -i+1
                if minx<minn:
                    minn = minx        
            j = j+1
        if minn==len(nums) and sum(0,len(nums)-1,nums)<target:
            return 0
        return minn
```



# [剑指 Offer II 009. 乘积小于 K 的子数组](https://leetcode-cn.com/problems/ZVAVXX/)

>给定一个正整数数组 nums和整数 k ，请找出该数组内乘积小于 k 的连续的子数组的个数。
>
> 
>
>示例 1:
>
>输入: nums = [10,5,2,6], k = 100
>输出: 8
>解释: 8 个乘积小于 100 的子数组分别为: [10], [5], [2], [6], [10,5], [5,2], [2,6], [5,2,6]。
>需要注意的是 [10,5,2] 并不是乘积小于100的子数组。

# 总结滑动窗口模板

python

```python
def findSubArray(nums):
    n = len(nums)
    left,right = 0,0
    sums = 0
    res = N+1 #长度，根据条件更改，若是最短则设置N+1，否则设置为0
    while right<N:
        sums += nums[right]
        while {符合题意的一个条件}:
            res = min或者max (res, right-left+1)#根据最大或者最小更改
            sums -= nums[left]
            left+=1
        right+=1
        
    return res # 返回长度
```



# [7. 整数反转](https://leetcode-cn.com/problems/reverse-integer/)

> 

python

```python
class Solution {
    public int reverse(int x) {
        int res = 0;
        int last = 0;
        while(x!=0) {
            //每次取末尾数字
            int tmp = x%10;
            last = res;
            res = res*10 + tmp;
            //判断整数溢出
            if(last != res/10)
            {
                return 0;
            }
            x /= 10;
        }
        return res;
    }
}	
```

