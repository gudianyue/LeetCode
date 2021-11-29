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

#### [剑指 Offer 46. 把数字翻译成字符串](https://leetcode-cn.com/problems/ba-shu-zi-fan-yi-cheng-zi-fu-chuan-lcof/)

链接：https://leetcode-cn.com/problems/ba-shu-zi-fan-yi-cheng-zi-fu-chuan-lcof/solution/mian-shi-ti-46-ba-shu-zi-fan-yi-cheng-zi-fu-chua-6/

![Picture1.png](https://pic.leetcode-cn.com/e231fde16304948251633cfc65d04396f117239ea2d13896b1d2678de9067b42-Picture1.png)

```python
class Solution(object):
    def translateNum(self, num):
        """
        :type num: int
        :rtype: int
        """
        s = str(num)
        a = b = 1
        for i in range(2, len(s)+1):
            a, b = (a+b if "10" <= s[i-2:i] <= "25" else a), a
        return a
执行用时：24 ms, 在所有 Python 提交中击败了27.83%的用户
内存消耗：13.1 MB, 在所有 Python 提交中击败了43.53%的用户
通过测试用例：49 / 49
```

取余

```python
class Solution(object):
    def translateNum(self, num):
        """
        :type num: int
        :rtype: int
        """
        a = b = 1
        y = num % 10
        while num != 0:
            num //= 10
            x = num % 10
            a, b = (a+b if 10 <= 10*x+y <= 25 else a), a
            y = x
        return a
执行用时：12 ms, 在所有 Python 提交中击败了88.83%的用户
内存消耗：13.2 MB, 在所有 Python 提交中击败了5.02%的用户
通过测试用例：49 / 49
```

优化，直接取余100，再考虑能不能组合

```python
class Solution(object):
    def translateNum(self, num):
        """
        :type num: int
        :rtype: int
        """
        a = b = 1
        while num > 9:
            x = num % 100
            a, b = (a+b if 10 <= x <= 25 else a), a
            num //= 10
        return a
执行用时：12 ms, 在所有 Python 提交中击败了88.83%的用户
内存消耗：13 MB, 在所有 Python 提交中击败了63.11%的用户
通过测试用例：49 / 49
```

#### [剑指 Offer 48. 最长不含重复字符的子字符串](https://leetcode-cn.com/problems/zui-chang-bu-han-zhong-fu-zi-fu-de-zi-zi-fu-chuan-lcof/)

链接：https://leetcode-cn.com/problems/zui-chang-bu-han-zhong-fu-zi-fu-de-zi-zi-fu-chuan-lcof/solution/mian-shi-ti-48-zui-chang-bu-han-zhong-fu-zi-fu-d-9/

![Picture1.png](https://pic.leetcode-cn.com/c576757494724070d0c40cd192352ef9f48c42e14af09a1333972b9d843624a3-Picture1.png)

```python
class Solution(object):
    def lengthOfLongestSubstring(self, s):
        """
        :type s: str
        :rtype: int
        """
        hash_map = {}
        res = temp = 0
        for j in range(len(s)):
            i = hash_map.get(s[j], -1)
            hash_map[s[j]] = j
            temp = temp + 1 if temp < j-i else j-i
            res = max(res, temp)
        return res
执行用时：36 ms, 在所有 Python 提交中击败了93.12%的用户
内存消耗：13.8 MB, 在所有 Python 提交中击败了58.07%的用户
通过测试用例：987 / 987
```

双指针

```python
class Solution(object):
    def lengthOfLongestSubstring(self, s):
        """
        :type s: str
        :rtype: int
        """
        hash_map, res, i = {}, 0, -1
        for j in range(len(s)):
            if s[j] in hash_map:
                i = max(hash_map[s[j]], i)
            hash_map[s[j]] = j
            res = max(res, j-i)
        return res
执行用时：36 ms, 在所有 Python 提交中击败了93.12%的用户
内存消耗：14 MB, 在所有 Python 提交中击败了40.47%的用户
通过测试用例：987 / 987
```

#### [剑指 Offer 18. 删除链表的节点](https://leetcode-cn.com/problems/shan-chu-lian-biao-de-jie-dian-lcof/)

新建一个指针，利用指针滑动寻找节点，首先先判断头结点是不是。

```python
# Definition for singly-linked list.
# class ListNode(object):
#     def __init__(self, x):
#         self.val = x
#         self.next = None
class Solution(object):
    def deleteNode(self, head, val):
        """
        :type head: ListNode
        :type val: int
        :rtype: ListNode
        """
        p = head
        if p.val == val:
            head = head.next
            return head
        while p.next:
            if p.next.val == val:
                p.next = p.next.next
                break
            else:
                p = p.next 
        return head
执行用时：16 ms, 在所有 Python 提交中击败了97.68%的用户
内存消耗：13.2 MB, 在所有 Python 提交中击败了96.86%的用户
通过测试用例：37 / 37
```

#### [剑指 Offer 22. 链表中倒数第k个节点](https://leetcode-cn.com/problems/lian-biao-zhong-dao-shu-di-kge-jie-dian-lcof/)

利用哈希表存储节点-节点位置键值对，并记录链表长度，最后将链表总长度+1-k即是输出头结点在原链表中的位置，在哈希表中找到记录输出即可。

```Python
# Definition for singly-linked list.
# class ListNode(object):
#     def __init__(self, x):
#         self.val = x
#         self.next = None

class Solution(object):
    def getKthFromEnd(self, head, k):
        """
        :type head: ListNode
        :type k: int
        :rtype: ListNode
        """
        total = 0
        hash_map = {}
        while head:
            total += 1
            hash_map[total] = head
            head = head.next
        node = total + 1 - k 
        return hash_map[node]
执行用时：16 ms, 在所有 Python 提交中击败了87.36%的用户
内存消耗：13.1 MB, 在所有 Python 提交中击败了48.58%的用户
通过测试用例：208 / 208
```

快慢指针，先让快指针走k个节点，这样快慢指针之间就相隔k个节点了。快慢指针一起遍历链表时，当快指针刚好遍历完整个链表，慢指针刚好位于倒数第k个节点位置。

```python
# Definition for singly-linked list.
# class ListNode(object):
#     def __init__(self, x):
#         self.val = x
#         self.next = None

class Solution(object):
    def getKthFromEnd(self, head, k):
        """
        :type head: ListNode
        :type k: int
        :rtype: ListNode
        """
        fast, slow = head, head
        while fast and k > 0:
            fast, k = fast.next, k-1
        while fast:
            fast, slow = fast.next, slow.next
        return slow
执行用时：16 ms, 在所有 Python 提交中击败了87.36%的用户
内存消耗：12.9 MB, 在所有 Python 提交中击败了92.08%的用户
通过测试用例：208 / 208
```

#### [剑指 Offer 25. 合并两个排序的链表](https://leetcode-cn.com/problems/he-bing-liang-ge-pai-xu-de-lian-biao-lcof/)

新建两个指针，一个用于连接输出链表头结点（连接l1、l2之间第一个最小的节点），一个用于遍历l1、l2链表时不断向后遍历输出链表。

```python
执行用时：
32 ms
, 在所有 Python 提交中击败了
90.58%
的用户
内存消耗：
14.3 MB
, 在所有 Python 提交中击败了
71.75%
的用户
通过测试用例：
218 / 218# Definition for singly-linked list.
# class ListNode(object):
#     def __init__(self, x):
#         self.val = x
#         self.next = None

class Solution(object):
    def mergeTwoLists(self, l1, l2):
        """
        :type l1: ListNode
        :type l2: ListNode
        :rtype: ListNode
        """ 
        head = cur = ListNode(None)
        while l1 and l2:
            if l1.val <= l2.val:
                cur.next = l1
                l1 = l1.next
            else:
                cur.next = l2
                l2 = l2.next
            cur = cur.next
        if l1:
            cur.next = l1
        else:
            cur.next = l2
        return head.next
执行用时：32 ms, 在所有 Python 提交中击败了90.58%的用户
内存消耗：14.3 MB, 在所有 Python 提交中击败了71.75%的用户
通过测试用例：218 / 218
```

#### [剑指 Offer 52. 两个链表的第一个公共节点](https://leetcode-cn.com/problems/liang-ge-lian-biao-de-di-yi-ge-gong-gong-jie-dian-lcof/)

暴力破解：建立一个列表保存AB链表中遍历过的节点，每次AB都移动一个节点，当有节点重复出现了，就是第一个相交的节点。

```Python
# Definition for singly-linked list.
# class ListNode(object):
#     def __init__(self, x):
#         self.val = x
#         self.next = None

class Solution(object):
    def getIntersectionNode(self, headA, headB):
        """
        :type head1, head1: ListNode
        :rtype: ListNode
        """
        node_list = []
        while headA or headB:
            if headA:
                if headA in node_list:
                    return headA
                else:
                    node_list.append(headA)
                    headA = headA.next
            if headB:
                if headB in node_list:
                    return headB
                else:
                    node_list.append(headB)
                    headB = headB.next
        return None
执行用时：7132 ms, 在所有 Python 提交中击败了5.98%的用户
内存消耗：42.3 MB, 在所有 Python 提交中击败了42.52%的用户
通过测试用例：45 / 45
```

双指针：https://leetcode-cn.com/problems/liang-ge-lian-biao-de-di-yi-ge-gong-gong-jie-dian-lcof/solution/shuang-zhi-zhen-fa-lang-man-xiang-yu-by-ml-zimingm/

我们使用两个指针 node1，node2 分别指向两个链表 headA，headB 的头结点，然后同时分别逐结点遍历，当 node1 到达链表 headA 的末尾时，重新定位到链表 headB 的头结点；当 node2 到达链表 headB 的末尾时，重新定位到链表 headA 的头结点。

这样，当它们相遇时，所指向的结点就是第一个公共结点。

```Python
# Definition for singly-linked list.
# class ListNode(object):
#     def __init__(self, x):
#         self.val = x
#         self.next = None

class Solution(object):
    def getIntersectionNode(self, headA, headB):
        """
        :type head1, head1: ListNode
        :rtype: ListNode
        """
        node1, node2 = headA, headB
        while node1 != node2:
            node1 = node1.next if node1 else headB
            node2 = node2.next if node2 else headA
        return node2
执行用时：140 ms, 在所有 Python 提交中击败了90.20%的用户
内存消耗：42.1 MB, 在所有 Python 提交中击败了87.71%的用户
通过测试用例：45 / 45
```

#### [剑指 Offer 21. 调整数组顺序使奇数位于偶数前面](https://leetcode-cn.com/problems/diao-zheng-shu-zu-shun-xu-shi-qi-shu-wei-yu-ou-shu-qian-mian-lcof/)

解法：双指针，第一个指针遍历所有数组元素，第二个指针只有在第一个指针遍历到奇数时才和第一个指针交换数组元素并向前移动一位。停止条件为第一个指针遍历完所有数组元素。

```Python
class Solution(object):
    def exchange(self, nums):
        """
        :type nums: List[int]
        :rtype: List[int]
        """
        start, cur = 0, 0
        while cur < len(nums):
            if nums[cur] % 2 == 0:
                cur += 1
            else:
                nums[start], nums[cur] = nums[cur], nums[start]
                start += 1
                cur += 1
        return nums
执行用时：32 ms, 在所有 Python 提交中击败了66.46%的用户
内存消耗：18.4 MB, 在所有 Python 提交中击败了39.78%的用户
通过测试用例：17 / 17
```

解题思路：
考虑定义双指针 ii , jj 分列数组左右两端，循环执行：

指针 ii 从左向右寻找偶数；
指针 jj 从右向左寻找奇数；
将 偶数 nums[i]nums[i] 和 奇数 nums[j]nums[j] 交换。
可始终保证： 指针 ii 左边都是奇数，指针 jj 右边都是偶数 。
链接：https://leetcode-cn.com/problems/diao-zheng-shu-zu-shun-xu-shi-qi-shu-wei-yu-ou-shu-qian-mian-lcof/solution/mian-shi-ti-21-diao-zheng-shu-zu-shun-xu-shi-qi-4/

```Python
class Solution(object):
    def exchange(self, nums):
        """
        :type nums: List[int]
        :rtype: List[int]
        """
        i, j = 0, len(nums)-1
        while i < j:
            while i<j and nums[i]%2 == 1:
                i += 1
            while i<j and nums[j]%2 == 0:
                j -= 1
            nums[i], nums[j] = nums[j], nums[i]
        return nums
执行用时：32 ms, 在所有 Python 提交中击败了66.46%的用户
内存消耗：18.2 MB, 在所有 Python 提交中击败了81.03%的用户
通过测试用例：17 / 17
```

#### [剑指 Offer 57. 和为s的两个数字](https://leetcode-cn.com/problems/he-wei-sde-liang-ge-shu-zi-lcof/)

算法流程：
初始化： 双指针 i , jj分别指向数组 nums 的左右两端 （俗称对撞双指针）。
循环搜索： 当双指针相遇时跳出；
计算和 s = nums[i] + nums[j]；
若 s > target ，则指针j向左移动，即执行 j = j - 1 ；
若 s < target ，则指针i向右移动，即执行 i = i + 1；
若 s = target ，立即返回数组 \[nums[i], nums\[j]]；
返回空数组，代表无和为 target 的数字组合。
链接：https://leetcode-cn.com/problems/he-wei-sde-liang-ge-shu-zi-lcof/solution/mian-shi-ti-57-he-wei-s-de-liang-ge-shu-zi-shuang-/

```python
class Solution(object):
    def twoSum(self, nums, target):
        """
        :type nums: List[int]
        :type target: int
        :rtype: List[int]
        """
        start, end = 0, len(nums)-1
        while start < end:
            if nums[start] + nums[end] == target:
                return [nums[start], nums[end]]
            elif nums[start] + nums[end] < target:
                start += 1
            else:
                end -= 1
        return None
执行用时：88 ms, 在所有 Python 提交中击败了38.93%的用户
内存消耗：21.5 MB, 在所有 Python 提交中击败了74.15%的用户
通过测试用例：36 / 36
```

思路：二分法可加快范围的缩小（超过100%）
时间复杂度： O(lgN) ~ O(N)
空间复杂度： O(1)

算法流程：

1. 初始化： 双指针i,j分别指向数组 nums的两端, split_index = (j + i) >> 1；
2. 循环搜索： 当双指针相遇(i >= j - 1)或找到答案时跳出；
3. 比较nums[j] + nums[i]与target；
   1. c. 当 nums[j] + nums[i] > target时，此时可根据nums[i] + nums[split_index]的结果判断是否要加速缩进
      * 当nums[i] + nums[split_index] > target，可知num[i]加上范围(split_index, j)内的任何一个元素均大于target，此时可加速缩进，令j = split_index - 1，split_index = (j + i) >> 1
      * 当nums[i] + nums[split_index] < target，无法加速缩进，令j -= 1, 易知潜在的右边数字在范围(split_index, j)内，令split_index = (j + split_index) >> 1
      * 当nums[i] + nums[split_index] = target，立即返回数组 [nums[i], nums[split_index]]
   2. 当 nums[j] + nums[i] < target时，此时可根据nums[split_index] + nums[j]的结果判断是否要加速缩进
      * 当nums[split_index] + nums[j] < target，可知num[j]加上范围(i, split_index)内的任何一个元素均小于target，可加速缩进，令i = split_index + 1，split_index = (j + i) >> 1
      * 当nums[split_index] + nums[j] > target，无法加速缩进，令i += 1, 易知潜在的左边数字在范围(i, split_index)内，令split_index = (split_index + i) >> 1
      * 当nums[split_index] + nums[j] = target，立即返回数组 [nums[split_index], nums[j]]
   3. 当 nums[j] + nums[i] = target时，立即返回数组 [nums[i], nums[j]]
4. 循环中没找到正确答案时，且最终i = j - 1 和 nums[j] + nums[i] = target时，返回数组 [nums[i], nums[j]]，否则返回[]

链接：https://leetcode-cn.com/problems/he-wei-sde-liang-ge-shu-zi-lcof/solution/shuang-zhi-zhen-gai-jin-ban-er-fen-fa-by-harrislia/

```python
class Solution(object):
    def twoSum(self, nums, target):
        """
        :type nums: List[int]
        :type target: int
        :rtype: List[int]
        """
        start, end = 0, len(nums)-1
        mid = (start + end) // 2
        while start < end:
            if nums[start] + nums[end] == target:
                return [nums[start], nums[end]]
            elif nums[start] + nums[end] < target:
                if nums[end] + nums[mid] < target:
                    start = mid + 1
                    mid = (start + end) // 2
                else:
                    start += 1
            else:
                if nums[start] + nums[mid] > target:
                    end = mid - 1
                    mid = (start + end) // 2
                else:
                    end -= 1
        return []
执行用时：72 ms, 在所有 Python 提交中击败了90.31%的用户
内存消耗：21.5 MB, 在所有 Python 提交中击败了81.26%的用户
通过测试用例：36 / 36
```

#### [剑指 Offer 58 - I. 翻转单词顺序](https://leetcode-cn.com/problems/fan-zhuan-dan-ci-shun-xu-lcof/)

算法解析：
倒序遍历字符串s ，记录单词左右索引边界 i , j ；
每确定一个单词的边界，则将其添加至单词列表 res；
最终，将单词列表拼接为字符串，并返回即可。
复杂度分析：
时间复杂度 O(N ： 其中 N 为字符串 s 的长度，线性遍历字符串。
空间复杂度 O(N)： 新建的 list(Python) 或 StringBuilder(Java) 中的字符串总长度 ≤N ，占用 O(N)大小的额外空间。
链接：https://leetcode-cn.com/problems/fan-zhuan-dan-ci-shun-xu-lcof/solution/mian-shi-ti-58-i-fan-zhuan-dan-ci-shun-xu-shuang-z/

```python 
class Solution(object):
    def reverseWords(self, s):
        """
        :type s: str
        :rtype: str
        """
        s = s.strip() 
        i = j = len(s) - 1
        res = []
        while i >= 0:
            while i >= 0 and s[i] != ' ': i -= 1 
            res.append(s[i + 1: j + 1]) 
            while s[i] == ' ': i -= 1
            j = i
        return ' '.join(res) 
执行用时：12 ms, 在所有 Python 提交中击败了95.72%的用户
内存消耗：13.9 MB, 在所有 Python 提交中击败了89.79%的用户
通过测试用例：25 / 25
```

#### [剑指 Offer 12. 矩阵中的路径](https://leetcode-cn.com/problems/ju-zhen-zhong-de-lu-jing-lcof/)

解题思路：
本问题是典型的矩阵搜索问题，可使用 深度优先搜索（DFS）+ 剪枝 解决。

* 深度优先搜索： 可以理解为暴力法遍历矩阵中所有字符串可能性。DFS 通过递归，先朝一个方向搜到底，再回溯至上个节点，沿另一个方向搜索，以此类推。
* 剪枝： 在搜索中，遇到 这条路不可能和目标字符串匹配成功 的情况（例如：此矩阵元素和目标字符不同、此元素已被访问），则应立即返回，称之为 可行性剪枝 。

![Picture0.png](https://pic.leetcode-cn.com/1604944042-glmqJO-Picture0.png)

DFS 解析：

* 递归参数： 当前元素在矩阵 board 中的行列索引 i 和 j ，当前目标字符在 word 中的索引 k 。
* 终止条件：
  1. 返回 falsefalse ： (1) 行或列索引越界 或 (2) 当前矩阵元素与目标字符不同 或 (3) 当前矩阵元素已访问过 （ (3) 可合并至 (2) ） 。
  2. 返回 truetrue ： k = len(word) - 1 ，即字符串 word 已全部匹配。
* 递推工作：
  1. 标记当前矩阵元素： 将 board[i][j] 修改为 空字符 '' ，代表此元素已访问过，防止之后搜索时重复访问。
  2. 搜索下一单元格： 朝当前元素的 上、下、左、右 四个方向开启下层递归，使用 或 连接 （代表只需找到一条可行路径就直接返回，不再做后续 DFS ），并记录结果至 res 。
  3. 还原当前矩阵元素： 将 board[i][j] 元素还原至初始值，即 word[k] 。
* 返回值： 返回布尔量 res ，代表是否搜索到目标字符串。
* 使用空字符（Python: '' , Java/C++: '\0' ）做标记是为了防止标记字符与矩阵原有字符重复。当存在重复时，此算法会将矩阵原有字符认作标记字符，从而出现错误。

复杂度分析：
M, N分别为矩阵行列大小， K为字符串 word 长度。

时间复杂度 O($3^K$MN)： 最差情况下，需要遍历矩阵中长度为 K 字符串的所有方案，时间复杂度为 O($3^K$)；矩阵中共有 MN 个起点，时间复杂度为 O(MN)。
方案数计算： 设字符串长度为 K ，搜索中每个字符有上、下、左、右四个方向可以选择，舍弃回头（上个字符）的方向，剩下 3 种选择，因此方案数的复杂度为 O($3^K$)。
空间复杂度 O(K) ： 搜索过程中的递归深度不超过 K ，因此系统因函数调用累计使用的栈空间占用 O(K)（因为函数返回后，系统调用的栈空间会释放）。最坏情况下 K = MN，递归深度为 MN，此时系统栈使用 O(MN)的额外空间。

链接：https://leetcode-cn.com/problems/ju-zhen-zhong-de-lu-jing-lcof/solution/mian-shi-ti-12-ju-zhen-zhong-de-lu-jing-shen-du-yo/


```python 
class Solution(object):
    def exist(self, board, word):
        """
        :type board: List[List[str]]
        :type word: str
        :rtype: bool
        """
        def dfs(i, j, k):
            if not 0 <= i <len(board) or not 0 <= j < len(board[0]) or board[i][j] != word[k]:
                return False
            if k == len(word)-1:
                return True
            board[i][j] = ''
            res = dfs(i+1, j, k+1) or dfs(i-1, j, k+1) or dfs(i, j+1, k+1) or dfs(i, j-1, k+1)
            board[i][j] = word[k]
            return res
        for i in range(len(board)):
            for j in range(len(board[0])):
                if dfs(i, j, 0):
                    return True
        return False
执行用时：164 ms, 在所有 Python 提交中击败了41.76%的用户
内存消耗：16.4 MB, 在所有 Python 提交中击败了16.48%的用户
通过测试用例：87 / 87
```

思路
回溯其实就是纯暴力枚举，把所有情况列举下，如果列举到一半发现已经不符合要求了及时剪枝，并且把之前做出的选择撤销，比如本题如果列举的一条路径不符合要求，把之前访问过的位置全部改回原来的值。

步骤如下：

1. 首先，要在矩阵中找字符串中的第一个字符，找到后进入递归
2. 对于已访问的位置，修改其值为'/'，访问完毕后要将值改回来，这是回溯的核心
3. 查找当前字符的周围字符，如果周围字符没有被访问过且与字符串的下一个字符相等，再次进入递归
4. 当索引index已经等于字符串长度时，说明已经找到了一条路径，返回True
5. 只要找到一条路径就返回true：if dfs返回True： 返回True


链接：https://leetcode-cn.com/problems/ju-zhen-zhong-de-lu-jing-lcof/solution/jian-zhi-offer-12-ju-zhen-zhong-de-lu-ji-u3mw/

```python
class Solution(object):
    def exist(self, board, word):
        """
        :type board: List[List[str]]
        :type word: str
        :rtype: bool
        """
        row, col = len(board), len(board[0])
        w = len(word)
        def dfs(x, y, k):
            if k == w:
                return True
            for dx, dy in ((-1, 0), (1, 0), (0, -1), (0, 1)):
                nx, ny = x + dx, y + dy
                if 0 <= nx < row and 0 <= ny < col and board[nx][ny] == word[k]:
                    board[nx][ny] = ''
                    if dfs(nx, ny, k+1):
                        return True
                    board[nx][ny] = word[k]
            return False
        for i in range(row):
            for j in range(col):
                if board[i][j] == word[0]:
                    board[i][j] = ''
                    if dfs(i, j, 1):
                        return True
                    board[i][j] = word[0]
        return False
执行用时：120 ms, 在所有 Python 提交中击败了96.84%的用户
内存消耗：15.7 MB, 在所有 Python 提交中击败了73.62%的用户
通过测试用例：87 / 87
```

#### [剑指 Offer 13. 机器人的运动范围](https://leetcode-cn.com/problems/ji-qi-ren-de-yun-dong-fan-wei-lcof/)

方法一：广度优先搜索
思路和算法

我们将行坐标和列坐标数位之和大于 k 的格子看作障碍物，那么这道题就是一道很传统的搜索题目，我们可以使用广度优先搜索或者深度优先搜索来解决它，本文选择使用广度优先搜索的方法来讲解。

那么如何计算一个数的数位之和呢？我们只需要对数 x 每次对 10 取余，就能知道数 x 的个位数是多少，然后再将 x 除 10，这个操作等价于将 x 的十进制数向右移一位，删除个位数（类似于二进制中的 >> 右移运算符），不断重复直到 x 为 0 时结束。

同时这道题还有一个隐藏的优化：我们在搜索的过程中搜索方向可以缩减为向右和向下，而不必再向上和向左进行搜索。如下图，我们展示了 16 * 16 的地图随着限制条件 k 的放大，可行方格的变化趋势，每个格子里的值为行坐标和列坐标的数位之和，蓝色方格代表非障碍方格，即其值小于等于当前的限制条件 k。我们可以发现随着限制条件 k 的增大，(0, 0) 所在的蓝色方格区域内新加入的非障碍方格都可以由上方或左方的格子移动一步得到。而其他不连通的蓝色方格区域会随着 k 的增大而连通，且连通的时候也是由上方或左方的格子移动一步得到，因此我们可以将我们的搜索方向缩减为向右或向下。

![img](https://pic.leetcode-cn.com/858d08887e25e9f3503e28ad5b3d2e0fafdf8b69c0b534f8e222662acc8eb00e-%E5%B9%BB%E7%81%AF%E7%89%878.JPG)

复杂度分析

时间复杂度：O(mn)，其中 m 为方格的行数，n 为方格的列数。考虑所有格子都能进入，那么搜索的时候一个格子最多会被访问的次数为常数，所以时间复杂度为 O(2mn)=O(mn)。

空间复杂度：O(mn)，其中 m 为方格的行数，n 为方格的列数。搜索的时候需要一个大小为 O(mn) 的标记结构用来标记每个格子是否被走过。
链接：https://leetcode-cn.com/problems/ji-qi-ren-de-yun-dong-fan-wei-lcof/solution/ji-qi-ren-de-yun-dong-fan-wei-by-leetcode-solution/

```python
class Solution(object):
    def movingCount(self, m, n, k):
        """
        :type m: int
        :type n: int
        :type k: int
        :rtype: int
        """
        def digitsum(n):
            ans = 0
            while n:
                ans += n % 10
                n //= 10
            return ans
        q = collections.deque()
        q.append((0, 0))
        s = set()
        while q:
            x, y = q.popleft()
            if (x, y) not in s and 0 <= x < m and 0 <= y < n and digitsum(x) + digitsum(y) <= k:
                s.add((x, y))
                for nx, ny in ((x+1, y), (x, y+1)):
                    q.append((nx, ny))
        return len(s)
执行用时：28 ms, 在所有 Python 提交中击败了78.25%的用户
内存消耗：13.4 MB, 在所有 Python 提交中击败了77.56%的用户
通过测试用例：49 / 49
```

方法二：递推
思路

考虑到方法一提到搜索的方向只需要朝下或朝右，我们可以得出一种递推的求解方法。

算法

定义 vis\[i]\[j] 为 (i, j) 坐标是否可达，如果可达返回 1，否则返回 0。

首先 (i, j) 本身需要可以进入，因此需要先判断 i 和 j 的数位之和是否大于 k ，如果大于的话直接设置 vis\[i]\[j] 为不可达即可。

否则，前面提到搜索方向只需朝下或朝右，因此 (i, j) 的格子只会从 (i - 1, j) 或者 (i, j - 1) 两个格子走过来（不考虑边界条件），那么 vis\[i]\[j] 是否可达的状态则可由如下公式计算得到：

$$
\textit{vis}[i][j]=\textit{vis}[i-1][j]\ \textrm{or}\ \textit{vis}[i][j-1]
$$
即只要有一个格子可达，那么 (i, j) 这个格子就是可达的，因此我们只要遍历所有格子，递推计算出它们是否可达然后用变量 ans 记录可达的格子数量即可。

初始条件 vis\[i]\[j] = 1 ，递推计算的过程中注意边界的处理。
链接：https://leetcode-cn.com/problems/ji-qi-ren-de-yun-dong-fan-wei-lcof/solution/ji-qi-ren-de-yun-dong-fan-wei-by-leetcode-solution/

```python
class Solution(object):
    def movingCount(self, m, n, k):
        """
        :type m: int
        :type n: int
        :type k: int
        :rtype: int
        """
        def digitsum(n):
            ans = 0
            while n:
                ans += n % 10
                n //= 10
            return ans
        vis = set()
        vis.add((0, 0))
        for i in range(m):
            for j in range(n):
                if ((i-1, j) in vis or (i, j-1) in vis) and digitsum(i) + digitsum(j) <= k:
                    vis.add((i, j))
        return len(vis)
执行用时：24 ms, 在所有 Python 提交中击败了91.52%的用户
内存消耗：13.6 MB, 在所有 Python 提交中击败了49.52%的用户
通过测试用例：49 / 49
```

#### [剑指 Offer 34. 二叉树中和为某一值的路径](https://leetcode-cn.com/problems/er-cha-shu-zhong-he-wei-mou-yi-zhi-de-lu-jing-lcof/)

方法一：深度优先搜索
思路及算法

我们可以采用深度优先搜索的方式，枚举每一条从根节点到叶子节点的路径。当我们遍历到叶子节点，且此时路径和恰为目标和时，我们就找到了一条满足条件的路径。

复杂度分析

时间复杂度：O($N^2$)，其中 N 是树的节点数。在最坏情况下，树的上半部分为链状，下半部分为完全二叉树，并且从根节点到每一个叶子节点的路径都符合题目要求。此时，路径的数目为 O(N)，并且每一条路径的节点个数也为 O(N)，因此要将这些路径全部添加进答案中，时间复杂度为 O($N^2$)。

空间复杂度：O(N)，其中 N 是树的节点数。空间复杂度主要取决于栈空间的开销，栈中的元素个数不会超过树的节点数。

链接：https://leetcode-cn.com/problems/er-cha-shu-zhong-he-wei-mou-yi-zhi-de-lu-jing-lcof/solution/er-cha-shu-zhong-he-wei-mou-yi-zhi-de-lu-68dg/

```Python
# Definition for a binary tree node.
# class TreeNode(object):
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution(object):
    def pathSum(self, root, target):
        """
        :type root: TreeNode
        :type target: int
        :rtype: List[List[int]]
        """
        res = list()
        path = list()
        def dfs(root, target):
            if not root:
                return
            path.append(root.val)
            target -= root.val
            if not root.left and not root.right and target == 0:
                res.append(path[:])
            dfs(root.left, target)
            dfs(root.right, target)
            path.pop()
        dfs(root, target)
        return res
执行用时：32 ms, 在所有 Python 提交中击败了37.50%的用户
内存消耗：16 MB, 在所有 Python 提交中击败了21.27%的用户
通过测试用例：114 / 114
```

方法二：广度优先搜索
思路及算法

我们也可以采用广度优先搜索的方式，遍历这棵树。当我们遍历到叶子节点，且此时路径和恰为目标和时，我们就找到了一条满足条件的路径。

为了节省空间，我们使用哈希表记录树中的每一个节点的父节点。每次找到一个满足条件的节点，我们就从该节点出发不断向父节点迭代，即可还原出从根节点到当前节点的路径。

链接：https://leetcode-cn.com/problems/er-cha-shu-zhong-he-wei-mou-yi-zhi-de-lu-jing-lcof/solution/er-cha-shu-zhong-he-wei-mou-yi-zhi-de-lu-68dg/

```python
# Definition for a binary tree node.
# class TreeNode(object):
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution(object):
    def pathSum(self, root, target):
        """
        :type root: TreeNode
        :type target: int
        :rtype: List[List[int]]
        """
        res = list()
        parents = collections.defaultdict(lambda: None)#当不存在key时，会返回None。因为根节点是没有父节点的，所以要为其返回None。否则会报错

        def getpath(node):
            temp = list()
            while node:
                temp.append(node.val)
                node = parents[node]
            res.append(temp[::-1])
        
        if not root:
            return res
        
        que_node = collections.deque([root])
        que_total = collections.deque([0])
        while que_node:
            node = que_node.popleft()
            total = que_total.popleft() + node.val
            
            if not node.left and not node.right and total == target:
                getpath(node)
            else:
                if node.left:
                    parents[node.left] = node
                    que_node.append(node.left)
                    que_total.append(total)
                if node.right:
                    parents[node.right] = node
                    que_node.append(node.right)
                    que_total.append(total)
        return res
执行用时：16 ms, 在所有 Python 提交中击败了99.81%的用户
内存消耗：15.3 MB, 在所有 Python 提交中击败了96.83%的用户
通过测试用例：114 / 114
```

#### [剑指 Offer 36. 二叉搜索树与双向链表](https://leetcode-cn.com/problems/er-cha-sou-suo-shu-yu-shuang-xiang-lian-biao-lcof/)

解题思路：
本文解法基于性质：二叉搜索树的中序遍历为 递增序列 。
将 二叉搜索树 转换成一个 “排序的循环双向链表” ，其中包含三个要素：

排序链表： 节点应从小到大排序，因此应使用 中序遍历 “从小到大”访问树的节点。
双向链表： 在构建相邻节点的引用关系时，设前驱节点 pre 和当前节点 cur ，不仅应构建 pre.right = cur ，也应构建 cur.left = pre 。
循环链表： 设链表头节点 head 和尾节点 tail ，则应构建 head.left = tail 和 tail.right = head 。
![Picture1.png](https://pic.leetcode-cn.com/1599401091-PKIjds-Picture1.png)

根据以上分析，考虑使用中序遍历访问树的各节点 cur ；并在访问每个节点时构建 cur 和前驱节点 pre 的引用指向；中序遍历完成后，最后构建头节点和尾节点的引用指向即可。

算法流程：
dfs(cur): 递归法中序遍历；

1. 终止条件： 当节点 cur 为空，代表越过叶节点，直接返回；
2. 递归左子树，即 dfs(cur.left) ；
3. 构建链表：
   1. 当 pre 为空时： 代表正在访问链表头节点，记为 head ；
   2. 当 pre 不为空时： 修改双向节点引用，即 pre.right = cur ， cur.left = pre ；
   3. 保存 cur ： 更新 pre = cur ，即节点 cur 是后继节点的 pre ；
4. 递归右子树，即 dfs(cur.right) ；

treeToDoublyList(root)：

1. 特例处理： 若节点 root 为空，则直接返回；
2. 初始化： 空节点 pre ；
3. 转化为双向链表： 调用 dfs(root) ；
4. 构建循环链表： 中序遍历完成后，head 指向头节点， pre 指向尾节点，因此修改 head 和 pre 的双向节点引用即可；
5. 返回值： 返回链表的头节点 head 即可；


链接：https://leetcode-cn.com/problems/er-cha-sou-suo-shu-yu-shuang-xiang-lian-biao-lcof/solution/mian-shi-ti-36-er-cha-sou-suo-shu-yu-shuang-xian-5/

```python
"""
# Definition for a Node.
class Node(object):
    def __init__(self, val, left=None, right=None):
        self.val = val
        self.left = left
        self.right = right
"""
class Solution(object):
    def treeToDoublyList(self, root):
        """
        :type root: Node
        :rtype: Node
        """
        def dfs(cur):
            if not cur:
                return
            dfs(cur.left)
            if self.pre:
                self.pre.right, cur.left = cur, self.pre
            else:
                self.head = cur
            self.pre = cur
            dfs(cur.right)

        if not root:
            return
        
        self.pre = None
        dfs(root)
        self.head.left, self.pre.right = self.pre, self.head
        return self.head
执行用时：36 ms, 在所有 Python 提交中击败了50.00%的用户
内存消耗：13.9 MB, 在所有 Python 提交中击败了83.18%的用户
通过测试用例：50 / 50
```

非递归实现中序遍历

二叉搜索树的中序遍历：递增序列

用两个队列实现，一个队列p用于保存遍历的节点，另一个队列q保存中序遍历的递增序列
p队列，先进先出，如果正在遍历的节点有左子节点，就把正在遍历的节点append进p队列，并把左子节点也append进p队列，并把p的左子节点置为None
如果正在遍历的左子节点为空，就把正在遍历的节点append进q队列
如果正在遍历的右子节点不为空，就把右子节点append进p队列
直至p为空结束遍历
此时q是一个递增序列
然后构造新节点，创建新的双向链表就可以了
考虑特殊情况，根节点为空
链接：https://leetcode-cn.com/problems/er-cha-sou-suo-shu-yu-shuang-xiang-lian-biao-lcof/solution/fei-di-gui-shi-xian-shen-du-you-xian-bia-hpl4/

```python
"""
# Definition for a Node.
class Node(object):
    def __init__(self, val, left=None, right=None):
        self.val = val
        self.left = left
        self.right = right
"""
class Solution(object):
    def treeToDoublyList(self, root):
        """
        :type root: Node
        :rtype: Node
        """
        if not root:
            return root
        stack = [root]
        new_stack = []
        while stack:
            cur = stack.pop()
            if cur.left:
                stack.append(cur)
                l = cur.left
                cur.left = None
                stack.append(l)
                continue
            new_stack.append(cur.val)
            if cur.right:
                stack.append(cur.right)
        head = Node(-1)
        cur = head
        for val in new_stack:
            new_node = Node(val)
            cur.right = new_node
            new_node.left = cur
            cur = cur.right
        cur.right = head.right
        head.right.left = cur
        return head.right
执行用时：36 ms, 在所有 Python 提交中击败了50.00%的用户
内存消耗：13.9 MB, 在所有 Python 提交中击败了82.49%的用户
通过测试用例：50 / 50
```

非递归形式+Python生成器

链接：https://leetcode-cn.com/problems/er-cha-sou-suo-shu-yu-shuang-xiang-lian-biao-lcof/solution/fei-di-gui-xing-shi-pythonsheng-cheng-qi-h5of/

```Python
"""
# Definition for a Node.
class Node(object):
    def __init__(self, val, left=None, right=None):
        self.val = val
        self.left = left
        self.right = right
"""
class Solution(object):
    def treeToDoublyList(self, root):
        """
        :type root: Node
        :rtype: Node
        """
        def generator(root):
            stack = []
            cur = root
            while cur is not None or stack:
                while cur is not None:
                    stack.append(cur)
                    cur = cur.left
                cur = stack.pop()
                yield cur
                cur = cur.right
        if not root:
            return root
        
        gen = generator(root)
        head = next(gen)
        cur = head
        for node in gen:
            cur.right = node
            node.left = cur
            cur = node
        head.left = cur
        cur.right = head
        return head
执行用时：32 ms, 在所有 Python 提交中击败了79.03%的用户
内存消耗：13.6 MB, 在所有 Python 提交中击败了96.54%的用户
通过测试用例：50 / 50
```

#### [剑指 Offer 54. 二叉搜索树的第k大节点](https://leetcode-cn.com/problems/er-cha-sou-suo-shu-de-di-kda-jie-dian-lcof/)

先中序遍历，在找第k大节点

```python
# Definition for a binary tree node.
# class TreeNode(object):
#     def __init__(self, x):
#         self.val = x
#         self.left = None
#         self.right = None

class Solution(object):
    def kthLargest(self, root, k):
        """
        :type root: TreeNode
        :type k: int
        :rtype: int
        """
        stack = []
        def dfs(root):
            if not root:
                return
            dfs(root.left)
            stack.append(root.val)
            dfs(root.right)
        dfs(root)
        return stack[-k]
执行用时：36 ms, 在所有 Python 提交中击败了70.19%的用户
内存消耗：20.5 MB, 在所有 Python 提交中击败了83.30%的用户
通过测试用例：91 / 91
```

倒中序遍历

```Python
# Definition for a binary tree node.
# class TreeNode(object):
#     def __init__(self, x):
#         self.val = x
#         self.left = None
#         self.right = None

class Solution(object):
    def kthLargest(self, root, k):
        """
        :type root: TreeNode
        :type k: int
        :rtype: int
        """
        stack = []
        while root or stack:
            while root:
                stack.append(root)
                root = root.right
            root = stack.pop()
            k -= 1
            if k == 0:
                return root.val
            root = root.left
执行用时：24 ms, 在所有 Python 提交中击败了99.32%的用户
内存消耗：20.4 MB, 在所有 Python 提交中击败了92.67%的用户
通过测试用例：91 / 91
```

#### [剑指 Offer 45. 把数组排成最小的数](https://leetcode-cn.com/problems/ba-shu-zu-pai-cheng-zui-xiao-de-shu-lcof/)

解题思路：
此题求拼接起来的最小数字，本质上是一个排序问题。设数组 numsnums 中任意两数字的字符串为 x 和 y ，则规定 排序判断规则 为：

若拼接字符串 x + y > y + x ，则 x “大于” y ；
反之，若 x + y < y + x，则 x “小于” y ；
x“小于” y 代表：排序完成后，数组中 x 应在 y 左边；“大于” 则反之。

根据以上规则，套用任何排序方法对 nums 执行排序即可。

![Picture1.png](https://pic.leetcode-cn.com/95e81dbccc44f26292d88c509afd68204a86b37d342f83d109fa7aa0cd4a6049-Picture1.png)
链接：https://leetcode-cn.com/problems/ba-shu-zu-pai-cheng-zui-xiao-de-shu-lcof/solution/mian-shi-ti-45-ba-shu-zu-pai-cheng-zui-xiao-de-s-4/
快速排序（自己写的)

```python
class Solution(object):
    def minNumber(self, nums):
        """
        :type nums: List[int]
        :rtype: str
        """
        def quicksort(ilist):
            if len(ilist) <= 1:
                return ilist
            left = []
            right = []
            for i in ilist[1:]:
                if str(i) + str(ilist[0]) <= str(ilist[0]) + str(i):
                    left.append(i)
                else:
                    right.append(i)
            return quicksort(left) + [ilist[0]] + quicksort(right)
        nums = quicksort(nums)
        strs = [str(num) for num in nums]
        return ''.join(strs)
执行用时：36 ms, 在所有 Python 提交中击败了30.98%的用户
内存消耗：13.2 MB, 在所有 Python 提交中击败了48.42%的用户
通过测试用例：222 / 222
```

快速排序（大佬写的）

```python
class Solution(object):
    def minNumber(self, nums):
        """
        :type nums: List[int]
        :rtype: str
        """
        def quicksort(l, r):
            if l >= r:
                return 
            i, j = l, r
            while i < j:
                while strs[j] + strs[l] >= strs[l] + strs[j] and i < j: j -= 1
                while strs[i] + strs[l] <= strs[l] + strs[i] and i < j: i += 1
                strs[i], strs[j] = strs[j], strs[i]
            strs[i], strs[l] = strs[l], strs[i]#由于是j指针先开始移动，所以最后停下的时候一定是i、j均指向一个小于等于l处的值，交换i、l处的值
            quicksort(l, i-1)
            quicksort(i+1, r)
        strs = [str(num) for num in nums]
        quicksort(0, len(strs)-1)
        return ''.join(strs)
执行用时：20 ms, 在所有 Python 提交中击败了90.35%的用户
内存消耗：13.2 MB, 在所有 Python 提交中击败了39.70%的用户
通过测试用例：222 / 222
```

#### [剑指 Offer 61. 扑克牌中的顺子](https://leetcode-cn.com/problems/bu-ke-pai-zhong-de-shun-zi-lcof/)

解题思路：
根据题意，此 5 张牌是顺子的 充分条件 如下：

除大小王外，所有牌 无重复 ；
设此 5 张牌中最大的牌为 max ，最小的牌为 min（大小王除外），则需满足：
max - min < 5
max−min<5

因而，可将问题转化为：此 5 张牌是否满足以上两个条件？

![Picture1.png](https://pic.leetcode-cn.com/df03847e2d04a3fcb5649541d4b6733fb2cb0d9293c3433823e04935826c33ef-Picture1.png)
链接：https://leetcode-cn.com/problems/bu-ke-pai-zhong-de-shun-zi-lcof/solution/mian-shi-ti-61-bu-ke-pai-zhong-de-shun-zi-ji-he-se/

```python
class Solution(object):
    def isStraight(self, nums):
        """
        :type nums: List[int]
        :rtype: bool
        """
        repeat = []
        ma, mi = 0, 14
        for num in nums:
            if num == 0:
                continue
            elif num in repeat:
                return False
            else:
                repeat.append(num)
                ma = max(ma, num)
                mi = min(mi, num)
        if ma - mi < 5:
            return True
        else:
            return False
执行用时：16 ms, 在所有 Python 提交中击败了76.16%的用户
内存消耗：13.1 MB, 在所有 Python 提交中击败了37.78%的用户
通过测试用例：203 / 203
```

排序法

```python
class Solution(object):
    def isStraight(self, nums):
        """
        :type nums: List[int]
        :rtype: bool
        """
        joker = 0
        nums.sort()
        for i in range(4):
            if nums[i] == 0: joker += 1 
            elif nums[i] == nums[i + 1]: return False 
        return nums[4] - nums[joker] < 5 
执行用时：12 ms, 在所有 Python 提交中击败了93.13%的用户
内存消耗：13 MB, 在所有 Python 提交中击败了55.15%的用户
通过测试用例：203 / 203
```

#### [剑指 Offer 40. 最小的k个数](https://leetcode-cn.com/problems/zui-xiao-de-kge-shu-lcof/)

排序后输出

```Python
class Solution(object):
    def getLeastNumbers(self, arr, k):
        """
        :type arr: List[int]
        :type k: int
        :rtype: List[int]
        """
        def dfs(l, r):
            if l >= r:
                return
            i, j = l, r
            while i < j:
                while arr[j] >= arr[l] and i < j: j -= 1
                while arr[i] <= arr[l] and i < j: i += 1
                arr[i], arr[j] = arr[j], arr[i]
            arr[i], arr[l] = arr[l], arr[i]
            dfs(l, i-1)
            dfs(i+1, r)
        if k == 0:
            return []
        dfs(0, len(arr)-1)
        return arr[:k]
执行用时：196 ms, 在所有 Python 提交中击败了17.62%的用户
内存消耗：16.5 MB, 在所有 Python 提交中击败了14.60%的用户
通过测试用例：38 / 38
```

使用库函数

```python
class Solution(object):
    def getLeastNumbers(self, arr, k):
        """
        :type arr: List[int]
        :type k: int
        :rtype: List[int]
        """
        arr.sort()
        if k == 0:
            return []
        return arr[:k]
执行用时：36 ms, 在所有 Python 提交中击败了90.05%的用户
内存消耗：14.3 MB, 在所有 Python 提交中击败了52.33%的用户
通过测试用例：38 / 38
```

题目只要求返回最小的 k 个数，对这 k 个数的顺序并没有要求。因此，只需要将数组划分为 最小的 kk 个数 和 其他数字 两部分即可，而快速排序的哨兵划分可完成此目标。
链接：https://leetcode-cn.com/problems/zui-xiao-de-kge-shu-lcof/solution/jian-zhi-offer-40-zui-xiao-de-k-ge-shu-j-9yze/

```python
class Solution(object):
    def getLeastNumbers(self, arr, k):
        """
        :type arr: List[int]
        :type k: int
        :rtype: List[int]
        """
        def dfs(l, r):
            i, j = l, r
            while i < j:
                while arr[j] >= arr[l] and i < j: j -= 1
                while arr[i] <= arr[l] and i < j: i += 1
                arr[i], arr[j] = arr[j], arr[i]
            arr[i], arr[l] = arr[l], arr[i]
            if k < i: return dfs(l, i-1)
            if k > i: return dfs(i+1, r)
            return arr[:k]
        if k == 0:
            return []
        elif k >= len(arr):
            return arr
        else:
            return dfs(0, len(arr)-1)
执行用时：80 ms, 在所有 Python 提交中击败了42.05%的用户
内存消耗：16.7 MB, 在所有 Python 提交中击败了5.73%的用户
通过测试用例：38 / 38
```

堆排序

```python
class Solution(object):
    def getLeastNumbers(self, arr, k):
        """
        :type arr: List[int]
        :type k: int
        :rtype: List[int]
        """
        if k == 0:
            return []
        elif k >= len(arr):
            return arr
        hp = [-x for x in arr[:k]]
        heapq.heapify(hp)
        for i in range(k, len(arr)):
            if -hp[0] > arr[i]:
                heapq.heappop(hp)
                heapq.heappush(hp, -arr[i])
        ans = [-x for x in hp]
        return ans
执行用时：60 ms, 在所有 Python 提交中击败了62.27%的用户
内存消耗：14.5 MB, 在所有 Python 提交中击败了37.41%的用户
通过测试用例：38 / 38
```

#### [剑指 Offer 41. 数据流中的中位数](https://leetcode-cn.com/problems/shu-ju-liu-zhong-de-zhong-wei-shu-lcof/)

解题思路：
给定一长度为 N 的无序数组，其中位数的计算方法：首先对数组执行排序（使用 O(NlogN) 时间），然后返回中间元素即可（使用O(1) 时间）。

针对本题，根据以上思路，可以将数据流保存在一个列表中，并在添加元素时 保持数组有序 。此方法的时间复杂度为 O(N) ，其中包括： 查找元素插入位置 O(logN) （二分查找）、向数组某位置插入元素O(N) （插入位置之后的元素都需要向后移动一位）。

借助 堆 可进一步优化时间复杂度。

建立一个 小顶堆 A 和 大顶堆 B ，各保存列表的一半元素，且规定：

A 保存 较大 的一半，长度为$ \frac{N}{2} $（ N 为偶数）或$ \frac{N+1}{2} $（ N 为奇数）；
B 保存 较小 的一半，长度为$ \frac{N}{2} $（ N 为偶数）或$ \frac{N-1}{2} $（ N 为奇数）；
随后，中位数可仅根据 A, B的堆顶元素计算得到。
链接：https://leetcode-cn.com/problems/shu-ju-liu-zhong-de-zhong-wei-shu-lcof/solution/mian-shi-ti-41-shu-ju-liu-zhong-de-zhong-wei-shu-y/
![Picture1.png](https://pic.leetcode-cn.com/25837f1b195e56de20587a4ed97d9571463aa611789e768914638902add351f4-Picture1.png)

```python
class MedianFinder(object):

    def __init__(self):
        """
        initialize your data structure here.
        """
        self.A = []
        self.B = []


    def addNum(self, num):
        """
        :type num: int
        :rtype: None
        """
        if len(self.A) == len(self.B):
            heapq.heappush(self.B, -num)
            heapq.heappush(self.A, -heapq.heappop(self.B))
        else:
            heapq.heappush(self.A, num)
            heapq.heappush(self.B, -heapq.heappop(self.A))


    def findMedian(self):
        """
        :rtype: float
        """
        return self.A[0] if len(self.A) != len(self.B) else (self.A[0] - self.B[0]) / 2.0
执行用时：460 ms, 在所有 Python 提交中击败了58.41%的用户
内存消耗：25.4 MB, 在所有 Python 提交中击败了21.96%的用户
通过测试用例：18 / 18
```

#### [剑指 Offer 55 - I. 二叉树的深度](https://leetcode-cn.com/problems/er-cha-shu-de-shen-du-lcof/)

最大广度搜索。创建两个队列，一个用于保存当前层的节点，另一个保存当前层节点的子节点。当当前层节点遍历完毕深度+1，将下一层的节点赋予当前层队列并清空下一层节点队列，循环执行直至两个队列均为空。

```Python
# Definition for a binary tree node.
# class TreeNode(object):
#     def __init__(self, x):
#         self.val = x
#         self.left = None
#         self.right = None

class Solution(object):
    def maxDepth(self, root):
        """
        :type root: TreeNode
        :rtype: int
        """
        if not root:
            return 0
        depth = 0
        q = collections.deque()
        s = collections.deque()
        q.append(root)
        while q:
            temp = q.popleft()
            if temp.left:
                s.append(temp.left)
            if temp.right:
                s.append(temp.right)
            if not q:
                depth += 1
                if not s:
                    break
                else:
                    q, s = s, q
        return depth
执行用时：28 ms, 在所有 Python 提交中击败了49.81%的用户
内存消耗：15.8 MB, 在所有 Python 提交中击败了36.62%的用户
通过测试用例：39 / 39
```

树的遍历方式总体分为两类：深度优先搜索（DFS）、广度优先搜索（BFS）；

* 常见的 DFS ： 先序遍历、中序遍历、后序遍历；
* 常见的 BFS ： 层序遍历（即按层遍历）。

求树的深度需要遍历树的所有节点，本文将介绍基于 后序遍历（DFS） 和 层序遍历（BFS） 的两种解法。
方法一：后序遍历（DFS）
树的后序遍历 / 深度优先搜索往往利用 递归 或 栈 实现，本文使用递归实现。
关键点： 此树的深度和其左（右）子树的深度之间的关系。显然，此树的深度 等于 左子树的深度 与 右子树的深度 中的 最大值+1 。
链接：https://leetcode-cn.com/problems/er-cha-shu-de-shen-du-lcof/solution/mian-shi-ti-55-i-er-cha-shu-de-shen-du-xian-xu-bia/

![Picture1.png](https://pic.leetcode-cn.com/9b063f1f2b7ba125b97a2a11c5f774c0f8ff4df594696993a8eb8282750dae0d-Picture1.png)

算法解析：
终止条件： 当 root 为空，说明已越过叶节点，因此返回 深度 00 。
递推工作： 本质上是对树做后序遍历。
计算节点 root 的 左子树的深度 ，即调用 maxDepth(root.left)；
计算节点 root 的 右子树的深度 ，即调用 maxDepth(root.right)；
返回值： 返回 此树的深度 ，即 max(maxDepth(root.left), maxDepth(root.right)) + 1。
链接：https://leetcode-cn.com/problems/er-cha-shu-de-shen-du-lcof/solution/mian-shi-ti-55-i-er-cha-shu-de-shen-du-xian-xu-bia/

```python
# Definition for a binary tree node.
# class TreeNode(object):
#     def __init__(self, x):
#         self.val = x
#         self.left = None
#         self.right = None

class Solution(object):
    def maxDepth(self, root):
        """
        :type root: TreeNode
        :rtype: int
        """

        if not root:
            return 0
        else:
            return max(self.maxDepth(root.left), self.maxDepth(root.right)) + 1
执行用时：24 ms, 在所有 Python 提交中击败了79.74%的用户
内存消耗：15.6 MB, 在所有 Python 提交中击败了84.20%的用户
通过测试用例：39 / 39
```

#### [剑指 Offer 55 - II. 平衡二叉树](https://leetcode-cn.com/problems/ping-heng-er-cha-shu-lcof/)

由于可能存在子树之间最大深度符合条件，但是子树内部的左右子树不符合条件的情况（节点深度由左右子树的最大值决定，最小值不影响），故需要遍历所有可能节点。先利用递归求出节点左右子树的深度比较是否符合条件，符合则递归地遍历该节点的左右子节点直至节点为空。否则返回False。

```python
# Definition for a binary tree node.
# class TreeNode(object):
#     def __init__(self, x):
#         self.val = x
#         self.left = None
#         self.right = None

class Solution(object):
    def isBalanced(self, root):
        """
        :type root: TreeNode
        :rtype: bool
        """
        def dfs(root):
            if not root:
                return 0
            else:
                return max(dfs(root.left), dfs(root.right)) + 1
        if not root:
            return True
        def isB(root):
            if not root:
                return True
            elif abs(dfs(root.left) - dfs(root.right)) > 1:
                return False
            else:
                return isB(root.left) and isB(root.right)
        return isB(root)
执行用时：40 ms, 在所有 Python 提交中击败了52.26%的用户
内存消耗：18.2 MB, 在所有 Python 提交中击败了57.96%的用户
通过测试用例：227 / 227
```

官方代码优化：

https://leetcode-cn.com/problems/ping-heng-er-cha-shu-lcof/solution/ping-heng-er-cha-shu-by-leetcode-solutio-6r1g/

```python
# Definition for a binary tree node.
# class TreeNode(object):
#     def __init__(self, x):
#         self.val = x
#         self.left = None
#         self.right = None

class Solution(object):
    def isBalanced(self, root):
        """
        :type root: TreeNode
        :rtype: bool
        """
        def dfs(root):
            if not root:
                return 0
            else:
                return max(dfs(root.left), dfs(root.right)) + 1
        if not root:
            return True
        else:
            return abs(dfs(root.left) - dfs(root.right)) <= 1 and self.isBalanced(root.left) and self.isBalanced(root.right)
执行用时：36 ms, 在所有 Python 提交中击败了66.75%的用户
内存消耗：18.4 MB, 在所有 Python 提交中击败了12.35%的用户
通过测试用例：227 / 227
```

方法二：自底向上的递归
上面的方法由于是自顶向下递归，因此对于同一个节点，函数 dfs 会被重复调用，导致时间复杂度较高。如果使用自底向上的做法，则对于每个节点，函数 dfs 只会被调用一次。

自底向上递归的做法类似于后序遍历，对于当前遍历到的节点，先递归地判断其左右子树是否平衡，再判断以当前节点为根的子树是否平衡。如果一棵子树是平衡的，则返回其高度（高度一定是非负整数），否则返回−1。如果存在一棵子树不平衡，则整个二叉树一定不平衡。

作者：LeetCode-Solution
链接：https://leetcode-cn.com/problems/ping-heng-er-cha-shu-lcof/solution/ping-heng-er-cha-shu-by-leetcode-solutio-6r1g/

```python
# Definition for a binary tree node.
# class TreeNode(object):
#     def __init__(self, x):
#         self.val = x
#         self.left = None
#         self.right = None

class Solution(object):
    def isBalanced(self, root):
        """
        :type root: TreeNode
        :rtype: bool
        """
        def height(root):
            if not root:
                return 0
            leftheight = height(root.left)
            rightheight = height(root.right)
            if leftheight == -1 or rightheight == -1 or abs(leftheight - rightheight) > 1:
                return -1
            else:
                return max(leftheight, rightheight) + 1
        return height(root) >= 0
执行用时：20 ms, 在所有 Python 提交中击败了99.52%的用户
内存消耗：18.3 MB, 在所有 Python 提交中击败了31.36%的用户
通过测试用例：227 / 227
```

#### [剑指 Offer 64. 求1+2+…+n](https://leetcode-cn.com/problems/qiu-12n-lcof/)

链接：https://leetcode-cn.com/problems/qiu-12n-lcof/solution/mian-shi-ti-64-qiu-1-2-nluo-ji-fu-duan-lu-qing-xi-/

链接：https://leetcode-cn.com/problems/qiu-12n-lcof/solution/64-pythonji-chu-ni-que-ding-ni-zhi-dao-ma-by-lulla/

（1）如果多个变量均非0（包括None、False等），那么返回最后一个变量的值。如3 and 2 and 'a'的返回值为'a'；
（2）如果多个变量中存在0值，则返回第一个0值。如1 and 'a' and 0 and None的返回值为0。

利用and替代if，利用递归替代while。

```python
class Solution(object):
    def sumNums(self, n):
        """
        :type n: int
        :rtype: int
        """
        return n and self.sumNums(n-1) + n
执行用时：32 ms, 在所有 Python 提交中击败了39.09%的用户
内存消耗：20.1 MB, 在所有 Python 提交中击败了47.88%的用户
通过测试用例：35 / 35
```

#### [剑指 Offer 68 - I. 二叉搜索树的最近公共祖先](https://leetcode-cn.com/problems/er-cha-sou-suo-shu-de-zui-jin-gong-gong-zu-xian-lcof/)

思路与算法

我们从根节点开始遍历；

如果当前节点的值大于 p 和 q 的值，说明 p 和 q 应该在当前节点的左子树，因此将当前节点移动到它的左子节点；

如果当前节点的值小于 p 和 q 的值，说明 p 和 q 应该在当前节点的右子树，因此将当前节点移动到它的右子节点；

如果当前节点的值不满足上述两条要求，那么说明当前节点就是「分岔点」。此时，p 和 q 要么在当前节点的不同的子树中，要么其中一个就是当前节点。

可以发现，如果我们将这两个节点放在一起遍历，我们就省去了存储路径需要的空间。

作者：LeetCode-Solution
链接：https://leetcode-cn.com/problems/er-cha-sou-suo-shu-de-zui-jin-gong-gong-zu-xian-lcof/solution/er-cha-sou-suo-shu-de-zui-jin-gong-gong-0wpw1/

```python
# Definition for a binary tree node.
# class TreeNode(object):
#     def __init__(self, x):
#         self.val = x
#         self.left = None
#         self.right = None

class Solution(object):
    def lowestCommonAncestor(self, root, p, q):
        """
        :type root: TreeNode
        :type p: TreeNode
        :type q: TreeNode
        :rtype: TreeNode
        """
        temp = root
        while temp:
            if p.val < temp.val and q.val < temp.val:
                temp = temp.left
            elif p.val > temp.val and q.val > temp.val:
                temp = temp.right
            else:
                break
        return temp
执行用时：48 ms, 在所有 Python 提交中击败了87.07%的用户
内存消耗：20.8 MB, 在所有 Python 提交中击败了85.92%的用户
通过测试用例：27 / 27
```

#### [剑指 Offer 68 - II. 二叉树的最近公共祖先](https://leetcode-cn.com/problems/er-cha-shu-de-zui-jin-gong-gong-zu-xian-lcof/)

相较于上一题，此题没有指定为二叉搜索树，因而不能利用数值判断。

考虑通过递归对二叉树进行先序遍历，当遇到节点 pp 或 qq 时返回。从底至顶回溯，当节点 p, q在节点 root 的异侧时，节点 root 即为最近公共祖先，则向上返回 root 。

递归解析：

1. 终止条件：
   * 当越过叶节点，则直接返回 null ；
   * 当 root等于 p, q，则直接返回 root ；
2. 递推工作：
   * 开启递归左子节点，返回值记为 left ；
   * 开启递归右子节点，返回值记为 right ；
3. 返回值： 根据 left和 right ，可展开为四种情况；
   * 当 left 和 right 同时为空 ：说明 root的左 / 右子树中都不包含 p,q ，返回 null ；
   * 当 left和 right 同时不为空 ：说明 p, q分列在 root的 异侧 （分别在 左 / 右子树），因此 root为最近公共祖先，返回 root；
   * 当 left为空 ，right 不为空 ：p,q 都不在 root的左子树中，直接返回 right 。具体可分为两种情况：
     * p,q 其中一个在 root的 右子树 中，此时 right 指向 p（假设为 p ）；
     * p,q两节点都在 root 的 右子树 中，此时的 right 指向 最近公共祖先节点 ；
4. 当 left 不为空 ， right 为空 ：与情况 3. 同理；

作者：jyd
链接：https://leetcode-cn.com/problems/er-cha-shu-de-zui-jin-gong-gong-zu-xian-lcof/solution/mian-shi-ti-68-ii-er-cha-shu-de-zui-jin-gong-gon-7/

```python
# Definition for a binary tree node.
# class TreeNode(object):
#     def __init__(self, x):
#         self.val = x
#         self.left = None
#         self.right = None

class Solution(object):
    def lowestCommonAncestor(self, root, p, q):
        """
        :type root: TreeNode
        :type p: TreeNode
        :type q: TreeNode
        :rtype: TreeNode
        """
        if not root or root == p or root == q:
            return root
        left = self.lowestCommonAncestor(root.left, p, q)
        right = self.lowestCommonAncestor(root.right, p, q)
        if not left:
            return right
        if not right:
            return left
        return root
执行用时：52 ms, 在所有 Python 提交中击败了54.10%的用户
内存消耗：24.8 MB, 在所有 Python 提交中击败了63.83%的用户
通过测试用例：31 / 31
```

#### [剑指 Offer 07. 重建二叉树](https://leetcode-cn.com/problems/zhong-jian-er-cha-shu-lcof/)

解题思路：
前序遍历性质： 节点按照 [ 根节点 | 左子树 | 右子树 ] 排序。
中序遍历性质： 节点按照 [ 左子树 | 根节点 | 右子树 ] 排序。

以题目示例为例：

前序遍历划分 [ 3 | 9 | 20 15 7 ]
中序遍历划分 [ 9 | 3 | 15 20 7 ]
根据以上性质，可得出以下推论：

前序遍历的首元素 为 树的根节点 node 的值。
在中序遍历中搜索根节点 node 的索引 ，可将 中序遍历 划分为 [ 左子树 | 根节点 | 右子树 ] 。
根据中序遍历中的左（右）子树的节点数量，可将 前序遍历 划分为 [ 根节点 | 左子树 | 右子树 ] 。

![Picture1.png](https://pic.leetcode-cn.com/1629825510-roByLr-Picture1.png)

通过以上三步，可确定 三个节点 ：1.树的根节点、2.左子树根节点、3.右子树根节点。

根据「分治算法」思想，对于树的左、右子树，仍可复用以上方法划分子树的左右子树。

分治算法解析：
递推参数： 根节点在前序遍历的索引 root 、子树在中序遍历的左边界 left 、子树在中序遍历的右边界 right ；

终止条件： 当 left > right ，代表已经越过叶节点，此时返回 nullnull ；

递推工作：

建立根节点 node ： 节点值为 preorder[root] ；
划分左右子树： 查找根节点在中序遍历 inorder 中的索引 i ；
为了提升效率，本文使用哈希表 dic 存储中序遍历的值与索引的映射，查找操作的时间复杂度为 O(1)O(1) ；

构建左右子树： 开启左右子树递归；

![image-20211122160600419](E:\MyGit\LeetCode\剑指offer.assets\image-20211122160600419.png)

TIPS： i - left + root + 1含义为 根节点索引 + 左子树长度 + 1

返回值： 回溯返回 node ，作为上一层递归中根节点的左 / 右子节点；

复杂度分析：
时间复杂度 O(N) ： 其中 N 为树的节点数量。初始化 HashMap 需遍历 inorder ，占用 O(N) 。递归共建立 N 个节点，每层递归中的节点建立、搜索操作占用 O(1) ，因此使用 O(N)时间。
空间复杂度 O(N) ： HashMap 使用 O(N) 额外空间；最差情况下（输入二叉树为链表时），递归深度达到 N ，占用 O(N) 的栈帧空间；因此总共使用 O(N)空间。

作者：jyd
链接：https://leetcode-cn.com/problems/zhong-jian-er-cha-shu-lcof/solution/mian-shi-ti-07-zhong-jian-er-cha-shu-di-gui-fa-qin/

```python
# Definition for a binary tree node.
# class TreeNode(object):
#     def __init__(self, x):
#         self.val = x
#         self.left = None
#         self.right = None

class Solution(object):
    def buildTree(self, preorder, inorder):
        """
        :type preorder: List[int]
        :type inorder: List[int]
        :rtype: TreeNode
        """
        def recur(root, left, right):
            if left > right:
                return 
            node = TreeNode(preorder[root])
            i = dic[preorder[root]]
            node.left = recur(root+1, left, i-1)
            node.right = recur(i-left+root+1, i+1, right)#i-left为左子树长度，root+1为当前左子树根节点位置，前序遍历性质： 节点按照 [ 根节点 | 左子树 | 右子树 ] 排序。左子树根节点右移左子树长度即为右子树根节点位置。
            return node
        dic = {}
        for i in range(len(inorder)):
            dic[inorder[i]] = i
        return recur(0, 0, len(inorder)-1)
执行用时：24 ms, 在所有 Python 提交中击败了92.86%的用户
内存消耗：17.5 MB, 在所有 Python 提交中击败了67.86%的用户
通过测试用例：203 / 203
```

#### [剑指 Offer 16. 数值的整数次方](https://leetcode-cn.com/problems/shu-zhi-de-zheng-shu-ci-fang-lcof/)

利用位运算。

对于n<0，将n取绝对值，并将x取倒数，从而合并正负两种情况。

对于n，通过每次右移一位并判断它和1“与”的结果，将$x^n$分解为$x, x^2, x^4, ...$某些组合连乘的结果。举个例子，对于n=10，它的二进制是1010，可以将$x^{10}$表示为$x^2 * x^8$，即$x^{2^3} * x^{2^1}$

```python 
class Solution(object):
    def myPow(self, x, n):
        """
        :type x: float
        :type n: int
        :rtype: float
        """
        if n < 0:
            x = 1/x
            n = abs(n)
        out = 1            
        while n > 0:
            if n & 1:
                out = out * x
            x = x * x
            n >>= 1
        return out
执行用时：16 ms, 在所有 Python 提交中击败了87.61%的用户
内存消耗：12.9 MB, 在所有 Python 提交中击败了88.04%的用户
通过测试用例：304 / 304
```

#### [剑指 Offer 33. 二叉搜索树的后序遍历序列](https://leetcode-cn.com/problems/er-cha-sou-suo-shu-de-hou-xu-bian-li-xu-lie-lcof/)

方法一：递归分治
根据二叉搜索树的定义，可以通过递归，判断所有子树的 正确性 （即其后序遍历是否满足二叉搜索树的定义） ，若所有子树都正确，则此序列为二叉搜索树的后序遍历。
递归解析：
终止条件： 当i≥j ，说明此子树节点数量 ≤1 ，无需判别正确性，因此直接返回 true ；
递推工作：
划分左右子树： 遍历后序遍历的 \[i, j\] 区间元素，寻找 第一个大于根节点 的节点，索引记为 m 。此时，可划分出左子树区间 \[i,m-1\] 、右子树区间 \[m, j - 1\]、根节点索引 j。
判断是否为二叉搜索树：
左子树区间 \[i, m - 1\]内的所有节点都应 <postorder[j] 。而第 1.划分左右子树 步骤已经保证左子树区间的正确性，因此只需要判断右子树区间即可。
右子树区间\[m, j-1\]内的所有节点都应 >postorder[j] 。实现方式为遍历，当遇到 ≤postorder[j] 的节点则跳出；则可通过 p = j 判断是否为二叉搜索树。
返回值： 所有子树都需正确才可判定正确，因此使用 与逻辑符 \&\&连接。
p = j： 判断 此树 是否正确。
recur(i, m - 1) ： 判断 此树的左子树 是否正确。
recur(m, j - 1)： 判断 此树的右子树 是否正确。

作者：jyd
链接：https://leetcode-cn.com/problems/er-cha-sou-suo-shu-de-hou-xu-bian-li-xu-lie-lcof/solution/mian-shi-ti-33-er-cha-sou-suo-shu-de-hou-xu-bian-6/

```python
class Solution(object):
    def verifyPostorder(self, postorder):
        """
        :type postorder: List[int]
        :rtype: bool
        """
        def recur(i, j):
            if i >= j:
                return True
            p = i
            while postorder[p] < postorder[j]:
                p += 1
            m = p
            while postorder[p] > postorder[j]:
                p += 1
            return p == j and recur(i, m-1) and recur(m, j-1)
        return recur(0, len(postorder)-1)
执行用时：12 ms, 在所有 Python 提交中击败了97.21%的用户
内存消耗：13 MB, 在所有 Python 提交中击败了71.07%的用户
通过测试用例：23 / 23
```

#### [剑指 Offer 15. 二进制中1的个数](https://leetcode-cn.com/problems/er-jin-zhi-zhong-1de-ge-shu-lcof/)

位运算，每次将n右移一位，并将其与1进行“与”运算，从而判断最后一位是不是1.

```python
class Solution(object):
    def hammingWeight(self, n):
        """
        :type n: int
        :rtype: int
        """
        total = 0
        while n > 0:
            if n & 1 :
                total += 1
            n >>= 1
        return total
执行用时：12 ms, 在所有 Python 提交中击败了94.75%的用户
内存消耗：13.1 MB, 在所有 Python 提交中击败了43.49%的用户
通过测试用例：601 / 601
```

方法二：巧用 n \& (n - 1)
(n−1) 解析： 二进制数字 n 最右边的 1 变成 0 ，此 1 右边的 0 都变成 1 。
n \& (n - 1) 解析： 二进制数字 n 最右边的 1 变成 0 ，其余不变。

![Picture10.png](https://pic.leetcode-cn.com/9bc8ab7ba242888d5291770d35ef749ae76ee2f1a51d31d729324755fc4b1b1c-Picture10.png)

算法流程：

1. 初始化数量统计变量 res 。
2. 循环消去最右边的 1 ：当 n = 0 时跳出。
   * res += 1 ： 统计变量加 1 ；
   * n &= n - 1 ： 消去数字 n 最右边的 1 。
3. 返回统计数量 res 。

作者：jyd
链接：https://leetcode-cn.com/problems/er-jin-zhi-zhong-1de-ge-shu-lcof/solution/mian-shi-ti-15-er-jin-zhi-zhong-1de-ge-shu-wei-yun/

```python
class Solution(object):
    def hammingWeight(self, n):
        """
        :type n: int
        :rtype: int
        """
        total = 0
        while n > 0:
            total += 1
            n &= n-1
        return total
执行用时：12 ms, 在所有 Python 提交中击败了94.75%的用户
内存消耗：12.9 MB, 在所有 Python 提交中击败了77.67%的用户
通过测试用例：601 / 601
```

#### [剑指 Offer 65. 不用加减乘除做加法](https://leetcode-cn.com/problems/bu-yong-jia-jian-cheng-chu-zuo-jia-fa-lcof/)

很困难的简单题，解题过程参考大佬链接：https://leetcode-cn.com/problems/bu-yong-jia-jian-cheng-chu-zuo-jia-fa-lcof/solution/mian-shi-ti-65-bu-yong-jia-jian-cheng-chu-zuo-ji-7/

```python
class Solution(object):
    def add(self, a, b):
        """
        :type a: int
        :type b: int
        :rtype: int
        """
        x = 0xffffffff
        a, b = a & x, b & x
        while b != 0:
            a, b = (a ^ b), (a & b) << 1 & x
        return a if a <= 0x7fffffff else ~(a ^ x)
执行用时：12 ms, 在所有 Python 提交中击败了90.32%的用户
内存消耗：13.1 MB, 在所有 Python 提交中击败了30.88%的用户
通过测试用例：51 / 51
```

#### [剑指 Offer 56 - I. 数组中数字出现的次数](https://leetcode-cn.com/problems/shu-zu-zhong-shu-zi-chu-xian-de-ci-shu-lcof/)

位运算，本题主要是运用相同的数字与自身“异或”运算时为0的思路来解决。关键是题中有两个不同的数字，且相同数字是随机分布在数组中，并非连续出现。题目本身限制了空间和运行时间。导致哈希表和暴力法失效。

通过将所有的元素都异或一遍，剩下的就是两个不同元素的异或结果。由于它们是不同的数字，必有一位二进制数位异或结果为1。找到这个位置。我们可以将数组元素分开两组。一组是这个位置二进制为1的数字，它们包含成对出现的和一个单独出现的。另一组则是该位为0的数字。由于二进制只有两个情况，所有数字都会分为两类。极端情况是一类只有一个不同的数字，另一类包含所有相同数字和另一个不同数字。再对两类做异或操作，最后的结果就是两个不同的数字。

链接：https://leetcode-cn.com/problems/shu-zu-zhong-shu-zi-chu-xian-de-ci-shu-lcof/solution/jian-zhi-offer-56-i-shu-zu-zhong-shu-zi-tykom/

```python
class Solution(object):
    def singleNumbers(self, nums):
        """
        :type nums: List[int]
        :rtype: List[int]
        """
        x, y, n, m = 0, 0, 0, 1
        for num in nums:
            n ^= num
        while n & m == 0:
            m <<= 1
        for num in nums:
            if num & m:
                x ^= num
            else:
                y ^= num
        return [x, y]
执行用时：20 ms, 在所有 Python 提交中击败了98.06%的用户
内存消耗：13.9 MB, 在所有 Python 提交中击败了59.83%的用户
通过测试用例：35 / 35
```

#### [剑指 Offer 56 - II. 数组中数字出现的次数 II](https://leetcode-cn.com/problems/shu-zu-zhong-shu-zi-chu-xian-de-ci-shu-ii-lcof/)

暴力哈希表法：

```python
class Solution(object):
    def singleNumber(self, nums):
        """
        :type nums: List[int]
        :rtype: int
        """
        dict = {}
        for num in nums:
            if num in dict:
                dict[num] += 1
            else:
                dict[num] = 1
        for key in dict:
            if dict[key] == 1:
                return key
执行用时：28 ms, 在所有 Python 提交中击败了86.62%的用户
内存消耗：13.9 MB, 在所有 Python 提交中击败了75.35%的用户
通过测试用例：32 / 32
```

k神的状态机（位运算）

![Picture1.png](https://pic.leetcode-cn.com/28f2379be5beccb877c8f1586d8673a256594e0fc45422b03773b8d4c8418825-Picture1.png)

![Picture4.png](https://pic.leetcode-cn.com/f75d89219ad93c69757b187c64784b4c7a57dce7911884fe82f14073d654d32f-Picture4.png)

https://leetcode-cn.com/problems/shu-zu-zhong-shu-zi-chu-xian-de-ci-shu-ii-lcof/solution/mian-shi-ti-56-ii-shu-zu-zhong-shu-zi-chu-xian-d-4/

```Python
class Solution(object):
    def singleNumber(self, nums):
        """
        :type nums: List[int]
        :rtype: int
        """
        one, two = 0, 0
        for num in nums:
            one = one ^ num & ~two
            two = two ^ num & ~one
        return one
执行用时：24 ms, 在所有 Python 提交中击败了95.07%的用户
内存消耗：13.9 MB, 在所有 Python 提交中击败了86.27%的用户
通过测试用例：32 / 32
```

#### [剑指 Offer 39. 数组中出现次数超过一半的数字](https://leetcode-cn.com/problems/shu-zu-zhong-chu-xian-ci-shu-chao-guo-yi-ban-de-shu-zi-lcof/)

简单粗暴法：

先排序，由于重复数字数量超过数组长度的一半，则在数组中间的数字必定是该重复数字。

```python
class Solution(object):
    def majorityElement(self, nums):
        """
        :type nums: List[int]
        :rtype: int
        """
        nums.sort()
        m = len(nums) // 2  
        return nums[m]
执行用时：28 ms, 在所有 Python 提交中击败了51.36%的用户
内存消耗：14.2 MB, 在所有 Python 提交中击败了33.85%的用户
通过测试用例：45 / 45
```

解题思路：
本文将 “数组中出现次数超过一半的数字” 简称为 “众数” 。
需要注意的是，数学中众数的定义为 “数组中出现次数最多的数字” ，与本文定义不同。

本题常见的三种解法：

哈希表统计法： 遍历数组 nums ，用 HashMap 统计各数字的数量，即可找出 众数 。此方法时间和空间复杂度均为 O(N)。
数组排序法： 将数组 nums 排序，数组中点的元素 一定为众数。
摩尔投票法： 核心理念为 票数正负抵消 。此方法时间和空间复杂度分别为 O(N) 和 O(1)，为本题的最佳解法。
摩尔投票法：
设输入数组 nums 的众数为 x ，数组长度为 n 。

推论一： 若记 众数 的票数为 +1，非众数 的票数为 -1 ，则一定有所有数字的 票数和 > 0 。

推论二： 若数组的前 a 个数字的 票数和 = 0，则 数组剩余 (n-a)个数字的 票数和一定仍 >0 ，即后 (n-a)个数字的 众数仍为 x 。

![Picture1.png](https://pic.leetcode-cn.com/1603612327-bOQxzq-Picture1.png)

根据以上推论，记数组首个元素为 $n_1$ ，众数为 x ，遍历并统计票数。当发生 票数和 = 0时，剩余数组的众数一定不变 ，这是由于：

当  $n_1$  = x ： 抵消的所有数字中，有一半是众数 x 。
当  $n_1$ !=x ： 抵消的所有数字中，众数 x 的数量最少为 0 个，最多为一半。
利用此特性，每轮假设发生 票数和 = 0都可以 缩小剩余数组区间 。当遍历完成时，最后一轮假设的数字即为众数。

算法流程:

1. 初始化： 票数统计 votes = 0 ， 众数 x；
2. 循环： 遍历数组 nums 中的每个数字 num ；
   * 当 票数 votes 等于 0 ，则假设当前数字 num 是众数；
   * 当 num = x 时，票数 votes 自增 1 ；当 num != x 时，票数 votes 自减 1 ；
3. 返回值： 返回 x 即可；

作者：jyd
链接：https://leetcode-cn.com/problems/shu-zu-zhong-chu-xian-ci-shu-chao-guo-yi-ban-de-shu-zi-lcof/solution/mian-shi-ti-39-shu-zu-zhong-chu-xian-ci-shu-chao-3/

```python
class Solution(object):
    def majorityElement(self, nums):
        """
        :type nums: List[int]
        :rtype: int
        """
        votes = 0
        for num in nums:
            if votes == 0:
                x = num
            if num == x:
                votes += 1
            else:
                votes -= 1
        return x
执行用时：24 ms, 在所有 Python 提交中击败了73.15%的用户
内存消耗：14.1 MB, 在所有 Python 提交中击败了57.78%的用户
通过测试用例：45 / 45
```

#### [剑指 Offer 66. 构建乘积数组](https://leetcode-cn.com/problems/gou-jian-cheng-ji-shu-zu-lcof/)

解题思路：
本题的难点在于 不能使用除法 ，即需要 只用乘法 生成数组 BB 。根据题目对 B[i]B[i] 的定义，可列表格，如下图所示。

根据表格的主对角线（全为 11 ），可将表格分为 上三角 和 下三角 两部分。分别迭代计算下三角和上三角两部分的乘积，即可 不使用除法 就获得结果。

![img](https://pic.leetcode-cn.com/1624619180-vpyyqh-Picture1.png)

作者：jyd
链接：https://leetcode-cn.com/problems/gou-jian-cheng-ji-shu-zu-lcof/solution/mian-shi-ti-66-gou-jian-cheng-ji-shu-zu-biao-ge-fe/

```python 
class Solution(object):
    def constructArr(self, a):
        """
        :type a: List[int]
        :rtype: List[int]
        """
        b, tmp = [1] * len(a), 1
        for i in range(1, len(a)):
            b[i] = b[i-1] * a[i-1]
        for i in range(len(a)-2, -1, -1):
            tmp *= a[i+1]
            b[i] *= tmp
        return b
执行用时：48 ms, 在所有 Python 提交中击败了74.58%的用户
内存消耗：27 MB, 在所有 Python 提交中击败了65.25%的用户
通过测试用例：44 / 44
```

#### [剑指 Offer 14- I. 剪绳子](https://leetcode-cn.com/problems/jian-sheng-zi-lcof/)

根据数学推导可以得出当均分时可以取得最大值，均分的长度最好是3。详细查看K神的题解：https://leetcode-cn.com/problems/jian-sheng-zi-lcof/solution/mian-shi-ti-14-i-jian-sheng-zi-tan-xin-si-xiang-by/

```python
class Solution(object):
    def cuttingRope(self, n):
        """
        :type n: int
        :rtype: int
        """
        if n <= 3:
            return n-1
        a, b= n // 3, n % 3
        if b == 0:
            return int(math.pow(3, a))
        if b == 1:
            return int(math.pow(3, a-1) * 4)
        return int(math.pow(3, a) * 2)
执行用时：8 ms, 在所有 Python 提交中击败了99.62%的用户
内存消耗：12.8 MB, 在所有 Python 提交中击败了97.15%的用户
通过测试用例：50 / 50
```

动态规划：

这题用动态规划是比较好理解的

1. 我们想要求长度为n的绳子剪掉后的最大乘积，可以从前面比n小的绳子转移而来
2. 用一个dp数组记录从0到n长度的绳子剪掉后的最大乘积，也就是dp[i]表示长度为i的绳子剪成m段后的最大乘积，初始化dp[2] = 1
3. 我们先把绳子剪掉第一段（长度为j），如果只剪掉长度为1，对最后的乘积无任何增益，所以从长度为2开始剪
4. 剪了第一段后，剩下(i - j)长度可以剪也可以不剪。如果不剪的话长度乘积即为j * (i - j)；如果剪的话长度乘积即为j * dp[i - j]。取两者最大值max(j * (i - j), j * dp[i - j])
5. 第一段长度j可以取的区间为[2,i)，对所有j不同的情况取最大值，因此最终dp[i]的转移方程为
   dp[i] = max(dp[i], max(j * (i - j), j * dp[i - j]))
6. 最后返回dp[n]即可

**复杂度分析**

- 时间复杂度：O*(*$n^2$)
- 空间复杂度：O(n)

作者：edelweisskoko
链接：https://leetcode-cn.com/problems/jian-sheng-zi-lcof/solution/jian-zhi-offer-14-i-jian-sheng-zi-huan-s-xopj/

```python
class Solution(object):
    def cuttingRope(self, n):
        """
        :type n: int
        :rtype: int
        """
        dp = [0] * (n+1)
        dp[2] = 1
        for i in range(3, n+1):
            for j in range(2, i):
                dp[i] = max(dp[i], max(j * (i-j), j * dp[i-j]))
        return dp[n]
执行用时：20 ms, 在所有 Python 提交中击败了46.68%的用户
内存消耗：13 MB, 在所有 Python 提交中击败了59.77%的用户
通过测试用例：50 / 50
```

#### [剑指 Offer 57 - II. 和为s的连续正数序列](https://leetcode-cn.com/problems/he-wei-sde-lian-xu-zheng-shu-xu-lie-lcof/)

滑动窗口

大神解答链接：https://leetcode-cn.com/problems/he-wei-sde-lian-xu-zheng-shu-xu-lie-lcof/solution/shi-yao-shi-hua-dong-chuang-kou-yi-ji-ru-he-yong-h/

```python
class Solution(object):
    def findContinuousSequence(self, target):
        """
        :type target: int
        :rtype: List[List[int]]
        """
        i, j = 1, 1
        list_sum = 0
        res = []
        while i <= target // 2:
            if list_sum < target:
                list_sum += j
                j += 1
            elif list_sum > target:
                list_sum -= i
                i += 1
            else:
                arr = list(range(i, j))
                res.append(arr)
                list_sum -= i
                i += 1
        return res
执行用时：52 ms, 在所有 Python 提交中击败了77.70%的用户
内存消耗：13.3 MB, 在所有 Python 提交中击败了34.56%的用户
通过测试用例：32 / 32
```

#### [剑指 Offer 62. 圆圈中最后剩下的数字](https://leetcode-cn.com/problems/yuan-quan-zhong-zui-hou-sheng-xia-de-shu-zi-lcof/)

动态规划

模拟整个删除过程最直观，即构建一个长度为 n 的链表，各节点值为对应的顺序索引；每轮删除第 m 个节点，直至链表长度为 1 时结束，返回最后剩余节点的值即可。

模拟法需要循环删除 n - 1轮，每轮在链表中寻找删除节点需要 mm 次访问操作（链表线性遍历），因此总体时间复杂度为 O(nm) 。题目给定的 m, n 取值范围如下所示，观察可知此时间复杂度是不可接受的。

$1≤n≤10^5 \\ 1≤m≤10^6$

实际上，本题是著名的 “约瑟夫环” 问题，可使用 动态规划 解决。

输入 n, m，记此约瑟夫环问题为 「n, m问题」 ，设解（即最后留下的数字）为 f(n)，则有：

「n, m 问题」：数字环为 0, 1, 2, ..., n - 1，解为 f(n) ；
「n-1, m问题」：数字环为 0, 1, 2, ..., n - 2 ，解为 f(n-1) ；
以此类推……

作者：jyd
链接：https://leetcode-cn.com/problems/yuan-quan-zhong-zui-hou-sheng-xia-de-shu-zi-lcof/solution/jian-zhi-offer-62-yuan-quan-zhong-zui-ho-dcow/

```python
class Solution(object):
    def lastRemaining(self, n, m):
        """
        :type n: int
        :type m: int
        :rtype: int
        """
        x = 0
        for i in range(2, n+1):
            x = (x + m) % i
        return x
执行用时：40 ms, 在所有 Python 提交中击败了90.78%的用户
内存消耗：15.9 MB, 在所有 Python 提交中击败了75.53%的用户
通过测试用例：36 / 36
```

#### [剑指 Offer 29. 顺时针打印矩阵](https://leetcode-cn.com/problems/shun-shi-zhen-da-yin-ju-zhen-lcof/)

解题思路：
根据题目示例 matrix = [[1,2,3],[4,5,6],[7,8,9]] 的对应输出 [1,2,3,6,9,8,7,4,5] 可以发现，顺时针打印矩阵的顺序是 “从左向右、从上向下、从右向左、从下向上” 循环。

因此，考虑设定矩阵的“左、上、右、下”四个边界，模拟以上矩阵遍历顺序。

![Picture1.png](https://pic.leetcode-cn.com/c6de3a1bc0f38820941dbcff0e17a49204eba91b967d4ccc0d5485e68a4fcc95-Picture1.png)

算法流程：

1. 空值处理： 当 matrix 为空时，直接返回空列表 [] 即可。
2. 初始化： 矩阵 左、右、上、下 四个边界 l , r , t , b ，用于打印的结果列表 res 。
3. 循环打印： “从左向右、从上向下、从右向左、从下向上” 四个方向循环，每个方向打印中做以下三件事 （各方向的具体信息见下表） ；
   * 根据边界打印，即将元素按顺序添加至列表 res 尾部；
   * 边界向内收缩 11 （代表已被打印）；
   * 判断是否打印完毕（边界是否相遇），若打印完毕则跳出。
4. 返回值： 返回 res 即可。

作者：jyd
链接：https://leetcode-cn.com/problems/shun-shi-zhen-da-yin-ju-zhen-lcof/solution/mian-shi-ti-29-shun-shi-zhen-da-yin-ju-zhen-she-di/

```python
class Solution(object):
    def spiralOrder(self, matrix):
        """
        :type matrix: List[List[int]]
        :rtype: List[int]
        """
        if not matrix:
            return []
        l, r, t, b, res = 0, len(matrix[0])-1, 0, len(matrix)-1, []
        while True:
            for i in range(l, r + 1):
                res.append(matrix[t][i])
            t += 1
            if t > b:
                break
            for i in range(t, b + 1):
                res.append(matrix[i][r])
            r -= 1
            if l > r:
                break
            for i in range(r, l-1, -1):
                res.append(matrix[b][i])
            b -= 1
            if t > b:
                break
            for i in range(b, t-1, -1):
                res.append(matrix[i][l])
            l += 1
            if l > r:
                break
        return res
执行用时：24 ms, 在所有 Python 提交中击败了75.00%的用户
内存消耗：13.8 MB, 在所有 Python 提交中击败了54.61%的用户
通过测试用例：27 / 27
```

解题思路：
将矩阵第一行的元素添加到 res 列表里，删除第一行（也就是 matrix 中的第一个列表），然后逆时针旋转（这里通过转置+倒序实现），新的 matrix 的第一行即为接下来要打印的元素。

作者：xiao-ma-nong-25
链接：https://leetcode-cn.com/problems/shun-shi-zhen-da-yin-ju-zhen-lcof/solution/shan-chu-di-yi-xing-ni-shi-zhen-xuan-zhuan-python5/

```python
class Solution:
    def spiralOrder(self, matrix: List[List[int]]) -> List[int]:
        res = []
        while matrix:
            res += matrix.pop(0)
            matrix = list(zip(*matrix))[::-1]
        return res
执行用时：36 ms, 在所有 Python 提交中击败了19.90%的用户
内存消耗：13.9 MB, 在所有 Python 提交中击败了47.09%的用户
通过测试用例：27 / 27
```

#### [剑指 Offer 31. 栈的压入、弹出序列](https://leetcode-cn.com/problems/zhan-de-ya-ru-dan-chu-xu-lie-lcof/)

考虑借用一个辅助栈 stack ，模拟 压入 / 弹出操作的排列。根据是否模拟成功，即可得到结果。

入栈操作： 按照压栈序列的顺序执行。
出栈操作： 每次入栈后，循环判断 “栈顶元素 == 弹出序列的当前元素” 是否成立，将符合弹出序列顺序的栈顶元素全部弹出。
由于题目规定 栈的所有数字均不相等 ，因此在循环入栈中，每个元素出栈的位置的可能性是唯一的（若有重复数字，则具有多个可出栈的位置）。因而，在遇到 “栈顶元素 == 弹出序列的当前元素” 就应立即执行出栈。

算法流程：

1. 初始化： 辅助栈 stack ，弹出序列的索引 i ；
2. 遍历压栈序列： 各元素记为 num；
   * 元素 num 入栈；
   * 循环出栈：若 stack 的栈顶元素 == 弹出序列元素 popped[i]，则执行出栈与 i++ ；
3. 返回值： 若 stack为空，则此弹出序列合法。
   复杂度分析：
   * 时间复杂度 O(N) ： 其中 N 为列表 pushed的长度；每个元素最多入栈与出栈一次，即最多共 2N 次出入栈操作。
   * 空间复杂度 O(N) ： 辅助栈 stack 最多同时存储 N 个元素。

作者：jyd
链接：https://leetcode-cn.com/problems/zhan-de-ya-ru-dan-chu-xu-lie-lcof/solution/mian-shi-ti-31-zhan-de-ya-ru-dan-chu-xu-lie-mo-n-2/

```python
class Solution(object):
    def validateStackSequences(self, pushed, popped):
        """
        :type pushed: List[int]
        :type popped: List[int]
        :rtype: bool
        """
        stack, i = [], 0
        for num in pushed:
            stack.append(num)
            while stack and stack[-1] == popped[i]:
                stack.pop()
                i += 1
        return not stack
执行用时：16 ms, 在所有 Python 提交中击败了95.70%的用户
内存消耗：13.4 MB, 在所有 Python 提交中击败了10.13%的用户
通过测试用例：151 / 151
```

#### [剑指 Offer 20. 表示数值的字符串](https://leetcode-cn.com/problems/biao-shi-shu-zhi-de-zi-fu-chuan-lcof/)

解题思路：
本题使用有限状态自动机。根据字符类型和合法数值的特点，先定义状态，再画出状态转移图，最后编写代码即可。

字符类型：

空格 「 」、数字「 0—90—9 」 、正负号 「 +-+− 」 、小数点 「 .. 」 、幂符号 「 eEeE 」 。

状态定义：

按照字符串从左到右的顺序，定义以下 9 种状态。

1. 开始的空格
2. 幂符号前的正负号
3. 小数点前的数字
4. 小数点、小数点后的数字
5. 当小数点前为空格时，小数点、小数点后的数字
6. 幂符号
7. 幂符号后的正负号
8. 幂符号后的数字
9. 结尾的空格

结束状态：

合法的结束状态有 2, 3, 7, 8 。

![Picture1.png](https://pic.leetcode-cn.com/6f41d7e46fd0344c013980e3f46429dd7a7311bb4292783a482466a90f15747b-Picture1.png)

作者：jyd
链接：https://leetcode-cn.com/problems/biao-shi-shu-zhi-de-zi-fu-chuan-lcof/solution/mian-shi-ti-20-biao-shi-shu-zhi-de-zi-fu-chuan-y-2/

```python
class Solution(object):
    def isNumber(self, s):
        """
        :type s: str
        :rtype: bool
        """
        states = [
            {' ': 0, 's': 1, 'd': 2, '.': 4}, # 0. start with 'blank'开头空格 还是指向states[0]，上一位是 开头空格，下一位可以是 空格、符号、数字、前面没有数的小数点
            {'d': 2, '.': 4},                 # 1. 'sign' before 'e'上一位是符号，符号位后面可以是 数字、前面没有数的小数点
            {'d': 2, '.': 3, 'e': 5, ' ': 8}, # 2. 'digit' before 'dot'：上一位是数字，数字的下一位可以是 数字、前面有数的小数点、e、结尾空格
            {'d': 3, 'e': 5, ' ': 8},         # 3. 'digit' after 'dot'上一位是前面有数的小数点，下一位可以是 数字、e（8.e2 = 8e2，和2的情况一样）、结尾空格
            {'d': 3},                         # 4. 'digit' after 'dot' (‘blank’ before 'dot')上一位是前面没有数的小数点，下一位只能是 数字（符号肯定不行，e得前面有数才行）  
            {'s': 6, 'd': 7},                 # 5. 'e' 上一位是e，下一位可以是 符号、数字
            {'d': 7},                         # 6. 'sign' after 'e' 上一位是e后面的符号，下一位只能是 数字
            {'d': 7, ' ': 8},                 # 7. 'digit' after 'e' 上一位是e后面的数字，下一位可以是 数字、结尾空格
            {' ': 8}                          # 8. end with 'blank' 上一位是结尾空格，下一位只能是 结尾空格
        ]
        p = 0
        for c in s:
            if '0' <= c <= '9':
                t = 'd'
            elif c in "+-":
                t = 's'
            elif c in "Ee":
                t = 'e'
            elif c in ". ":
                t = c
            else:
                t = '?'
            if t not in states[p]:
                return False
            p = states[p][t]
        return p in (2, 3, 7, 8)
执行用时：32 ms, 在所有 Python 提交中击败了45.88%的用户
内存消耗：13 MB, 在所有 Python 提交中击败了82.74%的用户
通过测试用例：1480 / 1480
```

#### [剑指 Offer 67. 把字符串转换成整数](https://leetcode-cn.com/problems/ba-zi-fu-chuan-zhuan-huan-cheng-zheng-shu-lcof/)

暴力解题法

本题目涉及的变化不多，尝试暴力解题，列举所有可能的状态。考虑到题目有正负的要求，设立一个符号位s记录符号。

1. 在遇到第一个非空格符号前，我们设立一个l记录遍历到的字符位置，设立res记录遍历到的数字。开始遍历字符串，每移动一次l+1，根据l判断是否遍历完字符串，是直接返回0。直到遇到第一个非空字符
   * 它是+-之一，将其赋值给s，l+1，根据l判断是否遍历完字符串，是直接返回0（只有一个符号位在字符串内）；
   * 它是数字，将其添加到res中，l+1，根据l判断是否遍历完字符串，是直接返回该位数字（只有一个数字在字符串内），否跳出循环，进入下一个阶段；
   * 它不是以上两者，即第一个非空字符既不是+-也不是数字，直接返回0。
2. 若第一步没有返回，则证明遇到的第一个非空字符是数字或者+-，接下来就继续判断后面的字符是否是数字，是就添加到res，不是就结束遍历。
3. 此时res里面有两种情况
   * 第一个非空字符是+-，后面接着的字符不是数字，此时res为空，返回0
   * 第一个是数字，后面不论是什么res非空（在步骤1加入了第一个数字）
4. 若res非空，先判断符号位s是否是-，是则再判断大小是否超出范围，再将输出取负；否的话直接判断是否超出范围。

```Python
class Solution(object):
    def strToInt(self, str):
        """
        :type str: str
        :rtype: int
        """
        if len(str) == 0:
            return 0
        s = ''
        l = 0
        res = []
        for c in str:
            if c == ' ':
                l += 1
                if l >= len(str):
                    return 0
                continue
            elif c in "+-":
                s = c
                l += 1
                if l >= len(str):
                    return 0
                break
            elif '0' <= c <= '9':
                l += 1
                if l >= len(str):
                    return int(c)
                else:
                    res.append(c)
                    break
            else:
                return 0
        for i in range(l, len(str)):
            if '0' <= str[i] <= '9':
                res.append(str[i])
            else:
                break
        if len(res) == 0:
            return 0
        out = int(''.join(res))
        if s == '-':
            if out <= 2 ** 31:
                return -out
            else:
                return - 2 ** 31
        if out > 2 ** 31 -1:
            return 2 ** 31 -1
        return out
执行用时：28 ms, 在所有 Python 提交中击败了53.93%的用户
内存消耗：13.1 MB, 在所有 Python 提交中击败了67.01%的用户
通过测试用例：1079 / 1079
```

解题思路：
根据题意，有以下四种字符需要考虑：

首部空格： 删除之即可；
符号位： 三种情况，即 ''+'' , ''-'' , ''无符号" ；新建一个变量保存符号位，返回前判断正负即可。
非数字字符： 遇到首个非数字的字符时，应立即返回。
数字字符：
字符转数字： “此数字的 ASCII 码” 与 “ 0 的 ASCII 码” 相减即可；
数字拼接： 若从左向右遍历数字，设当前位字符为 c ，当前位数字为 x ，数字结果为 res ，则数字拼接公式为：

![image-20211129170002444](E:\MyGit\LeetCode\剑指offer.assets\image-20211129170002444.png)

![Picture1.png](https://pic.leetcode-cn.com/0be9098b13047fe3e07f3c4e51c612244ace01a023ed010bce43940408334f2a-Picture1.png)

数字越界处理：

题目要求返回的数值范围应在 \[$-2^{31}, 2^{31} - 1$\]，因此需要考虑数字越界问题。而由于题目指出 环境只能存储 32 位大小的有符号整数 ，因此判断数字越界时，要始终保持 res 在 int 类型的取值范围内。

在每轮数字拼接前，判断 res在此轮拼接后是否超过 2147483647 ，若超过则加上符号位直接返回。
设数字拼接边界 bndry = 2147483647 // 10 = 214748364 ，则以下两种情况越界：

![image-20211129170056471](E:\MyGit\LeetCode\剑指offer.assets\image-20211129170056471.png)

![Picture2.png](https://pic.leetcode-cn.com/d1b06a91801868af63f6e309da31bcfa01c7b6c385529fb974389a61e454cd12-Picture2.png)

作者：jyd
链接：https://leetcode-cn.com/problems/ba-zi-fu-chuan-zhuan-huan-cheng-zheng-shu-lcof/solution/mian-shi-ti-67-ba-zi-fu-chuan-zhuan-huan-cheng-z-4/

```python
class Solution(object):
    def strToInt(self, str):
        """
        :type str: str
        :rtype: int
        """
        str = str.strip()
        if not str:
            return 0
        res, i, sign = 0, 1, 1
        int_max, int_min, bndry = 2 ** 31 - 1, -2 **31, 2 ** 31 // 10
        if str[0] == '-':
            sign = -1
        elif str[0] != '+':
            i = 0
        for c in str[i:]:
            if not '0' <= c <= '9':
                break
            if res > bndry or res == bndry and c > '7':
                return int_max if sign == 1 else int_min
            res = 10 * res + ord(c) - ord('0')
        return sign * res
执行用时：20 ms, 在所有 Python 提交中击败了92.15%的用户
内存消耗：13 MB, 在所有 Python 提交中击败了87.96%的用户
通过测试用例：1079 / 1079
```

#### [剑指 Offer 59 - I. 滑动窗口的最大值](https://leetcode-cn.com/problems/hua-dong-chuang-kou-de-zui-da-zhi-lcof/)

暴力解题法：

直接按照顺序遍历，每次都取子数组里面的最大值。

```python
class Solution(object):
    def maxSlidingWindow(self, nums, k):
        """
        :type nums: List[int]
        :type k: int
        :rtype: List[int]
        """
        if not nums or k == 1:
            return nums
        out = []
        for i in range(len(nums) - k + 1):
            tmp = nums[i: i+k]
            out.append(max(tmp))
        return out
执行用时：308 ms, 在所有 Python 提交中击败了27.93%的用户
内存消耗：20 MB, 在所有 Python 提交中击败了71.07%的用户
通过测试用例：18 / 18
```

K神解题法：https://leetcode-cn.com/problems/hua-dong-chuang-kou-de-zui-da-zhi-lcof/solution/mian-shi-ti-59-i-hua-dong-chuang-kou-de-zui-da-1-6/

```Python
class Solution(object):
    def maxSlidingWindow(self, nums, k):
        """
        :type nums: List[int]
        :type k: int
        :rtype: List[int]
        """
        if not nums or k == 0: return []
        deque = collections.deque()
        # 未形成窗口
        for i in range(k):
            while deque and deque[-1] < nums[i]:
                deque.pop()
            deque.append(nums[i])
        res = [deque[0]]
        # 形成窗口后
        for i in range(k, len(nums)):
            if deque[0] == nums[i - k]:
                deque.popleft()
            while deque and deque[-1] < nums[i]:
                deque.pop()
            deque.append(nums[i])
            res.append(deque[0])
        return res
执行用时：36 ms, 在所有 Python 提交中击败了90.77%的用户
内存消耗：20.3 MB, 在所有 Python 提交中击败了34.41%的用户
通过测试用例：18 / 18
```

#### [剑指 Offer 59 - II. 队列的最大值](https://leetcode-cn.com/problems/dui-lie-de-zui-da-zhi-lcof/)

思路

本算法基于问题的一个重要性质：当一个元素进入队列的时候，它前面所有比它小的元素就不会再对答案产生影响。

举个例子，如果我们向队列中插入数字序列 1 1 1 1 2，那么在第一个数字 2 被插入后，数字 2 前面的所有数字 1 将不会对结果产生影响。因为按照队列的取出顺序，数字 2 只能在所有的数字 1 被取出之后才能被取出，因此如果数字 1 如果在队列中，那么数字 2 必然也在队列中，使得数字 1 对结果没有影响。

按照上面的思路，我们可以设计这样的方法：从队列尾部插入元素时，我们可以提前取出队列中所有比这个元素小的元素，使得队列中只保留对结果有影响的数字。这样的方法等价于要求维持队列单调递减，即要保证每个元素的前面都没有比它小的元素。

那么如何高效实现一个始终递减的队列呢？我们只需要在插入每一个元素 value 时，从队列尾部依次取出比当前元素 value 小的元素，直到遇到一个比当前元素大的元素 value' 即可。

上面的过程保证了只要在元素 value 被插入之前队列递减，那么在 value 被插入之后队列依然递减。
而队列的初始状态（空队列）符合单调递减的定义。
由数学归纳法可知队列将会始终保持单调递减。
上面的过程需要从队列尾部取出元素，因此需要使用双端队列来实现。另外我们也需要一个辅助队列来记录所有被插入的值，以确定 pop_front 函数的返回值。

保证了队列单调递减后，求最大值时只需要直接取双端队列中的第一项即可。

作者：LeetCode-Solution
链接：https://leetcode-cn.com/problems/dui-lie-de-zui-da-zhi-lcof/solution/mian-shi-ti-59-ii-dui-lie-de-zui-da-zhi-by-leetcod/

```python
class MaxQueue(object):

    def __init__(self):
        self.queue = collections.deque()
        self.deque = collections.deque()


    def max_value(self):
        """
        :rtype: int
        """
        return self.deque[0] if self.deque else -1


    def push_back(self, value):
        """
        :type value: int
        :rtype: None
        """
        self.queue.append(value)
        while self.deque and self.deque[-1] < value:
            del self.deque[-1]
        self.deque.append(value)


    def pop_front(self):
        """
        :rtype: int
        """
        if not self.queue: return -1
        self.queue[0] == self.deque[0] and self.deque.popleft()
        return self.queue.popleft()
执行用时：540 ms, 在所有 Python 提交中击败了93.29%的用户
内存消耗：17.5 MB, 在所有 Python 提交中击败了56.71%的用户
通过测试用例：34 / 34
```

