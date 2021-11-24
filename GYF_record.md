# 142.环形链表 二



思路：

建立哈希表保存节点，判断节点是否在哈希表当中，不在则当前节点为环入口



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



> 双指针方法待补充