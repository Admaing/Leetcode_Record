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
# [剑指 Offer II 032. 有效的变位词](https://leetcode-cn.com/problems/dKk3P7/)

> 给定两个字符串 s 和 t ，编写一个函数来判断它们是不是一组变位词（字母异位词）。
>
> 注意：若 s 和 t 中每个字符出现的次数都相同且字符顺序不完全相同，则称 s 和 t 互为变位词（字母异位词）。
>
>  
>
> 示例 1:
>
> 输入: s = "anagram", t = "nagaram"
> 输出: true



思路：将两个字符串存入哈希表，返回两个哈希表比较的结果

简单题我重拳出击 😁

```python
class Solution:
    def isAnagram(self, s: str, t: str) -> bool:
        hash_map = {}
        hash_map1 = {}
        if t==s:
            return False

        for i in s:
            if i in hash_map:
                hash_map[i]+=1
            else:
                hash_map[i] = 0

        for i in t:
            if i in hash_map1:
                hash_map1[i]+=1
            else:
                hash_map1[i] = 0

        return hash_map1==hash_map
```

# [剑指 Offer II 039. 直方图最大矩形面积](https://leetcode-cn.com/problems/0ynMMM/)

> 给定非负整数数组 heights ，数组中的数字用来表示柱状图中各个柱子的高度。每个柱子彼此相邻，且宽度为 1 。
>
> 求在该柱状图中，能够勾勒出来的矩形的最大面积。
>
> **示例 1:**
>
> ![img](12.20.assets/histogram.jpg)
>
> ```
> 输入：heights = [2,1,5,6,2,3]
> 输出：10
> 解释：最大的矩形为图中红色区域，面积为 10
> ```



暴力

```python
class Solution:
    def largestRectangleArea(self, heights: List[int]) -> int:
        a = []
        m_h = 0
        for i in heights:
            a.append(i)
            q = a[::-1]
            for j,w in enumerate(q):
                m_q = min(q[:j+1])
                print(j)

                if m_q*(j+1) > m_h:
                    m_h = m_q*(j+1)


        return m_h
```



超时





```python
class Solution:
    def largestRectangleArea(self, heights: List[int]) -> int:
        a = [-1]
        m_h = 0
        for i in heights:
            if i > a[-1]:
            	a.append(i)
            else:
            	q = a[::-1]
            for j,w in enumerate(q):
                m_q = min(q[:j+1])

                if m_q*(j+1) > m_h:
                    m_h = m_q*(j+1)


        return m_h
```





```python
class Solution:
    def largestRectangleArea(self, heights: List[int]) -> int:
        n = len(heights)

        left_less = [-1 for _ in range(n)]
        stk = []
        for i in range(n):
            x = heights[i]
            while stk and heights[stk[-1]] >= x:
                stk.pop()
            if stk:
                left_less[i] = stk[-1]
            stk.append(i)

        right_less = [n for _ in range(n)]
        stk = []
        for i in range(n - 1, -1, -1):
            x = heights[i]
            while stk and heights[stk[-1]] >= x:
                stk.pop()
            if stk:
                right_less[i] = stk[-1]
            stk.append(i)
        
        res = 0
        for i, h in enumerate(heights):
            cur = (right_less[i] - left_less[i] - 1) * h
            res = max(res, cur)
        return res

```

# [剑指 Offer II 057. 值和下标之差都在给定的范围内](https://leetcode-cn.com/problems/7WqeDu/)

> 给你一个整数数组 nums 和两个整数 k 和 t 。请你判断是否存在 两个不同下标 i 和 j，使得 abs(nums[i] - nums[j]) <= t ，同时又满足 abs(i - j) <= k 。
>
> 如果存在则返回 true，不存在返回 false。
>
>  
>
> 示例 1：
>
> 输入：nums = [1,2,3,1], k = 3, t = 0
> 输出：true

思路：有序集合+滑动窗口



```python
class Solution:
    def containsNearbyAlmostDuplicate(self, nums: List[int], k: int, t: int) -> bool:
        #有序集合
        from sortedcontainers import SortedList
        st = SortedList()
        for i,num in enumerate(nums):
            #有序集合
            st.add(num)
            index=bisect_left(st,num)
            if index<len(st)-1 and st[index+1]-st[index]<=t:return True
            if index>0 and st[index]-st[index-1]<=t:return True
            if len(st)>k:
                st.remove(nums[i-k])

        return False
```

# [剑指 Offer II 060. 出现频率最高的 k 个数字](https://leetcode-cn.com/problems/g5c51o/)

> 给定一个整数数组 nums 和一个整数 k ，请返回其中出现频率前 k 高的元素。可以按 任意顺序 返回答案。
>
>  
>
> 示例 1:
>
> 输入: nums = [1,1,1,2,2,3], k = 2
> 输出: [1,2]



思路：

1.建立哈希表

2.然后将哈希表排序

3.然后返回前k个值



```python
class Solution:
    def topKFrequent(self, nums: List[int], k: int) -> List[int]:
        num_freq = collections.Counter(nums)
        minHeap = []

        for x, f in num_freq.items():
            if len(minHeap) == k:
                if minHeap[0][0] < f:
                    heapq.heappop(minHeap)
                    heapq.heappush(minHeap, (f, x))
            else:
                heapq.heappush(minHeap, (f, x))
        
        res = []
        while minHeap:
            f, x = heapq.heappop(minHeap)
            res.append(x)
        return res
```

# 15

```python
class Solution:

  def threeSum(self, nums: List[int]) -> List[List[int]]:

​    

​    n=len(nums)

​    res=[]

​    if(not nums or n<3):

​      return []

​    nums.sort()

​    res=[]

​    for i in range(n):

​      if(nums[i]>0):

​        return res

​      if(i>0 and nums[i]==nums[i-1]):

​        continue

​      L=i+1

​      R=n-1

​      while(L<R):

​        if(nums[i]+nums[L]+nums[R]==0):

​          res.append([nums[i],nums[L],nums[R]])

​          while(L<R and nums[L]==nums[L+1]):

​            L=L+1

​          while(L<R and nums[R]==nums[R-1]):

​            R=R-1

​          L=L+1

​          R=R-1

​        elif(nums[i]+nums[L]+nums[R]>0):

​          R=R-1

​        else:

​          L=L+1

​    return res
```



# [16. 最接近的三数之和](https://leetcode-cn.com/problems/3sum-closest/)

```python
class Solution:
    def threeSumClosest(self, nums: List[int], target: int) -> int:
        nums.sort()
        n = len(nums)
        best = 10**7
        
        # 根据差值的绝对值来更新答案
        def update(cur):
            nonlocal best
            if abs(cur - target) < abs(best - target):
                best = cur
        
        # 枚举 a
        for i in range(n):
            # 保证和上一次枚举的元素不相等
            if i > 0 and nums[i] == nums[i - 1]:
                continue
            # 使用双指针枚举 b 和 c
            j, k = i + 1, n - 1
            while j < k:
                s = nums[i] + nums[j] + nums[k]
                # 如果和为 target 直接返回答案
                if s == target:
                    return target
                update(s)
                if s > target:
                    # 如果和大于 target，移动 c 对应的指针
                    k0 = k - 1
                    # 移动到下一个不相等的元素
                    while j < k0 and nums[k0] == nums[k]:
                        k0 -= 1
                    k = k0
                else:
                    # 如果和小于 target，移动 b 对应的指针
                    j0 = j + 1
                    # 移动到下一个不相等的元素
                    while j0 < k and nums[j0] == nums[j]:
                        j0 += 1
                    j = j0

        return best

```

# [35. 搜索插入位置](https://leetcode-cn.com/problems/search-insert-position/)

```python
func searchInsert(nums []int, target int) int {
    n := len(nums)
    left, right := 0, n - 1
    ans := n
    for left <= right {
        mid := (right - left) >> 1 + left
        if target <= nums[mid] {
            ans = mid
            right = mid - 1
        } else {
            left = mid + 1
        }
    }
    return ans
}


```
# [剑指 Offer II 061. 和最小的 k 个数对](https://leetcode-cn.com/problems/qn8gGX/)

> 给定两个以升序排列的整数数组 nums1 和 nums2 , 以及一个整数 k 。

> 定义一对值 (u,v)，其中第一个元素来自 nums1，第二个元素来自 nums2 。

> 请找到和最小的 k 个数对 (u1,v1),  (u2,v2)  ...  (uk,vk) 。

输入: nums1 = [1,7,11], nums2 = [2,4,6], k = 3
输出: [1,2],[1,4],[1,6]
解释: 返回序列中的前 3 对数：
    [1,2],[1,4],[1,6],[7,2],[7,4],[11,2],[7,6],[11,4],[11,6]

```python
class Solution:
    def kSmallestPairs(self, nums1: List[int], nums2: List[int], k: int) -> List[List[int]]:
        lst = []
        res = []
        for i in nums1:
            for j in nums2:
                lst.append((i,j,i+j))
        
        #构建出对值数组
        lst = sorted(lst,key = lambda x:x[2])
        if len(lst)<k:
            for i in range(len(lst)):
                res.append(list(lst[i])[0:2])
        else:
            for i in range(k):
                res.append(list(lst[i])[0:2])
        return res

```


# [700. 二叉搜索树中的搜索](https://leetcode-cn.com/problems/search-in-a-binary-search-tree/)

> 给定二叉搜索树（BST）的根节点 root 和一个整数值 val。
>
> 你需要在 BST 中找到节点值等于 val 的节点。 返回以该节点为根的子树。 如果节点不存在，则返回 null 。
>
>  
>
> 示例 1:
>
> 
>
> 输入：root = [4,2,7,1,3], val = 2
> 输出：[2,1,3]

```python
# Definition for a binary tree node.
# class TreeNode(object):
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution(object):
    def searchBST(self, root, val):
        """
        :type root: TreeNode
        :type val: int
        :rtype: TreeNode
        """
        if root is None:
            return None
        
        if val == root.val:
            return root

        return self.searchBST(root.left if val < root.val else root.right, val)
       
       
       # [剑指 Offer II 068. 查找插入位置](https://leetcode-cn.com/problems/N6YdxV/)

给定一个排序的整数数组 nums 和一个整数目标值 target ，请在数组中找到 target ，并返回其下标。如果目标值不存在于数组中，返回它将会被按顺序插入的位置。

请必须使用时间复杂度为 O(log n) 的算法。

 

示例 1:

输入: nums = [1,3,5,6], target = 5
输出: 2

> 二分

```python
# python3
class Solution:
    def searchInsert(self, nums: List[int], target: int) -> int:
        left,right = 0,len(nums)-1

        while left<=right:
            mid = (left+right)//2
            if nums[mid]==target:
                return mid
            elif nums[mid]<target:
                left=mid+1
            else:
                right=mid-1

        return left
```

# [剑指 Offer II 070. 排序数组中只出现一次的数字](https://leetcode-cn.com/problems/skFtm2/)

给定一个只包含整数的有序数组 nums ，每个元素都会出现两次，唯有一个数只会出现一次，请找出这个唯一的数字。

 

示例 1:

输入: nums = [1,1,2,3,3,4,4,8,8]
输出: 2
示例 2:

输入: nums =  [3,3,7,7,10,11,11]
输出: 10

```python
class Solution:
    def singleNonDuplicate(self, nums: List[int]) -> int:
        a = {}
        for i in nums:
            if i in a:
                a[i]+=1
            else:
                a[i] = 1

        
        for i in a.keys():
            if a[i]==1:
                return i 
                
```

# [剑指 Offer II 073. 狒狒吃香蕉](https://leetcode-cn.com/problems/nZZqjQ/)

python向上取整

math.ceil()

狒狒喜欢吃香蕉。这里有 N 堆香蕉，第 i 堆中有 piles[i] 根香蕉。警卫已经离开了，将在 H 小时后回来。

狒狒可以决定她吃香蕉的速度 K （单位：根/小时）。每个小时，她将会选择一堆香蕉，从中吃掉 K 根。如果这堆香蕉少于 K 根，她将吃掉这堆的所有香蕉，然后这一小时内不会再吃更多的香蕉，下一个小时才会开始吃另一堆的香蕉。  

狒狒喜欢慢慢吃，但仍然想在警卫回来前吃掉所有的香蕉。

返回她可以在 H 小时内吃掉所有香蕉的最小速度 K（K 为整数）。

 

示例 1：

输入: piles = [3,6,7,11], H = 8
输出: 4

```python
#超时算法
class Solution:
    def minEatingSpeed(self, piles: List[int], h: int) -> int:
        b = sorted(piles)
        k = 0
        hour = 0
        while hour!=h:
            k+=1
            hour = 0
            for i in b:
                hour+=math.ceil(i/k)
            # print(hour)    
            

        return k
```

```python
from math import ceil
class Solution:
    def minEatingSpeed(self, piles: List[int], h: int) -> int:
        def is_finish_eating(k):
            temp_h = 0
            for i in range(len(piles)):
                temp_h += ceil(piles[i] / k)
                if temp_h > h:
                    return False
            return True

        left = 1
        right = max(piles)
        while not left == right:
            mid = (left + right) // 2
            if not is_finish_eating(mid):
                left = mid + 1
            else:
                right = mid
        return left

```


# [剑指 Offer II 074. 合并区间](https://leetcode-cn.com/problems/SsGoHC/)

以数组 intervals 表示若干个区间的集合，其中单个区间为 intervals[i] = [starti, endi] 。请你合并所有重叠的区间，并返回一个不重叠的区间数组，该数组需恰好覆盖输入中的所有区间。

 

示例 1：

输入：intervals = [[1,3],[2,6],[8,10],[15,18]]
输出：[[1,6],[8,10],[15,18]]
解释：区间 [1,3] 和 [2,6] 重叠, 将它们合并为 [1,6].

贪心或者其他

```python
class Solution:
    def merge(self, intervals: List[List[int]]) -> List[List[int]]:
        # 根据左区间进行排序
        intervals.sort(key=lambda x: x[0])

        merged = []
        for interval in intervals:
            # 如果列表为空，或者当前区间与上一区间不重合，直接添加
            if not merged or merged[-1][1] < interval[0]:
                merged.append(interval)
            else:
                # 否则的话，我们就可以与上一区间进行合并
                merged[-1][1] = max(merged[-1][1], interval[1])

        return merged
```

       
       public class Offer076 {
    public int findKthLargest(int[] nums, int k) {
        // PriorityQueue，优先队列的底层使用了堆原理，可以直接当做堆使用，默认为小根堆
        // PriorityQueue的保存方法是用数组进行保存的，
        // 在已知堆大小的时候为其赋值初始化容量可以减少扩容时的消耗
        PriorityQueue<Integer> heap = new PriorityQueue<>(k);
        // 先将 k 个元素放入堆中
        // 题目保证了 k <= nums.length，如果没有，不可以直接遍历，容易造成越界
        // 可以使用 for(int i = 0,end = Math.min(k, nums.length); i < end; i++) 的方式避免越界
        for (int i = 0; i < k; i++) {
            heap.add(nums[i]);
        }
        // 题目中保证了 k >= 1，也就是说堆一定不为空，如果没有，
        // 在进行heap.peek()，heap.poll()操作前，都必须要进行heap.isEmpty()的判断才可以
        for (int i = k, length = nums.length; i < length; i++) {
            if (heap.peek() < nums[i]) {
                heap.poll();
                heap.add(nums[i]);
            }
        }
        return heap.poll();
    }
}
53. 最大子数组和

```go
func maxSubArray(nums []int) int {
    max := nums[0]
    for i := 1; i < len(nums); i++ {
        if nums[i] + nums[i-1] > nums[i] {
            nums[i] += nums[i-1]
        }
        if nums[i] > max {
            max = nums[i]
        }
    }
    return max
}

# [剑指 Offer II 077. 链表排序](https://leetcode-cn.com/problems/7WHec2/)

给定链表的头结点 head ，请将其按 升序 排列并返回 排序后的链表 。

 

示例 1：



输入：head = [4,2,1,3]
输出：[1,2,3,4]

把链表的结点存放list

排序后重新建表

```python
# Definition for singly-linked list.
# class ListNode:
#     def __init__(self, val=0, next=None):
#         self.val = val
#         self.next = next
class Solution:
    def sortList(self, head: ListNode) -> ListNode:
        if not head:
            return None
        phead = head
        alist = []

        while phead:
            alist.append(phead)
            phead = phead.next
        
        alist.sort(key=lambda x:x.val)
        root = alist[0]
        root.next = None
        cur = root
        for i in range(1,len(alist)):
            alist[i].next = None
            cur.next = alist[i]
            cur = alist[i]

        return root
```

# [剑指 Offer II 079. 所有子集](https://leetcode-cn.com/problems/TVdhkn/)

给定一个整数数组 nums ，数组中的元素 互不相同 。返回该数组所有可能的子集（幂集）。

解集 不能 包含重复的子集。你可以按 任意顺序 返回解集。

 

示例 1：

输入：nums = [1,2,3]
输出：[[],[1],[2],[1,2],[3],[1,3],[2,3],[1,2,3]]

画一遍递归树



```python
class Solution:
    def subsets(self, nums: List[int]) -> List[List[int]]: 
        ans = []
        def dfs(n,s,cur):
            if n==len(cur):
                ans.append(cur.copy())
                return

            for i in range(s,len(nums)):
                cur.append(nums[i])
                dfs(n,i+1,cur)
                cur.pop()

        for i in range(len(nums) + 1):
            dfs(i,0,[])
        return ans
```

INT_MAX = 2 ** 31 - 1
INT_MIN = -2 ** 31

class Automaton:
    def __init__(self):
        self.state = 'start'
        self.sign = 1
        self.ans = 0
        self.table = {
            'start': ['start', 'signed', 'in_number', 'end'],
            'signed': ['end', 'end', 'in_number', 'end'],
            'in_number': ['end', 'end', 'in_number', 'end'],
            'end': ['end', 'end', 'end', 'end'],
        }
        
    def get_col(self, c):
        if c.isspace():
            return 0
        if c == '+' or c == '-':
            return 1
        if c.isdigit():
            return 2
        return 3

    def get(self, c):
        self.state = self.table[self.state][self.get_col(c)]
        if self.state == 'in_number':
            self.ans = self.ans * 10 + int(c)
            self.ans = min(self.ans, INT_MAX) if self.sign == 1 else min(self.ans, -INT_MIN)
        elif self.state == 'signed':
            self.sign = 1 if c == '+' else -1

class Solution:
    def myAtoi(self, str: str) -> int:
        automaton = Automaton()
        for c in str:
            automaton.get(c)
        return automaton.sign * automaton.ans

