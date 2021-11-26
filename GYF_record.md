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

