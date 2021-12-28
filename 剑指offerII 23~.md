# [剑指 Offer II 023. 两个链表的第一个重合节点](https://leetcode-cn.com/problems/3u1WK4/)

>给定两个单链表的头节点 headA 和 headB ，请找出并返回两个单链表相交的起始节点。如果两个链表没有交点，返回 null 。
>
>图示两个链表在节点 c1 开始相交：
>
>输入：intersectVal = 8, listA = [4,1,8,4,5], listB = [5,0,1,8,4,5], skipA = 2, skipB = 3
>输出：Intersected at '8'
>解释：相交节点的值为 8 （注意，如果两个链表相交则不能为 0）。
>从各自的表头开始算起，链表 A 为 [4,1,8,4,5]，链表 B 为 [5,0,1,8,4,5]。
>在 A 中，相交节点前有 2 个节点；在 B 中，相交节点前有 3 个节点。



思路：一共有两条链表，所以说两条都走完的长度是一样的，那么当两者相交的时候，交点即为两条链表的焦点



```python

class Solution:
    def getIntersectionNode(self, headA, headB):
        x, y = headA, headB
        while x != y:
            x = x.next if x else headB
            y = y.next if y else headA
        return x

```


# [剑指 Offer II 024. 反转链表](https://leetcode-cn.com/problems/UHnkqh/)

>  给定单链表的头节点 `head` ，请反转链表，并返回反转后的链表的头节点。
>
>  
>
> ```
> 输入：head = [1,2,3,4,5]
> 输出：[5,4,3,2,1]
> ```
>
> 





两种思路：一种递归

一直遍历，寻找下一个节点，并将下一个节点的next指针指向前一个pre节点

python代码

```python
class Solution(object):
    def reverseList(self, head):
        pre,cur = None,head
        while cur:
            tmp = cur.next
            cur.next = pre

            pre, cur =  cur,tmp
        return pre
            
```


# [剑指 Offer II 025. 链表中的两数相加](https://leetcode-cn.com/problems/lMSNwu/)

> 给定两个 非空链表 l1和 l2 来代表两个非负整数。数字最高位位于链表开始位置。它们的每个节点只存储一位数字。将这两数相加会返回一个新的链表。
>
> 可以假设除了数字 0 之外，这两个数字都不会以零开头。
>
> ```
> 输入：l1 = [7,2,4,3], l2 = [5,6,4]
> 输出：[7,8,0,7]
> ```

思路：将两个链表反转，然后按位想加，遇到进位则将值保存到下一个节点

```python
#         self.next = next
class Solution(object):
    def addTwoNumbers(self, l1, l2):
        """
        :type l1: ListNode
        :type l2: ListNode
        :rtype: ListNode
        """
        def reverseList(head):
            pre, cur = None, head
            while cur:
                tmp = cur.next
                cur.next = pre
                pre, cur = cur, tmp
            return pre
        rev_l1 = reverseList(l1)
        rev_l2 = reverseList(l2)
        count = 0
        ret = ListNode()
        tmp = ret
        
        while rev_l1 or rev_l2 or count:
            num = 0
            if rev_l1:
                num = num+rev_l1.val
                rev_l1 = rev_l1.next

            if rev_l2:
                num = num+rev_l2.val
                rev_l2 = rev_l2.next
            
            count,num = divmod(count+num,10)
            tmp.next = ListNode(num)
            tmp = tmp.next
        return reverseList(ret.next)
```


# [剑指 Offer II 026. 重排链表](https://leetcode-cn.com/problems/LGjMqU/)

> 给定一个单链表 L 的头节点 head ，单链表 L 表示为：
>
>  L0 → L1 → … → Ln-1 → Ln 
> 请将其重新排列后变为：
>
> L0 → Ln → L1 → Ln-1 → L2 → Ln-2 → …
>
> 不能只是单纯的改变节点内部的值，而是需要实际的进行节点交换。
>
> ```
> 输入: head = [1,2,3,4]
> 输出: [1,4,2,3]
> ```



思路：反转链表+快慢指针指针

首先设置快曼指针，慢指针走一步，快指针走两步

当快指针走到尽头时，慢指针走到中间，

将链表分开，反转后面一部分的链表，再将两表合并，即可完成重排





```python
class Solution:
    def reverseList(self, head):
        pre, cur = None, head
        while cur:
            tmp = cur.next
            cur.next = pre
            pre, cur = cur, tmp
        return pre

    def reorderList(self, head: ListNode) -> None:
        pre = ListNode()
        pre.next = head
        slow = fast = pre
        while fast and fast.next:
            slow = slow.next
            fast = fast.next.next
        half = slow.next
        slow.next = None
        rev_half = self.reverseList(half)
        cur = pre.next
        while slow and rev_half:
            tmp = cur.next
            cur.next = rev_half
            cur = cur.next
            rev_half = rev_half.next
            cur.next = tmp
            cur = cur.next

```

# [剑指 Offer II 027. 回文链表](https://leetcode-cn.com/problems/aMhZSa/)

> 给定一个链表的 **头节点** `head` **，**请判断其是否为回文链表。
>
> 如果一个链表是回文，那么链表节点序列从前往后看和从后往前看是相同的。
>
>  
>
> ```
> 输入: head = [1,2,3,3,2,1]
> 输出: true
> ```

思路：使用栈数据结构将

```python

class Solution:
    def isPalindrome(self, head: ListNode) -> bool:
        vals = []
        current_node = head
        while current_node is not None:
            vals.append(current_node.val)
            current_node = current_node.next
        return vals == vals[::-1]

```

# [剑指 Offer II 029. 排序的循环链表](https://leetcode-cn.com/problems/4ueAj6/)

> 给定循环单调非递减列表中的一个点，写一个函数向这个列表中插入一个新元素 insertVal ，使这个列表仍然是循环升序的。
>
> 给定的可以是这个列表中任意一个顶点的指针，并不一定是这个列表中最小元素的指针。
>
> 如果有多个满足条件的插入位置，可以选择任意一个位置插入新的值，插入后整个列表仍然保持有序。
>
> 如果列表为空（给定的节点是 null），需要创建一个循环有序列表并返回这个节点。否则。请返回原先给定的节点。
>
> 输入：head = [3,4,1], insertVal = 2
> 输出：[3,4,1,2]
> 解释：在上图中，有一个包含三个元素的循环有序列表，你获得值为 3 的节点的指针，我们需要向表中插入元素 2 。新插入的节点应该在 1 和 3 之间，插入之后，整个列表如上图所示，最后返回节点 3 。



```python
"""
# Definition for a Node.
class Node:
    def __init__(self, val=None, next=None):
        self.val = val
        self.next = next
"""

class Solution:
    def insert(self, head: 'Node', insertVal: int) -> 'Node':
        if not head:
            tmp = Node(insertVal)
            tmp.next = tmp
            return tmp

        p = head
        while p.next != head:
            if p.val > p.next.val: # 到达边界点
                if p.val < insertVal or insertVal < p.next.val: # 大于最大值 or 小于最小值
                    break
            
            if p.val <= insertVal and insertVal <= p.next.val: # 在内部找到可以满足的插入点
                break

            p = p.next 
        
        p.next = Node(insertVal, p.next)
        return head
        
```

# 272周赛

## [5963. 反转两次的数字](https://leetcode-cn.com/problems/a-number-after-a-double-reversal/)

```python
class Solution(object):
    def isSameAfterReversals(self, num):
        """
        :type num: int
        :rtype: bool
        """
        if num==0:
            return True
        num1 = str(num)
        num1 = list(num1[::-1])
        while num1[0]=='0':
            num1.pop(0)
            
        num2 = ""
        for i in num1[::-1]:
            num2 = num2 + i
            
        num2 = int(num2)
        return num2==num
```

## [5965. 相同元素的间隔之和](https://leetcode-cn.com/problems/intervals-between-identical-elements/)

```python
class Solution:
    def getDistances(self, arr: List[int]) -> List[int]:
        d, res = defaultdict(list), [0] * len(arr)
        for i, x in enumerate(arr): d[x].append(i)
        for x in d:
            pi, n, c = 0, len(d[x]), sum(d[x])
            for i, pn in enumerate(d[x]): res[(pi := pn)] = (c := c - (n - 2 * i) * (pn - pi))
        return res

```


# [剑指 Offer II 028. 展平多级双向链表](https://leetcode-cn.com/problems/Qv1Da2/)

## DFS模板

```python
# python DFS模板  递归版本
visited = set()
def dfs(node, visited):
    if node in visited:
        return 
   	visited.add(node)
    
    for next_node in node.children():
        if not next_node in visited:
            dfs(next_node, visited)
```

> 多级双向链表中，除了指向下一个节点和前一个节点指针之外，它还有一个子链表指针，可能指向单独的双向链表。这些子列表也可能会有一个或多个自己的子项，依此类推，生成多级数据结构，如下面的示例所示。
>
> 给定位于列表第一级的头节点，请扁平化列表，即将这样的多级双向链表展平成普通的双向链表，使所有结点出现在单级双链表中。
>
>  输入：head = [1,2,null,3]
> 输出：[1,3,2]
> 解释：
>
> 输入的多级列表如下图所示：
>
>   1---2---NULL
>   |
>   3---NULL

DFS 每日一抄

```python
"""
# Definition for a Node.
class Node(object):
    def __init__(self, val, prev, next, child):
        self.val = val
        self.prev = prev
        self.next = next
        self.child = child
"""

class Solution(object):
    def flatten(self, head):
        """
        :type head: Node
        :rtype: Node
        """
        if head == None:
            return head

        dummy = Node(-1, Node,Node,Node)
        
        def dfs(pre,cur):
            if cur == None:
                return pre
            
            pre.next = cur
            cur.prev = pre

            nxt_head = cur.next

            tail = dfs(cur,cur.child)
            cur.child = None

            return dfs(tail, nxt_head)

        dfs(dummy,head)
        dummy.next.prev = None
        return dummy.next
```




# [剑指 Offer II 035. 最小时间差](https://leetcode-cn.com/problems/569nqc/)

> 给定一个 24 小时制（小时:分钟 "HH:MM"）的时间列表，找出列表中任意两个时间的最小时间差并以分钟数表示。
>
>  
>
> 示例 1：
>
> 输入：timePoints = ["23:59","00:00"]
> 输出：1



思路：所有时间为24*60，若列表长度超过这个值，则存在重复的时间，直接返回零，若不存在重复的时间，将分钟转换成为时间，a *24 + b  然后进行排序，最后将第一个点+24 * 60 放到列表末尾，

然后遍历寻找最大和最小的两个值进行相减，所得为结果

```python
class Solution(object):
    def findMinDifference(self, timePoints):
        """
        :type timePoints: List[str]
        :rtype: int
        """
        hash_map = {}
        for i in timePoints:
            if i in hash_map:
                return 0
            else:
                hash_map[i] = 1

        minutes = sorted(int(t[:2])*60 + int(t[3:]) for t in timePoints)
        minutes.append(minutes[0] + 24*60)
        res = minutes[-1]

        for i in range(1,len(minutes)):
            res = min(res,minutes[i]-minutes[i-1])
        return res
```

