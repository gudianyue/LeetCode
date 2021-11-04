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

