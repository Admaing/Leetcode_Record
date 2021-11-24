# 142.环形链表 二

> 给定一个链表，返回链表开始入环的第一个节点。 如果链表无环，则返回 null。

> 如果链表中有某个节点，可以通过连续跟踪 next 指针再次到达，则链表中存在环。 为了表示给定链表中的环，评测系统内部使用整数 pos 来表示链表尾连接到链表中的位置（索引从 0 开始）。如果 pos 是 -1，则在该链表中没有环。注意：pos 不作为参数进行传递，仅仅是为了标识链表的实际情况。不允许修改 链表。

 ![img](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2018/12/07/circularlinkedlist.png)

示例 1：

输入：head = [3,2,0,-4], pos = 1
输出：返回索引为 1 的链表节点
解释：链表中有一个环，其尾部连接到第二个节点。



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

