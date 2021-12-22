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


