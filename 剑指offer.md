# 剑指offer

#### [剑指 Offer 09. 用两个栈实现队列](https://leetcode-cn.com/problems/yong-liang-ge-zhan-shi-xian-dui-lie-lcof/)

没有用两个栈，只是用了一个队列

```python
class CQueue(object):

    def __init__(self):
        self.item = []


    def appendTail(self, value):
        """
        :type value: int
        :rtype: None
        """
        self.item.append(value)


    def deleteHead(self):
        """
        :rtype: int
        """
        if len(self.item) == 0:
            return -1
        else:
            out = self.item[0]
            del(self.item[0])
            return out



# Your CQueue object will be instantiated and called as such:
# obj = CQueue()
# obj.appendTail(value)
# param_2 = obj.deleteHead()

执行用时：1268 ms, 在所有 Python 提交中击败了52.11%的用户
内存消耗：17.2 MB, 在所有 Python 提交中击败了98.00%的用户
通过测试用例：55 / 55
```

两个栈的解法

```python
class CQueue(object):
    '''
    利用两个栈实现，第一个栈stack1执行入队操作，stack2执行出队操作
    1、当入队时，向stack1中插入元素
    2、当出队时，如果stack2中有元素，弹出栈顶
                如果没有，将stack1中的元素，全部出栈，放入stack2中，再弹出栈顶
    3、当stack1与stack2均为空时，返回-1
    '''

    def __init__(self):
        self.stack1 = []
        self.stack2 = []


    def appendTail(self, value):
        """
        :type value: int
        :rtype: None
        """
        self.stack1.append(value)


    def deleteHead(self):
        """
        :rtype: int
        """
        if self.stack2:
            return self.stack2.pop()
        if not self.stack1:
            return -1
        while self.stack1:
            self.stack2.append(self.stack1.pop())
        return self.stack2.pop()

         


# Your CQueue object will be instantiated and called as such:
# obj = CQueue()
# obj.appendTail(value)
# param_2 = obj.deleteHead()
执行用时：1248 ms, 在所有 Python 提交中击败了68.14%的用户
内存消耗：17.8 MB, 在所有 Python 提交中击败了5.24%的用户
通过测试用例：55 / 55
```

#### [剑指 Offer 30. 包含min函数的栈](https://leetcode-cn.com/problems/bao-han-minhan-shu-de-zhan-lcof/)

运用两个栈，其中一个是正常的栈，第二个栈只有在空或者输入数字小于或者等于栈顶元素才将数字压入栈顶，如此形成最小元素排序栈。当正常栈输出时，检查最小栈栈顶元素是否与输出相同，相同则最小栈也输出。

```python
class MinStack(object):

    def __init__(self):
        """
        initialize your data structure here.
        """
        self.stack1, self.stack2 = [], []


    def push(self, x):
        """
        :type x: int
        :rtype: None
        """
        self.stack1.append(x)
        if not self.stack2 or self.stack2[-1] >= x:
            self.stack2.append(x)


    def pop(self):
        """
        :rtype: None
        """
        if self.stack1.pop() == self.stack2[-1]:
            self.stack2.pop()


    def top(self):
        """
        :rtype: int
        """
        return self.stack1[-1]


    def min(self):
        """
        :rtype: int
        """
        return self.stack2[-1]



# Your MinStack object will be instantiated and called as such:
# obj = MinStack()
# obj.push(x)
# obj.pop()
# param_3 = obj.top()
# param_4 = obj.min()

执行用时：108 ms, 在所有 Python 提交中击败了79.36%的用户
内存消耗：16.9 MB, 在所有 Python 提交中击败了12.09%的用户
通过测试用例：
19 / 19
```

#### [剑指 Offer 03. 数组中重复的数字](https://leetcode-cn.com/problems/shu-zu-zhong-zhong-fu-de-shu-zi-lcof/)

哈希表，当表中存在重复元素时直接输出。

```python
class Solution(object):
    def findRepeatNumber(self, nums):
        """
        :type nums: List[int]
        :rtype: int
        """
        hash_map = set()
        for num in nums:
            if num not in hash_map:
                hash_map.add(num)
            else:
                return num
                
执行用时：24 ms, 在所有 Python 提交中击败了87.44%的用户
内存消耗：21.4 MB, 在所有 Python 提交中击败了18.47%的用户
通过测试用例：25 / 25
```

由于题目限定长度为n的数组里面数字范围在0 —— n-1之间，所以如果有重复数字，当我们把数字与其下标一一对应的时候，第二次出现的重复数字会发现在该数字为下标的位置已经有数字存在了。

```python
class Solution(object):
    def findRepeatNumber(self, nums):
        """
        :type nums: List[int]
        :rtype: int
        """
        i = 0
        while i < len(nums):
            if nums[i] == i:
                i += 1
                continue
            if nums[nums[i]] == nums[i]:
                return nums[i]
            else:
                nums[nums[i]], nums[i] = nums[i], nums[nums[i]]
        return -1


执行用时：36 ms, 在所有 Python 提交中击败了33.48%的用户
内存消耗：18.9 MB, 在所有 Python 提交中击败了91.88%的用户
通过测试用例：25 / 25
```

#### [剑指 Offer 06. 从尾到头打印链表](https://leetcode-cn.com/problems/cong-wei-dao-tou-da-yin-lian-biao-lcof/)

将链表按顺序打印出来到数组，再将数组反转。

```python
# Definition for singly-linked list.
# class ListNode(object):
#     def __init__(self, x):
#         self.val = x
#         self.next = None

class Solution(object):
    def reversePrint(self, head):
        """
        :type head: ListNode
        :rtype: List[int]
        """
        out = []
        while head:
            out.append(head.val)
            head = head.next
        out = out[::-1]
        return out
   
执行用时：24 ms, 在所有 Python 提交中击败了67.23%的用户
内存消耗：16.4 MB, 在所有 Python 提交中击败了71.31%的用户
通过测试用例：24 / 24
```

递归算法

```python
# Definition for singly-linked list.
# class ListNode(object):
#     def __init__(self, x):
#         self.val = x
#         self.next = None

class Solution(object):
    def reversePrint(self, head):
        """
        :type head: ListNode
        :rtype: List[int]
        """
        if not head:
            return []
        else:
            return self.reversePrint(head.next) + [head.val]

执行用时：108 ms, 在所有 Python 提交中击败了7.88%的用户
内存消耗：22.8 MB, 在所有 Python 提交中击败了15.44%的用户
通过测试用例：24 / 24
```

**链表特点：** 只能从前至后访问每个节点。
**题目要求：** 倒序输出节点值。
这种 **先入后出** 的需求可以借助 **栈** 来实现

```python
# Definition for singly-linked list.
# class ListNode(object):
#     def __init__(self, x):
#         self.val = x
#         self.next = None

class Solution(object):
    def reversePrint(self, head):
        """
        :type head: ListNode
        :rtype: List[int]
        """
        stack = []
        out = []
        while head:
            stack.append(head.val)
            head = head.next
        while stack:
            out.append(stack.pop())
        return out

执行用时：24 ms, 在所有 Python 提交中击败了67.23%的用户
内存消耗：16.5 MB, 在所有 Python 提交中击败了51.09%的用户
通过测试用例：24 / 24
```

#### [剑指 Offer 24. 反转链表](https://leetcode-cn.com/problems/fan-zhuan-lian-biao-lcof/)

暴力法，利用栈先进后出的特性保存反转链表，再逐个pop出到新的链表。

```python
# Definition for singly-linked list.
# class ListNode(object):
#     def __init__(self, x):
#         self.val = x
#         self.next = None

class Solution(object):
    def reverseList(self, head):
        """
        :type head: ListNode
        :rtype: ListNode
        """
        if head == None:
            return head
        rhead = None
        cur = None
        stack = []
        while head:
            stack.append(head.val)
            head = head.next
        rhead = ListNode(stack.pop()) #最后要输出表头，所以引入另一个指针滑动
        cur = rhead
        while stack:
            cur.next = ListNode(stack.pop())
            cur = cur.next            
        return rhead
执行用时：16 ms, 在所有 Python 提交中击败了90.53%的用户
内存消耗：16.6 MB, 在所有 Python 提交中击败了16.52%的用户
通过测试用例：27 / 27
```

以下为大佬解法https://leetcode-cn.com/problems/fan-zhuan-lian-biao-lcof/solution/dong-hua-yan-shi-duo-chong-jie-fa-206-fan-zhuan-li/

双指针迭代
我们可以申请两个指针，第一个指针叫 pre，最初是指向 null 的。
第二个指针 cur 指向 head，然后不断遍历 cur。
每次迭代到 cur，都将 cur 的 next 指向 pre，然后 pre 和 cur 前进一位。
都迭代完了(cur 变成 null 了)，pre 就是最后一个节点了。

```python
# Definition for singly-linked list.
# class ListNode(object):
#     def __init__(self, x):
#         self.val = x
#         self.next = None

class Solution(object):
    def reverseList(self, head):
        """
        :type head: ListNode
        :rtype: ListNode
        """
        pre = None
        cur = head
        while cur:
            tmp = cur.next
            cur.next = pre
            pre = cur
            cur = tmp
        return pre
执行用时：20 ms, 在所有 Python 提交中击败了69.31%的用户
内存消耗：14.9 MB, 在所有 Python 提交中击败了69.11%的用户
通过测试用例：27 / 27
```

迭代法

终止条件是当前节点或者下一个节点==null
在函数内部，改变节点的指向，也就是 head 的下一个节点指向 head 递归函数那句

head.next.next = head

```python
# Definition for singly-linked list.
# class ListNode(object):
#     def __init__(self, x):
#         self.val = x
#         self.next = None

class Solution(object):
    def reverseList(self, head):
        """
        :type head: ListNode
        :rtype: ListNode
        """
        if head == None or head.next == None:
            return head
        cur = self.reverseList(head.next)
        head.next.next = head
        head.next = None
        return cur
执行用时：28 ms, 在所有 Python 提交中击败了10.41%的用户
内存消耗：18.2 MB, 在所有 Python 提交中击败了12.35%的用户
通过测试用例：27 / 27
```

#### [剑指 Offer 35. 复杂链表的复制](https://leetcode-cn.com/problems/fu-za-lian-biao-de-fu-zhi-lcof/)

参考链接：https://leetcode-cn.com/problems/fu-za-lian-biao-de-fu-zhi-lcof/solution/lian-biao-de-shen-kao-bei-by-z1m/

深度优先搜索
从头结点 head 开始拷贝；
由于一个结点可能被多个指针指到，因此如果该结点已被拷贝，则不需要重复拷贝；
如果还没拷贝该结点，则创建一个新的结点进行拷贝，并将拷贝过的结点保存在哈希表中（哈希表中存储的是拷贝后的节点，不是原节点）；
使用递归拷贝所有的 next 结点，再递归拷贝所有的 random 结点（next可以保证一遍遍历所有节点直到None）。

```python
"""
# Definition for a Node.
class Node:
    def __init__(self, x, next=None, random=None):
        self.val = int(x)
        self.next = next
        self.random = random
"""
class Solution(object):
    def copyRandomList(self, head):
        """
        :type head: Node
        :rtype: Node
        """
        def digui(head):
            if head == None:
                return head
            if head in hash_map:
                return hash_map[head]
            newnode = Node(head.val, None, None)
            hash_map[head] = newnode
            newnode.next = digui(head.next)
            # next的遍历已经搜索完所有的node，故哈希表中保存有所有node的副本，除了None的情况
            if head.random:
                newnode.random = hash_map[head.random]
            else:
                newnode.random = None
            return newnode
        hash_map = {}
        return digui(head)
执行用时：32 ms, 在所有 Python 提交中击败了94.76%的用户
内存消耗：14.7 MB, 在所有 Python 提交中击败了6.22%的用户
通过测试用例：18 / 18
```

广度优先搜索

```python
"""
# Definition for a Node.
class Node:
    def __init__(self, x, next=None, random=None):
        self.val = int(x)
        self.next = next
        self.random = random
"""
class Solution(object):
    def copyRandomList(self, head):
        """
        :type head: Node
        :rtype: Node
        """
        hash_map = {}
        def bfs(head):
            if not head:
                return head
            clone = Node(head.val,None,None)
            queue = collections.deque()
            queue.append(head)
            hash_map[head] = clone
            while queue:
                temp = queue.pop()
                if temp.next and temp.next not in hash_map:
                    hash_map[temp.next] = Node(temp.next.val, None, None)
                    queue.append(temp.next)
                if temp.random and temp.random not in hash_map:
                    hash_map[temp.random] = Node(temp.random.val, None, None)
                    queue.append(temp.random)
                hash_map[temp].next = hash_map.get(temp.next)
                hash_map[temp].random = hash_map.get(temp.random)
            return clone
        return bfs(head)

执行用时：40 ms, 在所有 Python 提交中击败了47.56%的用户
内存消耗：13.9 MB, 在所有 Python 提交中击败了31.83%的用户
通过测试用例：18 / 18
```

我们也可以不使用哈希表的额外空间来保存已经拷贝过的结点，而是将链表进行拓展，在每个链表结点的旁边拷贝，比如 A->B->C 变成 A->A'->B->B'->C->C'，然后将拷贝的结点分离出来变成 A->B->C和A'->B'->C'，最后返回 A'->B'->C'。

![35_1.gif](https://pic.leetcode-cn.com/c53b7c728bcf064803cefc137766e5dbfa0247059ed8adf76a86d7e3f2de7546-35_1.gif)

```Python
"""
# Definition for a Node.
class Node:
    def __init__(self, x, next=None, random=None):
        self.val = int(x)
        self.next = next
        self.random = random
"""
class Solution(object):
    def copyRandomList(self, head):
        """
        :type head: Node
        :rtype: Node
        """
        if not head:
            return head
        cur = head
        while cur:
            new_node = Node(cur.val, None, None)
            new_node.next = cur.next
            cur.next = new_node
            cur = new_node.next
        cur = head
        while cur:
            cur.next.random = cur.random.next if cur.random else None
            cur = cur.next.next
        cur_old_list = head
        cur_new_list = head.next
        new_head = head.next
        while cur_old_list:
            cur_old_list.next = cur_old_list.next.next
            cur_new_list.next = cur_new_list.next.next if cur_new_list.next else None
            cur_old_list = cur_old_list.next
            cur_new_list = cur_new_list.next
        return new_head

    
执行用时：32 ms, 在所有 Python 提交中击败了94.76%的用户
内存消耗：13.8 MB, 在所有 Python 提交中击败了42.68%的用户
通过测试用例：18 / 18
```

#### [剑指 Offer 05. 替换空格](https://leetcode-cn.com/problems/ti-huan-kong-ge-lcof/)

简单粗暴：直接遍历整个字符串，遇到空格直接换成指定字符。

```python
class Solution(object):
    def replaceSpace(self, s):
        """
        :type s: str
        :rtype: str
        """
        out = str()
        r = "%20"
        for char in s:
            if char == ' ':
                out = out + r
            else:
                out = out + char
        return out
执行用时：16 ms, 在所有 Python 提交中击败了65.76%的用户
内存消耗：12.9 MB, 在所有 Python 提交中击败了87.00%的用户
通过测试用例：27 / 27
```

内置函数replace()，不知道会不会被打😂😂😂

```python
class Solution(object):
    def replaceSpace(self, s):
        """
        :type s: str
        :rtype: str
        """
        return s.replace(' ', '%20')
执行用时：8 ms, 在所有 Python 提交中击败了98.69%的用户
内存消耗：13 MB, 在所有 Python 提交中击败了76.05%的用户
通过测试用例：27 / 27
```

#### [剑指 Offer 58 - II. 左旋转字符串](https://leetcode-cn.com/problems/zuo-xuan-zhuan-zi-fu-chuan-lcof/)

简单粗暴的划分重组

```python
class Solution(object):
    def reverseLeftWords(self, s, n):
        """
        :type s: str
        :type n: int
        :rtype: str
        """
        return s[n:] + s[:n]
执行用时：24 ms, 在所有 Python 提交中击败了31.71%的用户
内存消耗：13.1 MB, 在所有 Python 提交中击败了92.65%的用户
通过测试用例：34 / 34
```

遍历重组

```python
class Solution(object):
    def reverseLeftWords(self, s, n):
        """
        :type s: str
        :type n: int
        :rtype: str
        """
        out = []
        for i in range(n, n+len(s)):
            out.append(s[i%len(s)])
        return ''.join(out)
执行用时：24 ms, 在所有 Python 提交中击败了31.71%的用户
内存消耗：13.9 MB, 在所有 Python 提交中击败了5.30%的用户
通过测试用例：34 / 34
```

#### [剑指 Offer 03. 数组中重复的数字](https://leetcode-cn.com/problems/shu-zu-zhong-zhong-fu-de-shu-zi-lcof/)

由于指定了数字不会超出数组的长度，可以在遍历数组的时候将数字交换到对应的下标位置。交换的规则是如果数字和以他作为下标的数组存储数字不相等，则交换。然后检查当前下标是否和下标处数字对应，没有就继续交换。由于有重复数字，当我们遇到需要交换的数字和以他为下标的数组存储的数字相等时，说明找到了重复的数字，返回该数字。

```python
class Solution(object):
    def findRepeatNumber(self, nums):
        """
        :type nums: List[int]
        :rtype: int
        """
        i = 0
        while i < len(nums):
            if nums[i] == i:
                i += 1
                continue
            if nums[nums[i]] == nums[i]:
                return nums[i]
            else:
                nums[nums[i]], nums[i] = nums[i], nums[nums[i]]
        return -1
执行用时：24 ms, 在所有 Python 提交中击败了87.25%的用户
内存消耗：18.8 MB, 在所有 Python 提交中击败了99.45%的用户
通过测试用例：25 / 25
```

#### [剑指 Offer 53 - I. 在排序数组中查找数字 I](https://leetcode-cn.com/problems/zai-pai-xu-shu-zu-zhong-cha-zhao-shu-zi-lcof/)

遍历数组，先判断是否为空，接着判断第一个值（最小）和最后一个值（最大）的情况，最后再开始遍历比较，遇到大的就停止。

```Python
class Solution(object):
    def search(self, nums, target):
        """
        :type nums: List[int]
        :type target: int
        :rtype: int
        """
        num = 0
        if not nums:
            return num
        if nums[0] > target or nums[-1] < target:
            return num
        for i in nums:
            if i < target:
                continue
            elif i == target:
                num += 1
            else:
                break
        return num
执行用时：12 ms, 在所有 Python 提交中击败了98.08%的用户
内存消耗：13.3 MB, 在所有 Python 提交中击败了72.92%的用户
通过测试用例：88 / 88
```

#### [剑指 Offer 53 - II. 0～n-1中缺失的数字](https://leetcode-cn.com/problems/que-shi-de-shu-zi-lcof/)

由于是排序的数组且数字唯一，可以遍历数组，当下标与数字不一致的时候，此时下标就是缺失的数字。如果所有都一致，那就是缺失数组长度的数字（数组长度为n，下标由0开始，最大就是n-1，没有n）。

```python
class Solution(object):
    def missingNumber(self, nums):
        """
        :type nums: List[int]
        :rtype: int
        """
        for i in range(len(nums)):
            if nums[i] != i:
                return i
        return len(nums)
执行用时：20 ms, 在所有 Python 提交中击败了72.94%的用户
内存消耗：13.9 MB, 在所有 Python 提交中击败了68.72%的用户
通过测试用例：122 / 122
```

二分法，一样是判断下标和数字对应不对应。

```Python
class Solution(object):
    def missingNumber(self, nums):
        """
        :type nums: List[int]
        :rtype: int
        """
        i, j = 0, len(nums) - 1
        while i <= j:
            m = (i + j) // 2
            if nums[m] == m: i = m + 1  #如果数组内所有数字都和下标对应，则i,j会相等，最后一步会使得i+1>j，此时刚好i就是数组长度，即缺失值
            else: j = m - 1
        return i
执行用时：12 ms, 在所有 Python 提交中击败了99.07%的用户
内存消耗：14 MB, 在所有 Python 提交中击败了23.15%的用户
通过测试用例：122 / 122
```

#### [剑指 Offer 04. 二维数组中的查找](https://leetcode-cn.com/problems/er-wei-shu-zu-zhong-de-cha-zhao-lcof/)

参考：https://leetcode-cn.com/problems/er-wei-shu-zu-zhong-de-cha-zhao-lcof/solution/gao-su-jie-fa-qing-xi-tu-jie-by-ml-zimingmeng/

```Python
class Solution(object):
    def findNumberIn2DArray(self, matrix, target):
        """
        :type matrix: List[List[int]]
        :type target: int
        :rtype: bool
        """
        if not matrix:
            return False
        row, col = len(matrix), len(matrix[0])
        n, m = row-1, 0
        while n >= 0 and m < col:#从左下角开始，比它小，则这一整行都不可能是，往上一行找。比它大，要么在这一列中，要么没有。
            if matrix[n][m] == target:
                return True
            elif matrix[n][m] > target:
                n -= 1
            else:
                m += 1
        return False
执行用时：24 ms, 在所有 Python 提交中击败了63.28%的用户
内存消耗：17.3 MB, 在所有 Python 提交中击败了31.82%的用户
通过测试用例：129 / 129
```

复杂度分析
时间复杂度：O(m+n)。m 和 n 分别为行数和列数。最坏情况下，我们从左下角移动到右上角，经过的路径长度为。
空间复杂度：O(1)。

二分查找：分行列，以对角线移动

```Python
class Solution(object):
    def findNumberIn2DArray(self, matrix, target):
        """
        :type matrix: List[List[int]]
        :type target: int
        :rtype: bool
        """
        def binary_search(matrix, target, start, vertical):
            loc = start
            hi = len(matrix) - 1 if vertical else len(matrix[0]) - 1
            while loc <= hi:
                mid = (loc+hi) // 2
                if vertical:
                    if matrix[mid][start] > target:
                        hi = mid - 1
                    elif matrix[mid][start] <target:
                        loc = mid + 1
                    else:
                        return True
                else:
                    if matrix[start][mid] > target:
                        hi = mid - 1
                    elif matrix[start][mid] < target:
                        loc = mid + 1
                    else:
                        return True
            return False
        if not matrix:
            return False
        for i in range(min(len(matrix), len(matrix[0]))):
            vertical_found = binary_search(matrix, target, i, True)
            horizontal_found = binary_search(matrix, target, i, False)
            if vertical_found or horizontal_found:
                return True
        return False
执行用时：24 ms, 在所有 Python 提交中击败了63.28%的用户
内存消耗：17.2 MB, 在所有 Python 提交中击败了57.76%的用户
通过测试用例：129 / 129
```

复杂度分析

 if min(n,m)\==n：(lgn+lgm)+(lg(n-1)+lg(m-1))+(lg1+lg(m-n+1)) =lgn!+lg(m!/n!) =lg(n!*(m!/n!)) =lg(m!) 

同理：if min(n,m)\==m：lg(n!) 因此时间复杂度为O(lgk),k=max(n,m)。

- 时间复杂度：O(lgk),k=max(n,m)
- 空间复杂度：O(1)。



递归

在 mid列寻找满足条件 matrix\[row - 1][mid]<target<matrix\[row][mid]的点，比如当 row=3,mid=2时（黄色区域），9<target<14，这时我们可以判断出来 target一定在左下或者右上区域：

由 target>9，可知 target 在 9 的右侧或下侧；
由 target<14，可知 target 在14 的上侧或左侧；
因此对左下和右上两个区域进行递归，直到遇到终止条件进行回溯，返回结果。 终止条件为：

区域中没有元素；
targettarget 大于深色区域右下角的值（最大值）或小于深色区域左上角的值（最小值）
其中，找到黄色点的方法如下：

列索引 mid 采用二分查找;
行索引沿 mid列从上向下移动，并保持该位置元素小于 target。

![TIM截图20200229160734.png](https://pic.leetcode-cn.com/00917701153b12d2819e2e0ed681737390b8e5bd26ea1d0c6abd7dfe87c8c927-TIM%E6%88%AA%E5%9B%BE20200229160734.png)

```python
class Solution(object):
    def findNumberIn2DArray(self, matrix, target):
        """
        :type matrix: List[List[int]]
        :type target: int
        :rtype: bool
        """
        if not matrix:
            return False
        def search_backtrack(left, up, right, down):
            if left > right or up > down:
                return False
            elif target < matrix[up][left] or target > matrix[down][right]:
                return False

            mid = left + (right - left) // 2

            row = up
            while row <= down and matrix[row][mid] <= target:
                if matrix[row][mid] == target:
                    return True
                row += 1

            return search_backtrack(left, row, mid - 1, down) or search_backtrack(mid + 1, up, right, row - 1)

        return search_backtrack(0, 0, len(matrix[0]) - 1, len(matrix) - 1)
执行用时：24 ms, 在所有 Python 提交中击败了63.28%的用户
内存消耗：17.4 MB, 在所有 Python 提交中击败了6.90%的用户
通过测试用例：129 / 129
```

复杂度分析

- 时间复杂度：O(nlgn)。
- 空间复杂度：O(lgn)。

#### [剑指 Offer 11. 旋转数组的最小数字](https://leetcode-cn.com/problems/xuan-zhuan-shu-zu-de-zui-xiao-shu-zi-lcof/)

简单粗暴：由于是排序数组的旋转，则遇到反序的第一个数字就是，如果没有反序，则是第一个。首先检测只有一个数字的情况。

```Python
class Solution(object):
    def minArray(self, numbers):
        """
        :type numbers: List[int]
        :rtype: int
        """
        if len(numbers) == 1:
            return numbers[0]
        for i in range(len(numbers)-1):
            if numbers[i+1] < numbers[i]:
                return numbers[i+1]
        return numbers[0]
执行用时：24 ms, 在所有 Python 提交中击败了22.11%的用户
内存消耗：13.5 MB, 在所有 Python 提交中击败了20.03%的用户
通过测试用例：192 / 192
```

优化一下

```python
class Solution(object):
    def minArray(self, numbers):
        """
        :type numbers: List[int]
        :rtype: int
        """
        left = numbers[0]
        for i in numbers[1:]:
            if i < left:
                return i
        return left
执行用时：16 ms, 在所有 Python 提交中击败了86.29%的用户
内存消耗：13.4 MB, 在所有 Python 提交中击败了41.60%的用户
通过测试用例：192 / 192
```

二分法

```python
class Solution(object):
    def minArray(self, numbers):
        """
        :type numbers: List[int]
        :rtype: int
        """
        left, right = 0, len(numbers)-1
        while left < right:
            mid = (left + right) // 2
            if numbers[mid] > numbers[right]:#寻找最小值，由于mid>right，所以mid本身不可能是最小值
                left = mid + 1
            elif numbers[mid] < numbers[right]:#由于mid<right，所以mid本身可能是最小值，不能排除mid
                right = mid
            else:#二者相等，则不能判断在何处。按照排序数组的规律，最右边应该是最大的，寻找最小值，最右边界往回缩。
                right -= 1
        return numbers[left]
执行用时：20 ms, 在所有 Python 提交中击败了57.39%的用户
内存消耗：13.2 MB, 在所有 Python 提交中击败了83.80%的用户
通过测试用例：192 / 192
```

#### [剑指 Offer 50. 第一个只出现一次的字符](https://leetcode-cn.com/problems/di-yi-ge-zhi-chu-xian-yi-ci-de-zi-fu-lcof/)

简单粗暴：两个表，一个表保存出现过的字母，另一个表只保存出现一次的字母。

```python
class Solution(object):
    def firstUniqChar(self, s):
        """
        :type s: str
        :rtype: str
        """
        if not s:
            return ' '
        appears = []
        once = []
        for char in s:
            if char in appears:
                if char in once:
                    once.remove(char)
                continue
            else:
                once.append(char)
                appears.append(char)
        if not once:
            return ' '
        else:
            return once[0]
执行用时：136 ms, 在所有 Python 提交中击败了39.27%的用户
内存消耗：13.5 MB, 在所有 Python 提交中击败了35.27%的用户
通过测试用例：104 / 104
```

哈希表：遍历字符串，建立字符和字符出现位置键值对的哈希表。当出现重复字符时，将键值改为-1。遍历哈希表键值，找出不为-1的最小值。

```python 
class Solution(object):
    def firstUniqChar(self, s):
        """
        :type s: str
        :rtype: str
        """
        if not s:
            return ' '
        appears = {}
        for i, char in enumerate(s):
            if char in appears:
                appears[char] = -1
            else:
                appears[char] = i
        first = len(s)
        for pos in appears.values():
            if pos != -1 and pos < first:
                first = pos
        return ' ' if first == len(s) else s[first]
执行用时：60 ms, 在所有 Python 提交中击败了88.85%的用户
内存消耗：13.6 MB, 在所有 Python 提交中击败了25.94%的用户
通过测试用例：
104 / 104
```

方法三：队列
思路与算法

我们也可以借助队列找到第一个不重复的字符。队列具有「先进先出」的性质，因此很适合用来找出第一个满足某个条件的元素。

具体地，我们使用与方法二相同的哈希映射，并且使用一个额外的队列，按照顺序存储每一个字符以及它们第一次出现的位置。当我们对字符串进行遍历时，设当前遍历到的字符为 cc，如果 cc 不在哈希映射中，我们就将 cc 与它的索引作为一个二元组放入队尾，否则我们就需要检查队列中的元素是否都满足「只出现一次」的要求，即我们不断地根据哈希映射中存储的值（是否为 -1−1）选择弹出队首的元素，直到队首元素「真的」只出现了一次或者队列为空。

在遍历完成后，如果队列为空，说明没有不重复的字符，返回空格，否则队首的元素即为第一个不重复的字符以及其索引的二元组。

小贴士

在维护队列时，我们使用了「延迟删除」这一技巧。也就是说，即使队列中有一些字符出现了超过一次，但它只要不位于队首，那么就不会对答案造成影响，我们也就可以不用去删除它。只有当它前面的所有字符被移出队列，它成为队首时，我们才需要将它移除。
链接：https://leetcode-cn.com/problems/di-yi-ge-zhi-chu-xian-yi-ci-de-zi-fu-lcof/solution/di-yi-ge-zhi-chu-xian-yi-ci-de-zi-fu-by-3zqv5/

```python
class Solution(object):
    def firstUniqChar(self, s):
        """
        :type s: str
        :rtype: str
        """
        if not s:
            return ' '
        position = {}
        q = collections.deque()
        for i, char in enumerate(s):
            if char not in position:
                position[char] = i
                q.append(s[i])
            else:
                position[char] = -1
                while q and position[q[0]] == -1:
                    q.popleft()
        return ' ' if not q else q[0]
执行用时：100 ms, 在所有 Python 提交中击败了48.73%的用户
内存消耗：13.3 MB, 在所有 Python 提交中击败了81.21%的用户
通过测试用例：104 / 104
```

#### [剑指 Offer 32 - I. 从上到下打印二叉树](https://leetcode-cn.com/problems/cong-shang-dao-xia-da-yin-er-cha-shu-lcof/)

广度优先搜索

```Python
# Definition for a binary tree node.
# class TreeNode(object):
#     def __init__(self, x):
#         self.val = x
#         self.left = None
#         self.right = None

class Solution(object):
    def levelOrder(self, root):
        """
        :type root: TreeNode
        :rtype: List[int]
        """
        def bfs(tree):
            queue = collections.deque()
            queue.append(tree)
            l = []
            l.append(tree.val)
            while queue:
                temp = queue.popleft()
                if temp.left:
                    l.append(temp.left.val)
                    queue.append(temp.left)
                if temp.right:
                    l.append(temp.right.val)
                    queue.append(temp.right)
            return l
        if root:
            return bfs(root)
        else:
            return []
执行用时：16 ms, 在所有 Python 提交中击败了88.89%的用户
内存消耗：13.6 MB, 在所有 Python 提交中击败了5.10%的用户
通过测试用例：34 / 34
```

另一种写法

```Python
# Definition for a binary tree node.
# class TreeNode(object):
#     def __init__(self, x):
#         self.val = x
#         self.left = None
#         self.right = None

class Solution(object):
    def levelOrder(self, root):
        """
        :type root: TreeNode
        :rtype: List[int]
        """
        def bfs(tree):
            queue = collections.deque()
            queue.append(tree)
            l = []
            while queue:
                temp = queue.popleft()
                l.append(temp.val)
                if temp.left:
                    queue.append(temp.left)
                if temp.right:
                    queue.append(temp.right)
            return l
        if root:
            return bfs(root)
        else:
            return []
执行用时：20 ms, 在所有 Python 提交中击败了60.26%的用户
内存消耗：13.4 MB, 在所有 Python 提交中击败了31.63%的用户
通过测试用例：34 / 34
# 慢了一点，是因为前面的写法一次就读取左右两个子节点数值的原因吗？
```

#### [剑指 Offer 32 - II. 从上到下打印二叉树 II](https://leetcode-cn.com/problems/cong-shang-dao-xia-da-yin-er-cha-shu-ii-lcof/)

维持两个队列，一个是当前层的节点，一个保留当前层的所有子节点，当遍历完当前层节点时，就将子节点队列赋予当前层节点队列，并清空子节点队列。

```python
# Definition for a binary tree node.
# class TreeNode(object):
#     def __init__(self, x):
#         self.val = x
#         self.left = None
#         self.right = None

class Solution(object):
    def levelOrder(self, root):
        """
        :type root: TreeNode
        :rtype: List[List[int]]
        """
        if not root:
            return []
        queue_1 = collections.deque()
        queue_2 = collections.deque()
        queue_1.append(root)
        l = []
        n = []
        while queue_1:
            temp = queue_1.popleft()
            n.append(temp.val)
            if temp.left:
                queue_2.append(temp.left)
            if temp.right:
                queue_2.append(temp.right)
            if not queue_1:
                l.append(n)
                n = []
                if not queue_2:
                    break
                else:
                    queue_1 = queue_2
                    queue_2 = collections.deque()
        return l
执行用时：16 ms, 在所有 Python 提交中击败了90.06%的用户
内存消耗：13.3 MB, 在所有 Python 提交中击败了63.61%的用户
通过测试用例：34 / 34
```

一个队列

```python
# Definition for a binary tree node.
# class TreeNode(object):
#     def __init__(self, x):
#         self.val = x
#         self.left = None
#         self.right = None

class Solution(object):
    def levelOrder(self, root):
        """
        :type root: TreeNode
        :rtype: List[List[int]]
        """
        if not root:
            return []
        queue = collections.deque()
        queue.append(root)
        l = []
        while queue:
            temp = []
            for _ in range(len(queue)):
                node = queue.popleft()
                temp.append(node.val)
                if node.left:
                    queue.append(node.left)
                if node.right:
                    queue.append(node.right)
            l.append(temp)
        return l
执行用时：20 ms, 在所有 Python 提交中击败了65.60%的用户
内存消耗：13.4 MB, 在所有 Python 提交中击败了47.71%的用户
通过测试用例：34 / 34
```

#### [剑指 Offer 32 - III. 从上到下打印二叉树 III](https://leetcode-cn.com/problems/cong-shang-dao-xia-da-yin-er-cha-shu-iii-lcof/)

类似上一题，只是多加一个指示符号，判断需不需要将当前层的输出反转。

```Python
# Definition for a binary tree node.
# class TreeNode(object):
#     def __init__(self, x):
#         self.val = x
#         self.left = None
#         self.right = None

class Solution(object):
    def levelOrder(self, root):
        """
        :type root: TreeNode
        :rtype: List[List[int]]
        """
        if not root:
            return []
        queue = collections.deque()
        queue.append(root)
        f = True
        l = []
        while queue:
            temp = []
            for _ in range(len(queue)):
                node = queue.popleft()
                temp.append(node.val)
                if node.left:
                    queue.append(node.left)
                if node.right:
                    queue.append(node.right)
            if f:
                l.append(temp)
                f = not f
            else:
                l.append(temp[::-1])
                f = not f
        return l
执行用时：20 ms, 在所有 Python 提交中击败了62.59%的用户
内存消耗：13.5 MB, 在所有 Python 提交中击败了38.60%的用户
通过测试用例：34 / 34
```

为什么两个队列内存消耗更少？

```python
# Definition for a binary tree node.
# class TreeNode(object):
#     def __init__(self, x):
#         self.val = x
#         self.left = None
#         self.right = None

class Solution(object):
    def levelOrder(self, root):
        """
        :type root: TreeNode
        :rtype: List[List[int]]
        """
        if not root:
            return []
        queue_1 = collections.deque()
        queue_2 = collections.deque()
        queue_1.append(root)
        l = []
        n = []
        f = True
        while queue_1:
            temp = queue_1.popleft()
            n.append(temp.val)
            if temp.left:
                queue_2.append(temp.left)
            if temp.right:
                queue_2.append(temp.right)
            if not queue_1:
                if f:
                    l.append(n)
                    f = not f
                else:
                    l.append(n[::-1])
                    f = not f
                n = []
                if not queue_2:
                    break
                else:
                    queue_1 = queue_2
                    queue_2 = collections.deque()
        return l
执行用时：20 ms, 在所有 Python 提交中击败了62.59%的用户
内存消耗：13.3 MB, 在所有 Python 提交中击败了72.28%的用户
通过测试用例：34 / 34
```

#### [剑指 Offer 26. 树的子结构](https://leetcode-cn.com/problems/shu-de-zi-jie-gou-lcof/)

链接：https://leetcode-cn.com/problems/shu-de-zi-jie-gou-lcof/solution/mian-shi-ti-26-shu-de-zi-jie-gou-xian-xu-bian-li-p/

解题思路：
若树B是树A 的子结构，则子结构的根节点可能为树 AA 的任意一个节点。因此，判断树 B 是否是树 A 的子结构，需完成以下两步工作：

1. 先序遍历树 A 中的每个节点 n_A；（对应函数 isSubStructure(A, B)）
2. 判断树 A 中 以 n_A为根节点的子树 是否包含树 B 。（对应函数 recur(A, B)）

![Picture1.png](https://pic.leetcode-cn.com/27d9f65b79ae4982fb58835d468c2a23ec2ac399ba5f38138f49538537264d03-Picture1.png)

算法流程：
名词规定：树 A 的根节点记作 节点 A ，树 B 的根节点称为 节点 B 。

recur(A, B) 函数：

终止条件：

1. 当节点 B 为空：说明树 B 已匹配完成（越过叶子节点），因此返回 true；
2. 当节点 A 为空：说明已经越过树 A 叶子节点，即匹配失败，返回 false ；
3. 当节点 A 和 B 的值不同：说明匹配失败，返回 false ；

返回值：

1. 判断 A 和 B 的左子节点是否相等，即 recur(A.left, B.left) ；
2. 判断 A 和 B 的右子节点是否相等，即 recur(A.right, B.right) ；

isSubStructure(A, B) 函数：

特例处理： 当 树 A 为空 或 树 B 为空 时，直接返回 false ；
返回值： 若树 B 是树 A 的子结构，则必满足以下三种情况之一，因此用或 || 连接；
以 节点 A 为根节点的子树 包含树 B ，对应 recur(A, B)；
树 B 是 树 A 左子树 的子结构，对应 isSubStructure(A.left, B)；
树 B 是 树 A 右子树 的子结构，对应 isSubStructure(A.right, B)；
以上 2. 3. 实质上是在对树 A 做 先序遍历 。

```Python
# Definition for a binary tree node.
# class TreeNode(object):
#     def __init__(self, x):
#         self.val = x
#         self.left = None
#         self.right = None

class Solution(object):
    def isSubStructure(self, A, B):
        """
        :type A: TreeNode
        :type B: TreeNode
        :rtype: bool
        """
        if not B or not A:
            return False
        def recur(A, B):
            if not B:
                return True
            if not A or A.val != B.val:
                return False
            return recur(A.left, B.left) and recur(A.right, B.right)
        return recur(A, B) or self.isSubStructure(A.left, B) or self.isSubStructure(A.right, B) 
执行用时：96 ms, 在所有 Python 提交中击败了44.02%的用户
内存消耗：22.2 MB, 在所有 Python 提交中击败了89.69%的用户
通过测试用例：48 / 48
```

链接：https://leetcode-cn.com/problems/shu-de-zi-jie-gou-lcof/solution/shuang-di-gui-chao-917yi-ci-tong-guo-by-vkxuh/

双递归，一个用以寻找当前A树中是否存在节点与B树头结点相同，遍历整个A树。另一个则在找到匹配的头结点后判断是不是整个B树都在A的某一子树里面。

```python
# Definition for a binary tree node.
# class TreeNode(object):
#     def __init__(self, x):
#         self.val = x
#         self.left = None
#         self.right = None

class Solution(object):
    def isSubStructure(self, A, B):
        """
        :type A: TreeNode
        :type B: TreeNode
        :rtype: bool
        """
        if not B or not A:
            return False
        self.result = False
        def search_node(node):
            if not node:
                return False
            if node.val == B.val:
                self.result = check(node, B)
            if self.result:
                return True
            search_node(node.left)
            search_node(node.right)
        def check(n1, n2):
            if not n2:
                return True
            if not n1:
                return False
            if n1.val == n2.val:
                return check(n1.left, n2.left) and check(n1.right, n2.right)
            return False
        search_node(A)
        return self.result
执行用时：72 ms, 在所有 Python 提交中击败了92.24%的用户
内存消耗：22.3 MB, 在所有 Python 提交中击败了63.87%的用户
通过测试用例：48 / 48
```

#### [剑指 Offer 27. 二叉树的镜像](https://leetcode-cn.com/problems/er-cha-shu-de-jing-xiang-lcof/)

递归，将树中的每一个节点都交换左右节点，按层次遍历。

```Python
# Definition for a binary tree node.
# class TreeNode(object):
#     def __init__(self, x):
#         self.val = x
#         self.left = None
#         self.right = None

class Solution(object):
    def mirrorTree(self, root):
        """
        :type root: TreeNode
        :rtype: TreeNode
        """
        if not root:
            return root
        def mirror_node(node1, node2):
            node2.val = node1.val
            if node1.left:
                node2.right = TreeNode(None)
                mirror_node(node1.left, node2.right)
            if node1.right:
                node2.left = TreeNode(None)
                mirror_node(node1.right, node2.left)
        out = TreeNode(None)
        mirror_node(root, out)
        return out
执行用时：16 ms, 在所有 Python 提交中击败了67.54%的用户
内存消耗：13.2 MB, 在所有 Python 提交中击败了14.03%的用户
通过测试用例：68 / 68
```

链接：https://leetcode-cn.com/problems/er-cha-shu-de-jing-xiang-lcof/solution/er-cha-shu-de-jing-xiang-by-leetcode-sol-z44i/

方法一：递归
思路与算法

这是一道很经典的二叉树问题。显然，我们从根节点开始，递归地对树进行遍历，并从叶子节点先开始翻转得到镜像。如果当前遍历到的节点 root 的左右两棵子树都已经翻转得到镜像，那么我们只需要交换两棵子树的位置，即可得到以root 为根节点的整棵子树的镜像。

```python
# Definition for a binary tree node.
# class TreeNode(object):
#     def __init__(self, x):
#         self.val = x
#         self.left = None
#         self.right = None

class Solution(object):
    def mirrorTree(self, root):
        """
        :type root: TreeNode
        :rtype: TreeNode
        """
        if not root:
            return root
        left = self.mirrorTree(root.left)
        right = self.mirrorTree(root.right)
        root.left, root.right = right, left
        return root
执行用时：12 ms, 在所有 Python 提交中击败了92.15%的用户
内存消耗：13 MB, 在所有 Python 提交中击败了61.00%的用户
通过测试用例：68 / 68
```

#### [剑指 Offer 28. 对称的二叉树](https://leetcode-cn.com/problems/dui-cheng-de-er-cha-shu-lcof/)

链接：https://leetcode-cn.com/problems/dui-cheng-de-er-cha-shu-lcof/solution/mian-shi-ti-28-dui-cheng-de-er-cha-shu-di-gui-qing/

解题思路：
对称二叉树定义： 对于树中 任意两个对称节点 L 和 R ，一定有：
L.val = R.val：即此两对称节点值相等。
L.left.val = R.right.val ：即 L 的 左子节点 和 R 的 右子节点 对称；
L.right.val = R.left.val：即 L 的 右子节点 和 R 的 左子节点 对称。
根据以上规律，考虑从顶至底递归，判断每对节点是否对称，从而判断树是否为对称二叉树。

![Picture1.png](https://pic.leetcode-cn.com/ebf894b723530a89cc9a1fe099f36c57c584d4987b080f625b33e228c0a02bec-Picture1.png)

算法流程：
isSymmetric(root) ：

特例处理： 若根节点 root 为空，则直接返回 true。
返回值： 即 recur(root.left, root.right) ;

recur(L, R) ：

终止条件：
当 L 和 R 同时越过叶节点： 此树从顶至底的节点都对称，因此返回 true ；
当 L 或 R 中只有一个越过叶节点： 此树不对称，因此返回 false ；
当节点 L 值 != 节点 R 值： 此树不对称，因此返回 false ；
递推工作：
判断两节点 L.left和 R.right是否对称，即 recur(L.left, R.right) ；
判断两节点 L.right 和 R.left是否对称，即 recur(L.right, R.left) ；
返回值： 两对节点都对称时，才是对称树，因此用与逻辑符 && 连接。

时间复杂度 O(N)： 其中 N 为二叉树的节点数量，每次执行 recur() 可以判断一对节点是否对称，因此最多调用 N/2次 recur() 方法。

![Picture12.png](https://pic.leetcode-cn.com/88916808515487aac3ca24f9c55cbbdf6514f012eea04ec46cc2cc26acf9c4eb-Picture12.png)

```
# Definition for a binary tree node.
# class TreeNode(object):
#     def __init__(self, x):
#         self.val = x
#         self.left = None
#         self.right = None

class Solution(object):
    def isSymmetric(self, root):
        """
        :type root: TreeNode
        :rtype: bool
        """
        def recur(L, R):
            if not L and not R:
                return True
            if not L or not R or L.val != R.val:
                return False
            return recur(L.left, R.right) and recur(L.right, R.left)
        return recur(root.left, root.right) if root else True
```

队列实现

链接：https://leetcode-cn.com/problems/dui-cheng-de-er-cha-shu-lcof/solution/dong-hua-yan-shi-duo-chong-shi-xian-mian-shi-ti-28/回想下递归的实现：
当两个子树的根节点相等时，就比较:
左子树的left 和 右子树的right，这个比较是用递归实现的。
现在我们改用队列来实现，思路如下：
首先从队列中拿出两个节点(left和right)比较
将left的left节点和right的right节点放入队列
将left的right节点和right的left节点放入队列
时间复杂度是O(n)，空间复杂度是O(n)
动画演示如下：
![1.gif](https://pic.leetcode-cn.com/45a663b08efaa14193d63ef63ae3d1d130807467d13707f584906ad3af4adc36-1.gif)

```python
# Definition for a binary tree node.
# class TreeNode(object):
#     def __init__(self, x):
#         self.val = x
#         self.left = None
#         self.right = None

class Solution(object):
    def isSymmetric(self, root):
        """
        :type root: TreeNode
        :rtype: bool
        """
        if not root or not (root.left or root.right):
            return True
        queue = [root.left, root.right]
        while queue:
            left = queue.pop(0)
            right = queue.pop(0)
            if not (left or right):
                continue
            if not (left and right):
                return False
            if left.val != right.val:
                return False
            queue.append(left.left)
            queue.append(right.right)
            queue.append(left.right)
            queue.append(right.left)
        return True
执行用时：24 ms, 在所有 Python 提交中击败了32.03%的用户
内存消耗：13.2 MB, 在所有 Python 提交中击败了66.43%的用户
通过测试用例：195 / 195
```

#### [剑指 Offer 10- I. 斐波那契数列](https://leetcode-cn.com/problems/fei-bo-na-qi-shu-lie-lcof/)

注意取模这个坑！

```Python
class Solution(object):
    def fib(self, n):
        """
        :type n: int
        :rtype: int
        """
        F0 = 0
        F1 = 1
        Fn = 0
        if n == 0:
            return F0
        if n == 1:
            return F1
        for i in range(n-1):
            Fn = int((F0 + F1) % (1e9 + 7)) 
            F0 = F1
            F1 = Fn
        Fn = int(Fn % (1e9 + 7))
        return Fn
执行用时：16 ms, 在所有 Python 提交中击败了64.61%的用户
内存消耗：13.1 MB, 在所有 Python 提交中击败了47.15%的用户
通过测试用例：51 / 51
```

方法二：矩阵快速幂

链接：https://leetcode-cn.com/problems/fei-bo-na-qi-shu-lie-lcof/solution/fei-bo-na-qi-shu-lie-by-leetcode-solutio-hbss/

![image-20211110104719686](E:\MyGit\LeetCode\剑指offer.assets\image-20211110104719686.png)

```Python
class Solution(object):
    def fib(self, n):
        """
        :type n: int
        :rtype: int
        """
        mod = 10 ** 9 + 7
        if n < 2:
            return n
        def mutiply(a, b):
            c = [[0, 0], [0, 0]]
            for i in range(2):
                for j in range(2):
                    c[i][j] = (a[i][0] * b[0][j] + a[i][1] * b[1][j]) % mod
            return c
        def matrix_pow(a, n):
            ret = [[1, 0], [0, 1]] # 单位矩阵
            while n > 0:
                if n & 1:
                    ret = mutiply(ret, a)
                n >>= 1
                a = mutiply(a, a)
            return ret
        res = matrix_pow([[1, 1], [1, 0]], n-1)
        return res[0][0] #最后的结果是乘以[F(1), F(0)]即[1, 0]，F(n)取值就是res[0][0]*1 + res[0][1]*0 = res[0][0]
执行用时：8 ms, 在所有 Python 提交中击败了99.13%的用户
内存消耗：13 MB, 在所有 Python 提交中击败了70.49%的用户
通过测试用例：51 / 51
```

#### [剑指 Offer 10- II. 青蛙跳台阶问题](https://leetcode-cn.com/problems/qing-wa-tiao-tai-jie-wen-ti-lcof/)

链接：https://leetcode-cn.com/problems/qing-wa-tiao-tai-jie-wen-ti-lcof/solution/mian-shi-ti-10-ii-qing-wa-tiao-tai-jie-wen-ti-dong/

![image-20211110111430551](E:\MyGit\LeetCode\剑指offer.assets\image-20211110111430551.png)

```Python
class Solution(object):
    def numWays(self, n):
        """
        :type n: int
        :rtype: int
        """
        mod = 10 ** 9 + 7
        a, b = 1, 1
        for i in range(n):
            a, b = b, a + b
        return a % mod
执行用时：16 ms, 在所有 Python 提交中击败了66.79%的用户
内存消耗：12.8 MB, 在所有 Python 提交中击败了99.81%的用户
通过测试用例：51 / 51
```

类似上题，优化速度。

```python 
class Solution(object):
    def numWays(self, n):
        """
        :type n: int
        :rtype: int
        """
        mod = 10 ** 9 + 7
        if n < 2:
            return 1
        def mutiplay(a, b):
            c = [[0, 0], [0, 0]]
            for i in range(2):
                for j in range(2):
                    c[i][j] = (a[i][0]*b[0][j] + a[i][1]*b[1][j])
            return c
        def matrix_pow(a, n):
            ret = [[1, 0], [0, 1]]
            while n > 0:
                if n & 1:
                    ret = mutiplay(ret, a)
                n >>= 1
                a = mutiplay(a, a)
            return ret
        res = matrix_pow([[1, 1], [1, 0]], n-1)
        return (res[0][0] + res[0][1]) % mod # 此时的初始条件是[1, 1]
执行用时：12 ms, 在所有 Python 提交中击败了91.72%的用户
内存消耗：13 MB, 在所有 Python 提交中击败了69.80%的用户
通过测试用例：51 / 51
```

#### [剑指 Offer 63. 股票的最大利润](https://leetcode-cn.com/problems/gu-piao-de-zui-da-li-run-lcof/)

链接：https://leetcode-cn.com/problems/gu-piao-de-zui-da-li-run-lcof/solution/mian-shi-ti-63-gu-piao-de-zui-da-li-run-dong-tai-2/

```python
class Solution(object):
    def maxProfit(self, prices):
        """
        :type prices: List[int]
        :rtype: int
        """
        cost, profit = float("+inf"), 0
        for price in prices:
            cost = min(cost, price)
            profit = max(profit, price-cost)
        return profit
执行用时：20 ms, 在所有 Python 提交中击败了78.92%的用户
内存消耗：13.5 MB, 在所有 Python 提交中击败了85.65%的用户
通过测试用例：200 / 200
```

链接：https://leetcode-cn.com/problems/gu-piao-de-zui-da-li-run-lcof/solution/li-san-shi-yi-si-chong-fang-fa-qing-song-gy57/

具体来说，我们先找到第一个能产生利润的组合（后一天比前一天大）。然后，如果往后的天数股票价格还是增大，则把第一个最小值（开始增大的前一天）不断往后移动并计算利润。如果遇到比当前最小值还小的，则即使后面有更大的值，计算利润也是和新的最小值计算，所以从新开始选择产生利润的组合。

```python
class Solution(object):
    def maxProfit(self, prices):
        """
        :type prices: List[int]
        :rtype: int
        """
        profit = 0
        for i in range(1, len(prices)):
            if prices[i] > prices[i-1]:
                profit = max(profit, prices[i]-prices[i-1])
                prices[i] = prices[i-1]
        return profit
执行用时：16 ms, 在所有 Python 提交中击败了94.17%的用户
内存消耗：14.2 MB, 在所有 Python 提交中击败了20.03%的用户
通过测试用例：200 / 200
```

#### [剑指 Offer 42. 连续子数组的最大和](https://leetcode-cn.com/problems/lian-xu-zi-shu-zu-de-zui-da-he-lcof/)

动态规划，假设前i-1个数字最大连续子序列的加和是小于0的，那么低i个数字的最大子序列直接取当前数字。可以在原数组上存储最大子序列的值以节省空间。

```Python
class Solution(object):
    def maxSubArray(self, nums):
        """
        :type nums: List[int]
        :rtype: int
        """
        if len(nums) == 1:
            return nums[0]
        for i in range(1, len(nums)):
            nums[i] += max(nums[i-1], 0)
        return max(nums)
执行用时：32 ms, 在所有 Python 提交中击败了92.91%的用户
内存消耗：19.2 MB, 在所有 Python 提交中击败了42.62%的用户
通过测试用例：202 / 202
```

#### [剑指 Offer 47. 礼物的最大价值](https://leetcode-cn.com/problems/li-wu-de-zui-da-jie-zhi-lcof/)

链接：https://leetcode-cn.com/problems/li-wu-de-zui-da-jie-zhi-lcof/solution/mian-shi-ti-47-li-wu-de-zui-da-jie-zhi-dong-tai-gu/

![image-20211111164136325](E:\MyGit\LeetCode\剑指offer.assets\image-20211111164136325.png)

```python
class Solution(object):
    def maxValue(self, grid):
        """
        :type grid: List[List[int]]
        :rtype: int
        """
        row, column = len(grid), len(grid[0])
        for i in range(row):
            for j in range(column):
                if i == 0 and j == 0:
                    continue
                elif i == 0:
                    grid[i][j] += grid[i][j-1]
                elif j == 0:
                    grid[i][j] += grid[i-1][j]
                else:
                    grid[i][j] += max(grid[i-1][j], grid[i][j-1])
        return grid[-1][-1]
执行用时：24 ms, 在所有 Python 提交中击败了83.11%的用户内存消耗：
14.3 MB, 在所有 Python 提交中击败了73.25%的用户
通过测试用例：61 / 61
```

以上代码逻辑清晰，和转移方程直接对应，但仍可提升效率：当 gridgrid 矩阵很大时， i = 0i=0 或 j = 0j=0 的情况仅占极少数，相当循环每轮都冗余了一次判断。因此，可先初始化矩阵第一行和第一列，再开始遍历递推。

```python
class Solution(object):
    def maxValue(self, grid):
        """
        :type grid: List[List[int]]
        :rtype: int
        """
        row, column = len(grid), len(grid[0])
        for i in range(1, row):
            grid[i][0] += grid[i-1][0]
        for j in range(1, column):
            grid[0][j] += grid[0][j-1]
        for i in range(1, row):
            for j in range(1, column):
                grid[i][j] += max(grid[i-1][j], grid[i][j-1])
        return grid[-1][-1]
执行用时：16 ms, 在所有 Python 提交中击败了99.50%的用户
内存消耗：14.6 MB, 在所有 Python 提交中击败了31.44%的用户
通过测试用例：61 / 61
```

