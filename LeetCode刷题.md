# LeetCode刷题

## 双指针（对撞指针）

### [167. 两数之和 II - 输入有序数组](https://leetcode-cn.com/problems/two-sum-ii-input-array-is-sorted/)

**解法**：采用头尾指针

**解题思路**：由于题目所给的数组是有序数组，从第一个元素和最后一个元素的加和开始对比，如果大于目标值，则尾部指针（指向最后一个元素）向前移动一位；如果小于目标值，则头部指针（指向第一个元素）向后移动一位。当找到相等加和或者头尾指针相遇时停止循环。

**注意**：1.先判断数组是否为空；2.循环跳出（停止）的判断

```python
class Solution(object):
    def twoSum(self, numbers, target):
        """
        :type numbers: List[int]
        :type target: int
        :rtype: List[int]
        """
        if not numbers:
            return []
        l, r = 0, len(numbers)-1
        while l != r:
            if numbers[l] + numbers[r] == target:
                return [l+1, r+1]
            elif numbers[l] + numbers[r] > target:
                r = r - 1
            else:
                l = l + 1
        return []
```

### [633. 平方数之和](https://leetcode-cn.com/problems/sum-of-square-numbers/)

**解法**：采用头尾指针

**解题思路**：与上题类似，可以看做是从一个包含0到目标数开方数（向下取整）的升序数组中寻找平方和为目标值的两个数。有则返回True，否则返回False。

**注意**：此题中，由于可以是相同的两个数的平方为目标值，故头尾指针可以重叠（指向同一个数）

```python
class Solution(object):
    def judgeSquareSum(self, c):
        """
        :type c: int
        :rtype: bool
        """
        assert c >= 0
        start, end = 0, int(sqrt(c))
        while start <= end:
            num = start**2 + end**2
            if num == c:
                return True
            elif num > c:
                end = end - 1
            else:
                start = start + 1
        return False
```

**大佬解法**：费马平方和
**费马平方和** : 奇质数能表示为两个平方数之和的充分必要条件是该质数被 4 除余 1 ，因此不存在两个完全平方数之和模4为3。

**翻译过来就是**：当且仅当一个自然数的质因数分解中，满足 4k+3 形式的质数次方数均为偶数时，该自然数才能被表示为两个平方数之和。

因此我们对 c 进行质因数分解，再判断满足 4k+3 形式的质因子的次方数是否均为偶数即可。

```python
class Solution:
    def judgeSquareSum(self, c: int) -> bool:
        if not c:
            return True
        # (a - b) ^ 2 + (a + b) ^ 2 = 2 * (a ^ 2 + b ^ 2) = 2 * c
        while c % 2 == 0:
            c //= 2
        # 费马平方和定理
        if c % 4 == 3:
            return False
        sqrt = int(math.sqrt(c)) 
        for i in range(3, sqrt + 1, 4):  ##直接跳过那些不是4n+3的因子
            count = 0
            while c % i == 0: ## 计算因子次方数
                c //= i
                count += 1
            if count % 2 != 0:
                return False
        return True
```

### [345. 反转字符串中的元音字母](https://leetcode-cn.com/problems/reverse-vowels-of-a-string/)

**解法**：采用头尾指针

**解题思路**：同上

```python
class Solution(object):
    def reverseVowels(self, s):
        """
        :type s: str
        :rtype: str
        """
        s = list(s)
        vowel = set('aeiouAEIOU')
        l, r = 0, len(s)-1
        if r < 1:
            return ''.join(s)
        while l < r:
            if s[l] in vowel and s[r] in vowel:
                s[l], s[r] = s[r], s[l]
                l += 1
                r -= 1
            elif s[l] in vowel:
                r -= 1
            elif s[r] in vowel:
                l += 1
            else:
                l += 1
                r -= 1
        return ''.join(s)
###
Python join() 方法用于将序列中的元素以指定的字符连接生成一个新的字符串。
str.join(sequence)

str = "-"
seq = ("a", "b", "c") # 字符串序列
print str.join( seq )
a-b-c
###
###
set() 函数创建一个无序不重复元素集，可进行关系测试，删除重复数据，还可以计算交集、差集、并集等>>>x = set('runoob')
>>> y = set('google')
>>> x, y
(set(['b', 'r', 'u', 'o', 'n']), set(['e', 'o', 'g', 'l']))   # 重复的被删除
###
```

### [125. 验证回文串](https://leetcode-cn.com/problems/valid-palindrome/)

**解法**：首尾指针

**解题思路**：题目中忽略大小写，因此采用lower()，题目中忽略非字母数字字符，因此采用isalnum()。

**注意**：先忽略非字母数字字符，再忽略大小写进行比较。

```python
class Solution(object):
    def isPalindrome(self, s):
        """
        :type s: str
        :rtype: bool
        """
        if len(s) < 1: return True
        l, r = 0, len(s) - 1
        while l < r:
            if s[l].isalnum() and s[r].isalnum():
                if s[l].lower() == s[r].lower():
                    l += 1
                    r -= 1
                    continue
                else: return False
            elif s[l].isalnum(): r -= 1
            elif s[r].isalnum(): l += 1
            else:
                l += 1
                r -= 1
        return True
```

### [344. 反转字符串](https://leetcode-cn.com/problems/reverse-string/)

**解法**：首尾指针。

```python
class Solution(object):
    def reverseString(self, s):
        """
        :type s: List[str]
        :rtype: None Do not return anything, modify s in-place instead.
        """
        if len(s) < 2:
            return s
        l, r = 0, len(s)-1
        while l < r:
            s[l], s[r] = s[r], s[l]
            l += 1
            r -= 1
        return s
```

**大佬解法**：由于首尾对换，只用考虑半边就可以了。

```python
class Solution(object):
    def reverseString(self, s):
        """
        :type s: List[str]
        :rtype: None Do not return anything, modify s in-place instead.
        """
        for i in range(len(s)//2):
            s[i], s[- 1 - i] = s[- 1 - i], s[i]
```

### [11. 盛最多水的容器](https://leetcode-cn.com/problems/container-with-most-water/)

**解法**：首尾指针。

**解题思路**：首先初始化最大面积为首尾两个数中最小值与两数距离相乘。接下来执行如下操作：两数中小的那一个进行移位（如果是左边小就令左边的指针向右移动，右边小就令右边指针向左移动），只有这样才能保证可能有比之前更大的值出现。不断循环直到左右指针相遇。

```python
class Solution(object):
    def maxArea(self, height):
        """
        :type height: List[int]
        :rtype: int
        """
        l, r = 0, len(height)-1
        target = 0
        while l<r:
            target = max(min(height[l], height[r]) * (r-l), target)
            if height[l] <= height[r]:
                l += 1
            else:
                r -= 1
        return target
```

## 双指针（快慢指针）

### [141. 环形链表](https://leetcode-cn.com/problems/linked-list-cycle/)

**解法**：快慢指针

**解题思路**：首先判断列表是否长度大于等于2，然后设置前后两个指针，前面慢指针每次移动一位，后面快指针每次移动两位。若有循环，则快慢指针终会相遇；若没有循环，快指针会先一步走到列表尾部，可根据快指针的状态停止程序。

```python
# Definition for singly-linked list.
# class ListNode(object):
#     def __init__(self, x):
#         self.val = x
#         self.next = None

class Solution(object):
    def hasCycle(self, head):
        """
        :type head: ListNode
        :rtype: bool
        """
        if not head or head.next == None:
            return False
        slow, fast = head, head.next
        while slow and fast:
            if slow == fast:
                return True
            slow = slow.next
            if fast.next:
                fast = fast.next.next
            else:
                return False
        return False
```

### [283. 移动零](https://leetcode-cn.com/problems/move-zeroes/)

**解法**：快慢指针

**解题思路**：首先判断是否是空或者只有一位的列表。然后在开始两个元素进行比较，当前面的元素是非零时两个指针均向后移动一步。当前面的指针指向0时，若后面的指针指向非零则交换二者，然后前后指针均向后移动一步，否则后面的指针向后移动一步直至找到非零元素或者到达列表结尾。当后面的指针到达列表结尾结束，返回原列表。

```python
class Solution(object):
    def moveZeroes(self, nums):
        """
        :type nums: List[int]
        :rtype: None Do not return anything, modify nums in-place instead.
        """
        if len(nums) < 2:
            return nums
        slow, fast = 0, 1
        while fast < len(nums):
            if nums[slow] == 0 and nums[fast] != 0:
                nums[slow], nums[fast] = nums[fast], nums[slow]
                slow += 1
                fast += 1
            elif nums[slow] == 0:
                fast += 1
            else:
                fast += 1
                slow += 1
        return nums
```

### [26. 删除有序数组中的重复项](https://leetcode-cn.com/problems/remove-duplicates-from-sorted-array/)

**解法**：快慢指针

```python
class Solution:
    def removeDuplicates(self, nums: List[int]) -> int:
        if not  nums: return 0
        # 慢指针指向待写入元素的位置，快指针遍历数组
        slower, faster = 0, 0
        while faster< len(nums):
        #当快指针指向的元素与慢指针不同时，说明相同的元素已经遍历结束，此时将慢指针后移，将快指针的元素写入慢指针位置，保留一个元素
            if nums[slower] != nums[faster]:
                slower += 1
                nums[slower] = nums[faster]
            faster += 1
        return slower + 1
```

或者（不用快慢指针， 慢，内存大）

```python
class Solution(object):
    def removeDuplicates(self, nums):
        """
        :type nums: List[int]
        :rtype: int
        """
        l = len(nums)
        if l == 0:
            return 0
        num = nums[0]
        count = 1
        for i in range(1, l):
            if nums[i] != num:
                nums[count] = nums[i]
                num = nums[i]
                count += 1
        return count
```

### [80. 删除有序数组中的重复项 II](https://leetcode-cn.com/problems/remove-duplicates-from-sorted-array-ii/)

**解法**：快慢指针

**解题思路**：设置一个标志表示同样数字是否已经出现过，如果是，就跳过；如果不是，则保留一次。

```python
class Solution(object):
    def removeDuplicates(self, nums):
        """
        :type nums: List[int]
        :rtype: int
        """
        if not nums:
            return 0
        flag = True
        s, f = 0, 1
        while f < len(nums):
            if nums[s] != nums[f]:
                s += 1
                nums[s] = nums[f]
                flag = True
            else:
                if flag:
                    s += 1
                    nums[s] = nums[f]
                    flag = False
            f += 1
        return s+1
```

## 其他双指针

### [524. 通过删除字母匹配到字典里最长单词](https://leetcode-cn.com/problems/longest-word-in-dictionary-through-deleting/)

**解法**：双指针

**解题思路**：在s和dictionary分别设置指针，先找出第一个符合条件的单词，当出现第二个符合条件的单词时，与第一个进行比较。

```python
class Solution(object):
    def findLongestWord(self, s, dictionary):
        """
        :type s: str
        :type dictionary: List[str]
        :rtype: str
        """
        if  not s or not dictionary:
            return ''
        l = ''
        for d in dictionary:
            p = 0
            for p1 in range(len(s)):
                if p < len(d) and s[p1] == d[p]:
                    p += 1
                    continue
            if p == len(d) and not l:
                l = d
            elif p == len(d):
                if len(d) > len(l):
                    l = d
                elif len(d) < len(l):
                    continue
                else:
                    if l < d:
                        continue
                    else:
                        l = d
        return l
```

### [88. 合并两个有序数组](https://leetcode-cn.com/problems/merge-sorted-array/)

**解法**：三指针

**解题思路**：设置三个指针，分别指向nums1，nums2和最后数组的尾部。从后往前比较。分别插入最大的数。最后有两种情况：1.nums1多余，直接返回；2.nums2多余，将nums1前面的数字覆盖为nums2剩下的数字。

```python
class Solution(object):
    def merge(self, nums1, m, nums2, n):
        """
        :type nums1: List[int]
        :type m: int
        :type nums2: List[int]
        :type n: int
        :rtype: None Do not return anything, modify nums1 in-place instead.
        """
        s1, s2, end = m-1, n-1, m+n-1
        while s1>=0 and s2 >=0:
            if nums1[s1] < nums2[s2]:
                nums1[end] = nums2[s2]
                end -= 1
                s2 -= 1
            else:
                nums1[end] = nums1[s1]
                s1 -= 1
                end -= 1
        if s2 >= 0:
            nums1[0:s2+1] = nums2[0:s2+1]
```

## 哈希表问题

知乎原文链接：https://zhuanlan.zhihu.com/p/95156642

**定义**：散列表（Hash table，也叫哈希表），是根据键（Key）而直接访问在内存存储位置的数据结构。也就是说，它通过计算一个关于键值的函数，将所需查询的数据映射到表中一个位置来访问记录，这加快了查找速度。这个映射函数称做散列函数，存放记录的数组称做散列表。

**特点**：

1、哈希表其实也叫散列表，两个是一个玩意，英文是Hash Table；

2、哈希表是一个数据结构。哈希表其实本质上就是一个数组 。

**实现方法**：

实现哈希表我们可以采用两种方法：

1、数组+链表

2、数组+二叉树

**表的使用**：哈希表就是通过将关键值也就是key通过一个散列函数加工处理之后得到一个值，这个值就是数据存放的位置，我们就可以根据这个值快速的找到我们想要的数据。

![img](https://pic1.zhimg.com/80/v2-1fca2ca6fd75ee042a0d0c813f17ea24_720w.jpg)

这个哈希函数就是我们之前说的散列函数（散列表和哈希表的关系，仅仅是称呼的不同）。对于哈希表，它经常存放的是一些键值对的数据，简单点说就是一个值对应另外一个值，比如a对应b，那么a就是key，b是value，哈希表存放的就是这样的键值对，在哈希表中是通过哈希函数将一个值映射到另外一个值的，所以在哈希表中，a映射到b，a就叫做键值，而b呢？就叫做a的哈希值，也就是hash值。

**存放数据**：这里的学号是个key，我们之前也知道了，哈希表就是根据key值来通过哈希函数计算得到一个值，这个值就是用来确定这个Entry要存放在哈希表中的位置的，实际上这个值就是一个下标值，来确定放在数组的哪个位置上。

比如这里的学号是101011，那么经过哈希函数的计算之后得到了1，这个1就是告诉我们应该把这个Entry放到哪个位置，这个1就是数组的确切位置的下标，也就是需要放在数组中下表为1的位置，如图中所示。

数组中1的位置存放的是一个Entry，它不是一个简单的单个数值，而是一个键值对，也就是存放了key和value，key就是学号101011，value就是张三，我们经过哈希函数计算得出的1只是为了确定这个Entry该放在哪个位置而已。

![img](https://pic4.zhimg.com/80/v2-406307009ebdcca42a3b7481780d8bef_720w.jpg)

**哈希冲突解决**：一个是开放寻址法，一个是拉链法。

![img](https://pic2.zhimg.com/80/v2-8976728104f82a492592bcf330f984d5_720w.jpg)

既然位置被占了，那就另外再找个位置不就得了，怎么找其他的位置呢？这里其实也有很多的实现，我们说个最基本的就是既然当前位置被占用了，我们就看看该位置的后一个位置是否可用，也就是1的位置被占用了，我们就看看2的位置，如果没有被占用，那就放到这里呗，当然，也有可能2的位置也被占用了，那咱就继续往下找，看看3的位置，一次类推，直到找到空位置。

![img](https://pic3.zhimg.com/80/v2-2abff58382590a9d9f0578e918b44a2a_720w.jpg)

现在张三和李四都要放在1找个位置上，但是张三先来的，已经占了这个位置，待在了这个位置上了，那李四呢？解决办法就是链表，这时候这个1的位置存放的不单单是之前的那个Entry了，此时的Entry还额外的保存了一个next指针，这个指针指向数组外的另外一个位置，将李四安排在这里，然后张三那个Entry中的next指针就指向李四的这个位置，也就是保存的这个位置的内存地址，如果还有冲突，那就把又冲突的那个Entry放在一个新位置上，然后李四的Entry中的next指向它，这样就形成了一个链表。

如果冲突过多的话，这块的链表会变得比较长，怎么处理呢？这里举个例子吧，拿java集合类中的HashMap来说吧，如果这里的链表长度大于等于8的话，链表就会转换成树结构，当然如果长度小于等于6的话，就会还原链表。以此来解决链表过长导致的性能问题。中间有个7作为一个差值，来避免频繁的进行树和链表的转换，因为转换频繁也是影响性能的。

**哈希表的扩容**：

这里一般会有一个增长因子的概念，也叫作负载因子，简单点说就是已经被占的位置与总位置的一个百分比，比如一共十个位置，现在已经占了七个位置，就触发了扩容机制，因为它的增长因子是0.7，也就是达到了总位置的百分之七十就需要扩容。还拿HashMap来说，当它当前的容量占总容量的百分之七十五的时候就需要扩容了。而且这个扩容也不是简单的把数组扩大，而是新创建一个数组是原来的2倍，然后把原数组的所有Entry都重新Hash一遍放到新的数组。

因为数组扩大了，所以一般哈希函数也会有变化，这里的Hash也就是把之前的数据通过新的哈希函数计算出新的位置来存放。

**哈希表数据读取**：

![img](https://pic2.zhimg.com/80/v2-213d79309410e8add662c861f45fe5f5_720w.jpg)

比如我们现在要通过学号102011来查找学生的姓名，怎么操作呢？我们首先通过学号利用哈希函数得出位置1，然后我们就去位置1拿数据啊，拿到这个Entry之后我们得看看这个Entry的key是不是我们的学号102011，一看是101011，什么鬼，一边去，这不是我们要的key啊，然后根据这个Entry的next知道下一给位置，在比较key，好成功找到李四。

开放寻址也是这个思路，先确定到这个位置，然后再看这个位置上的key是不是我们要的，如过不是那就看看下一个位置的。

**核心**：

在哈希表中，哈希函数的设计很重要，一个好的哈希函数可以极大的提升性能，而且如果你的哈希函数设计的比较简单粗陋，那很容易被那些不怀好意的人捣乱，比如知道了你哈希函数的规则，故意制造容易冲突的key值，那就有意思了，你的哈希表就会一直撞啊，一直撞啊。

### [1. 两数之和](https://leetcode-cn.com/problems/two-sum/)

**解法**：哈希表

**解题思路**：由于返回的是下标且题目中对于下标返回的顺序没有要求，因此可以建立字典形式的哈希表。

```Python
class Solution(object):
    def twoSum(self, nums, target):
        """
        :type nums: List[int]
        :type target: int
        :rtype: List[int]
        """
        if not nums:
            return []
        hash_map = {}
        for i in range(len(nums)):
            t = target - nums[i]
            if t in hash_map:
                return [i, hash_map[t]]
            else:
                hash_map[nums[i]] = i
```

### [217. 存在重复元素](https://leetcode-cn.com/problems/contains-duplicate/)

**暴力法**：set()函数获取不重复元素集，比较前后长度。

```python
class Solution(object):
    def containsDuplicate(self, nums):
        """
        :type nums: List[int]
        :rtype: bool
        """
        if len(nums) < 2:
            return False
        l = set(nums)
        if len((l)) == len(nums):
            return False
        else:
            return True
执行用时：20 ms, 在所有 Python 提交中击败了94.48%的用户
内存消耗：17.2 MB, 在所有 Python 提交中击败了59.04%的用户
```

**哈希表法**

```Python
class Solution(object):
    def containsDuplicate(self, nums):
        """
        :type nums: List[int]
        :rtype: bool
        """
        if len(nums) < 2:
            return False
        hash_map = set()
        for num in nums:
            if num not in hash_map:
                hash_map.add(num)
            else:
                return True
        return False
执行用时：24 ms, 在所有 Python 提交中击败了82.43%的用户
内存消耗：17.2 MB, 在所有 Python 提交中击败了57.16%的用户
```

add() 方法用于给集合添加元素，如果添加的元素在集合中已存在，则不执行任何操作。

### [594. 最长和谐子序列](https://leetcode-cn.com/problems/longest-harmonious-subsequence/)

**哈希表法**：建立数字-次数的哈希表，比较每个数字在数组中是否存在比它大1的数字，存在则把次数加起来，比较得到最大的组合。

```Python
class Solution(object):
    def findLHS(self, nums):
        """
        :type nums: List[int]
        :rtype: int
        """
        if len(nums) < 2:
            return 0
        hash_map = {}
        for num in nums:
            hash_map[num] = hash_map.get(num, 0) + 1
        res = 0
        for k, v in hash_map.items():
            if hash_map.get(k+1, 0) != 0:
                res = max(res, hash_map.get(k+1)+v)
        return res
Python 字典(Dictionary) items() 函数以列表返回可遍历的(键, 值) 元组数组。
执行用时：264 ms, 在所有 Python 提交中击败了30.93%的用户
内存消耗：15.7 MB, 在所有 Python 提交中击败了6.19%的用户
```

### [128. 最长连续序列](https://leetcode-cn.com/problems/longest-consecutive-sequence/)

**哈希表法**：建立无重复数字的哈希表，然后确定连续序列开头的数字。遍历哈希表数字（避免重复的数字浪费时间），如果比它小1的数字存在于哈希表中，则该数字不能作为连续序列开头。在确定开头后，计算哈希表中连续序列的长度。

```python
class Solution(object):
    def longestConsecutive(self, nums):
        """
        :type nums: List[int]
        :rtype: int
        """
        if not nums:
            return 0
        hash_map = set(nums)
        max_len = 0
        for i in hash_map:
            if i-1 not in hash_map:
                leng = 1
                cur = i
                while cur+1 in hash_map:
                    leng += 1
                    cur += 1
                max_len = max(max_len, leng)
        return max_len
```

### [349. 两个数组的交集](https://leetcode-cn.com/problems/intersection-of-two-arrays/)

**哈希表**：先保存两个没有重复元素的列表（set()函数），再比较两个列表中相同的元素。

```python
class Solution(object):
    def intersection(self, nums1, nums2):
        """
        :type nums1: List[int]
        :type nums2: List[int]
        :rtype: List[int]
        """
        if not nums1 or not nums2: return []
        res = []
        hashmap1 = set(nums1)
        hashmap2 = set(nums2)
        for num in hashmap1:
            if num in hashmap2:
                res.append(num)
        return res
```

### [350. 两个数组的交集 II](https://leetcode-cn.com/problems/intersection-of-two-arrays-ii/)

**哈希表**：建立两个哈希表，保存两个数组的数值和出现的次数。比较相同的数值和它在两个数组中出现的次数，少的次数的就是数值在输出中出现的次数。

```python
class Solution(object):
    def intersect(self, nums1, nums2):
        """
        :type nums1: List[int]
        :type nums2: List[int]
        :rtype: List[int]
        """
        if not nums1 or not nums2:
            return []
        hash_map1 = {}
        hash_map2 = {}
        out = []
        for num in nums1:
            hash_map1[num] = hash_map1.get(num, 0) + 1
        for num in nums2:
            hash_map2[num] = hash_map2.get(num, 0) + 1
        for k, v in hash_map1.items():
            if k in hash_map2.keys():
                num = min(hash_map1[k], hash_map2[k])
                for i in range(num):
                    out.append(k)
        return out
执行用时: 28 ms
内存消耗: 13 MB
```

**优化**：将较短的数组建立哈希表，在较长的数组中寻找重复的元素。

```python
class Solution(object):
    def intersect(self, nums1, nums2):
        """
        :type nums1: List[int]
        :type nums2: List[int]
        :rtype: List[int]
        """
        if not nums1 or not nums2:
            return []
        if len(nums1) > len(nums2):
            return self.intersect(nums2, nums1)
        hash_map = {}
        out = []
        for num in nums1:
            hash_map[num] = hash_map.get(num, 0) + 1
        for num in nums2:
            if hash_map.get(num) and hash_map[num] > 0:
                out.append(num)
                hash_map[num] -= 1
        return out
执行用时: 24 ms
内存消耗: 13 MB
```

**继续优化**：双指针，先排序再比较相同的元素。

```python
class Solution(object):
    def intersect(self, nums1, nums2):
        """
        :type nums1: List[int]
        :type nums2: List[int]
        :rtype: List[int]
        """
        nums1.sort()
        nums2.sort()

        length1, length2 = len(nums1), len(nums2)
        intersection = list()
        index1 = index2 = 0
        while index1 < length1 and index2 < length2:
            if nums1[index1] < nums2[index2]:
                index1 += 1
            elif nums1[index1] > nums2[index2]:
                index2 += 1
            else:
                intersection.append(nums1[index1])
                index1 += 1
                index2 += 1

        return intersection
执行用时：16 ms, 在所有 Python 提交中击败了94.97%的用户
内存消耗：12.9 MB, 在所有 Python 提交中击败了95.37%的用户
```

### [242. 有效的字母异位词](https://leetcode-cn.com/problems/valid-anagram/)

**哈希表**：为一个字符串建立字符与出现次数的哈希表，在遍历另一个字符串时，如果是哈希表中的字符，令其次数-1；若次数已经为0或者不是哈希表中的字符，返回FALSE。否则遍历完后返回TRUE。

```Python
class Solution(object):
    def isAnagram(self, s, t):
        """
        :type s: str
        :type t: str
        :rtype: bool
        """
        if len(s) != len(t):
            return False
        hash_map = {}
        for char in s:
            hash_map[char] = hash_map.get(char, 0) + 1
        for char in t:
            if not hash_map.get(char) or hash_map[char] == 0:
                return False
            else:
                hash_map[char] -= 1
        return True
```

### [202. 快乐数](https://leetcode-cn.com/problems/happy-number/)

**哈希表**：利用哈希表存储访问过的值，判断是否进行循环。

**知识点**：sum() 方法对**序列**进行求和计算。add() 方法用于给集合添加元素，如果添加的元素在集合中已存在，则不执行任何操作。

```Python
sum(iterable[, start])
参数
iterable -- 可迭代对象，如：列表、元组、集合。
start -- 指定相加的参数，如果没有设置这个值，默认为0。
返回值
返回计算结果。

set.add(elmnt)
参数
elmnt -- 必需，要添加的元素。
```

```Python
class Solution(object):
    def isHappy(self, n):
        """
        :type n: int
        :rtype: bool
        """
        if n == 1:
            return True
        hash_map = set()
        while True:
            if n == 1:
                return True
            elif n in hash_map:
                return False
            hash_map.add(n)
            n = sum([int(num)**2 for num in str(n)])
```

**快慢指针**：先定义一个函数返回下一个数字的数值，那么就可以隐性的建立一个链表。题目就变成判断链表是否出现循环。使用快慢指针，如果没有循环则快指针一定可以先一步到达1，否则快慢指针一定会相遇。

```Python
class Solution(object):
    def isHappy(self, n):
        """
        :type n: int
        :rtype: bool
        """
        def nxt(n):
            return sum([int(c)**2 for c in str(n)])
        slow, fast = n, nxt(n)
        while slow != fast and fast != 1:
            slow = nxt(slow)
            fast = nxt(nxt(fast))
        return fast==1
```

### [205. 同构字符串](https://leetcode-cn.com/problems/isomorphic-strings/)

**哈希表**：利用哈希表建立一一对应关系，当某一位对应不上时，判断为False。

```Python
class Solution(object):
    def isIsomorphic(self, s, t):
        """
        :type s: str
        :type t: str
        :rtype: bool
        """
        hash_map1 = {}
        hash_map2 = {}
        for i in range(len(s)):
            if (s[i] in hash_map1 and hash_map1[s[i]] != t[i]) or (t[i] in hash_map2 and hash_map2[t[i]] != s[i]):
                return False
            hash_map1[s[i]] = t[i]
            hash_map2[t[i]] = s[i]
        return True
```

**zip**：利用zip()函数将两个字符串对应位置的字母打包到一起（题目给出的字符串长度是一致的），用set()函数去掉重复的对，再用zip(*)函数解开为两个字符串。那么，如果两个字符串对应位置的字符是一一对应的，解开后的两个字符串中都不会出现重复字符；如果不是，肯定有一个字符串中有重复字符出现（一对多）。

```python
zip() 函数用于将可迭代的对象作为参数，将对象中对应的元素打包成一个个元组，然后返回由这些元组组成的对象，这样做的好处是节约了不少的内存。
我们可以使用 list() 转换来输出列表。
如果各个迭代器的元素个数不一致，则返回列表长度与最短的对象相同，利用 * 号操作符，可以将元组解压为列表。

zip 语法：
zip([iterable, ...])
参数说明：
iterabl -- 一个或多个迭代器;

>>>a = [1,2,3]
>>> b = [4,5,6]
>>> c = [4,5,6,7,8]
>>> zipped = zip(a,b)     # 返回一个对象
>>> zipped
<zip object at 0x103abc288>
>>> list(zipped)  # list() 转换为列表
[(1, 4), (2, 5), (3, 6)]
>>> list(zip(a,c))              # 元素个数与最短的列表一致
[(1, 4), (2, 5), (3, 6)]
>>> a1, a2 = zip(*zip(a,b))          # 与 zip 相反，zip(*) 可理解为解压，返回二维矩阵式
>>> list(a1)
[1, 2, 3]
>>> list(a2)
[4, 5, 6]
```

```Python
class Solution(object):
    def isIsomorphic(self, s, t):
        """
        :type s: str
        :type t: str
        :rtype: bool
        """
        l = zip(*set(zip(s, t)))
        for i in l:
            if len(i) != len(set(i)):
                return False
        return True
```

### [451. 根据字符出现频率排序](https://leetcode-cn.com/problems/sort-characters-by-frequency/)

**哈希表**：先建立哈希表，再对出现次数进行排序。

```python
sorted() 函数对所有可迭代的对象进行排序操作。
sort 与 sorted 区别：
sort 是应用在 list 上的方法，sorted 可以对所有可迭代的对象进行排序操作。
list 的 sort 方法返回的是对已经存在的列表进行操作，而内建函数 sorted 方法返回的是一个新的 list，而不是在原来的基础上进行的操作。

sorted 语法：
sorted(iterable, key=None, reverse=False)  
参数说明：
iterable -- 可迭代对象。
key -- 主要是用来进行比较的元素，只有一个参数，具体的函数的参数就是取自于可迭代对象中，指定可迭代对象中的一个元素来进行排序。
reverse -- 排序规则，reverse = True 降序 ， reverse = False 升序（默认）。

>>>sorted([5, 2, 3, 1, 4])
[1, 2, 3, 4, 5]                      # 默认为升序

先按照成绩降序排序，相同成绩的按照名字升序排序：
d1 = [{'name':'alice', 'score':38}, {'name':'bob', 'score':18}, {'name':'darl', 'score':28}, {'name':'christ', 'score':28}]
l = sorted(d1, key=lambda x:(-x['score'], x['name']))

print(l)

[{'name': 'alice', 'score': 38}, {'name': 'christ', 'score': 28}, {'name': 'darl', 'score': 28}, {'name': 'bob', 'score': 18}]
```

```Python
class Solution(object):
    def frequencySort(self, s):
        """
        :type s: str
        :rtype: str
        """
        if not s:
            return ''
        hash_map = {}
        for char in s:
            hash_map[char] = hash_map.get(char, 0) + 1
        t = sorted(hash_map.items(), key = lambda kv:(kv[1], kv[0]), reverse = True)
        out = []
        for k, v in t:
            for i in range(v):
                out.append(k)
        return ''.join(out)
```

**使用collections.Counter**

```python
from collections import Counter
class Solution(object):
    def frequencySort(self, s):
        """
        :type s: str
        :rtype: str
        """
        c = Counter(s)
        a = sorted(c.keys(), key = lambda x: -c[x])
        result = ""
        for i in a:
            result += i*c[i]
        return result
```

### [15. 三数之和](https://leetcode-cn.com/problems/3sum/)

**哈希表**：在将数组排序之后，遍历数组时，题目变成了在剩下的数组中寻找加和为当前元素取负的两数之和。

```Python
class Solution(object):
    def threeSum(self, nums):
        """
        :type nums: List[int]
        :rtype: List[List[int]]
        """
        if len(nums) < 3:
            return []
        res = set()
        nums.sort()
        if nums[0] > 0 or nums[-1] < 0: ## 由于数组已经排序，没有小于0的数或者最大的数小于0都不可能出现满足题目要求的组合。
            return []
        for i in range(len(nums)-2):
            if i > 0 and nums[i-1] == nums[i]: ## 由于输出的结果不能重复，对于相同的数字，前面的数字在遍历后面的数组时已经所有可能组合都遍历了，所以如果出现了重复的数字，直接跳过
                continue
            if nums[i] > 0:
                break
            target = -nums[i]
            hash_map = set()
            for j in range(i+1, len(nums)):
                t = target - nums[j]
                if t not in hash_map:
                    hash_map.add(nums[j])
                    continue
                triplet = [nums[i], t, nums[j]]
                triplet.sort()
                if tuple(triplet) not in res:
                    res.add(tuple(triplet))
        return [list(num) for num in res]
执行用时: 452 ms
内存消耗: 21.1 MB
```

**双指针**：后面寻找两数之和的方法可以使用双指针。

```Python
class Solution(object):
    def threeSum(self, nums):
        """
        :type nums: List[int]
        :rtype: List[List[int]]
        """
        if len(nums) < 3:
            return []
        res = set()
        nums.sort()
        if nums[0] > 0 or nums[-1] < 0:
            return []
        for i in range(len(nums)-2):
            if i > 0 and nums[i-1] == nums[i]:
                continue
            if nums[i] > 0:
                break
            target = -nums[i]
            start, end = i+1, len(nums)-1
            while start < end:
                if nums[start] + nums[end] < target:
                    start += 1
                elif nums[start] + nums[end] > target:
                    end -= 1
                else:
                    triple = [nums[i], nums[start], nums[end]]
                    triple.sort()
                    if tuple(triple) not in res:
                        res.add(tuple(triple))
                    start += 1
                    end -= 1
        return [list(num) for num in res]
执行用时：276 ms, 在所有 Python 提交中击败了99.46%的用户
内存消耗：20.7 MB, 在所有 Python 提交中击败了5.02%的用户
```

### [454. 四数相加 II](https://leetcode-cn.com/problems/4sum-ii/)

**哈希表**：利用哈希表将两个数组组合成一个表，将O(n4)复杂度降到O(n2)。

```Python
class Solution(object):
    def fourSumCount(self, nums1, nums2, nums3, nums4):
        """
        :type nums1: List[int]
        :type nums2: List[int]
        :type nums3: List[int]
        :type nums4: List[int]
        :rtype: int
        """
        hash_map1 = {}
        hash_map2 = {}
        out = 0
        for A in nums1:
            for B in nums2:
                hash_map1[A+B] = hash_map1.get(A+B, 0) + 1
        for C in nums3:
            for D in nums4:
                hash_map2[C+D] = hash_map2.get(C+D, 0) + 1
        for k, v in hash_map1.items():
            out += hash_map2.get(0-k, 0) * v
        return out
执行用时：500 ms, 在所有 Python 提交中击败了85.35%的用户
内存消耗：13.5 MB, 在所有 Python 提交中击败了18.54%的用户
```

**优化**：由于是寻找所有可能的组合，即下标不同但是数字组合相同也算一次，所以在第二遍遍历两个数组时直接比较。

```Python
class Solution(object):
    def fourSumCount(self, nums1, nums2, nums3, nums4):
        """
        :type nums1: List[int]
        :type nums2: List[int]
        :type nums3: List[int]
        :type nums4: List[int]
        :rtype: int
        """
        hash_map = {}
        out = 0
        for A in nums1:
            for B in nums2:
                hash_map[A+B] = hash_map.get(A+B, 0) + 1
        for C in nums3:
            for D in nums4:
                S = -C - D 
                if S in hash_map:
                    out += hash_map[S]
        return out
执行用时：408 ms, 在所有 Python 提交中击败了93.53%的用户
内存消耗：13.2 MB, 在所有 Python 提交中击败了87.50%的用户
```

### [49. 字母异位词分组](https://leetcode-cn.com/problems/group-anagrams/)

**哈希表**：利用哈希表建立一个字符串中所有字符及其出现的次数，然后不断对比。

```Python
class Solution(object):
    def groupAnagrams(self, strs):
        """
        :type strs: List[str]
        :rtype: List[List[str]]
        """
        if len(strs) < 2:
            return [strs]
        from collections import defaultdict
        hash_map = defaultdict(list)
        hash_map1 = {}
        for i in range(len(strs)):
            for j in range(ord('a'), ord('z') + 1):
                hash_map1[chr(j)] = 0
            for k in range(len(strs[i])):
                hash_map1[strs[i][k]] += 1
            hash_map[tuple(hash_map1.values())].append(strs[i])
        return list(hash_map.values())
执行用时：96 ms, 在所有 Python 提交中击败了18.22%的用户
内存消耗：19.4 MB, 在所有 Python 提交中击败了5.03%的用户
```

**官方**

```python
class Solution(object):
    def groupAnagrams(self, strs):
        """
        :type strs: List[str]
        :rtype: List[List[str]]
        """
        mp = collections.defaultdict(list)

        for st in strs:
            key = "".join(sorted(st))
            mp[key].append(st)

        return list(mp.values())
执行用时：48 ms, 在所有 Python 提交中击败了78.74%的用户
内存消耗：17 MB, 在所有 Python 提交中击败了57.95%的用户
```

**大佬解法**：https://leetcode-cn.com/problems/group-anagrams/solution/python3-99-by-meng-zhi-hen-n/

```Python
class Solution(object):
    def groupAnagrams(self, strs):
        """
        :type strs: List[str]
        :rtype: List[List[str]]
        """
        hash_map = {}
        for item in strs:
            key = tuple(sorted(item))
            hash_map[key] = hash_map.get(key, []) + [item]
        return list(hash_map.values())
执行用时：36 ms, 在所有 Python 提交中击败了99.07%的用户
内存消耗：17.4 MB, 在所有 Python 提交中击败了32.71%的用户
```

### [447. 回旋镖的数量](https://leetcode-cn.com/problems/number-of-boomerangs/)

**哈希表**：利用哈希表保存每两个点的距离，然后判断相同距离的对数是否大于2。

```python
class Solution(object):
    def numberOfBoomerangs(self, points):
        """
        :type points: List[List[int]]
        :rtype: int
        """
        if not points or len(points) < 3:
            return 0
        out = 0
        for i in range(len(points)):
            hash_map = {}
            for j in range(len(points)):## 不同的点不需要考虑有与自身一样的点，所以可以和自己计算距离
                diction = (points[i][0] - points[j][0])**2 + (points[i][1] - points[j][1])**2
                hash_map[diction] = hash_map.get(diction, 0) + 1
            for v in hash_map.values():## 相同距离的对数两两组合，数量是v*(v-1),因为要考虑顺序
                if v > 1:
                    out += v * (v-1)
        return out
执行用时：484 ms, 在所有 Python 提交中击败了95.45%的用户
内存消耗：13.1 MB, 在所有 Python 提交中击败了72.73%的用户
```

### [149. 直线上最多的点数](https://leetcode-cn.com/problems/max-points-on-a-line/)

**哈希表**：利用斜率来判定点共线（解题代码来自leetcode题解区powcai大佬，如果直接用除法计算容易出现截断从而出现错误。用公约数更好，如果直接调用math库里的gcd，这个库里的gcd只能计算出正值，如果math.gcd(3,-6)得到3，因此大佬重新写了gcd，可以参考）https://leetcode-cn.com/problems/max-points-on-a-line/comments/122673

```python
class Solution(object):
    def maxPoints(self, points):
        """
        :type points: List[List[int]]
        :rtype: int
        """
        from collections import Counter, defaultdict
        point_dict = Counter(tuple(point) for point in points)
        not_repeat_points = list(point_dict.keys())
        n = len(not_repeat_points)
        if n == 1:
            return point_dict[not_repeat_points[0]]
        out = 0
        def gcd(x, y):
            if y == 0:
                return x
            else:
                return gcd(y, x % y)
        for i in range(n-1):
            x1, y1 = not_repeat_points[i][0], not_repeat_points[i][1]
            slope = defaultdict(int)
            for j in range(i+1, n):
                x2, y2 = not_repeat_points[j][0], not_repeat_points[j][1]
                dx, dy = x2 - x1, y2 - y1
                g = gcd(dy, dx)
                if g != 0:
                    dy = dy // g
                    dx = dx // g
                slope["{}/{}".format(dy, dx)] += point_dict[not_repeat_points[j]]
            out = max(out, max(slope.values()) + point_dict[not_repeat_points[i]])
        return out
执行用时：44 ms, 在所有 Python 提交中击败了79.85%的用户
内存消耗：13.1 MB, 在所有 Python 提交中击败了60.81%的用户
```

### [219. 存在重复元素 II](https://leetcode-cn.com/problems/contains-duplicate-ii/)

**哈希表**：利用哈希表维护一个窗口大小为k的序列。

```Python
class Solution(object):
    def containsNearbyDuplicate(self, nums, k):
        """
        :type nums: List[int]
        :type k: int
        :rtype: bool
        """
        if not nums or len(nums) < 2:
            return False
        hsah_map = set()
        for i in range(len(nums)):
            if nums[i] in hsah_map:
                return True
            hsah_map.add(nums[i])
            if len(hsah_map) > k:
                hsah_map.remove(nums[i-k])
        return False
执行用时：92 ms, 在所有 Python 提交中击败了22.54%的用户
内存消耗：20 MB, 在所有 Python 提交中击败了37.17%的用户
```

**大佬解法，哈希表**：https://leetcode-cn.com/problems/contains-duplicate-ii/solution/python3-ha-xi-biao-by-ting-ting-28/

```
class Solution(object):
    def containsNearbyDuplicate(self, nums, k):
        """
        :type nums: List[int]
        :type k: int
        :rtype: bool
        """
        hash_map = {}
        for i in range(len(nums)):
            if nums[i] in hash_map and hash_map[nums[i]] >= i-k:
                return True
            hash_map[nums[i]] = i
        return False
执行用时：60 ms, 在所有 Python 提交中击败了40.77%的用户
内存消耗：22 MB, 在所有 Python 提交中击败了32.38%的用户        
```

### [220. 存在重复元素 III](https://leetcode-cn.com/problems/contains-duplicate-iii/)

**哈希表**：在建立窗口的基础上多了一个判断。

```Python
class Solution(object):
    def containsNearbyAlmostDuplicate(self, nums, k, t):
        """
        :type nums: List[int]
        :type k: int
        :type t: int
        :rtype: bool
        """
        if not nums and len(nums) < 2:
            return False
        hash_map =set()
        for i in range(len(nums)):
            if t == 0 and nums[i] in hash_map:
                return True
            elif t != 0:
                for num in hash_map:
                    if abs(nums[i] - num) <= t:
                        return True
            hash_map.add(nums[i])
            if len(hash_map) > k:
                hash_map.remove(nums[i-k])
        return False
执行用时：6076 ms, 在所有 Python 提交中击败了5.66%的用户
内存消耗：14.4 MB, 在所有 Python 提交中击败了93.80%的用户
```

**大佬解法**：桶排序，空间换时间。https://leetcode-cn.com/problems/contains-duplicate-iii/solution/gong-shui-san-xie-yi-ti-shuang-jie-hua-d-dlnv/

```python
class Solution(object):
    def containsNearbyAlmostDuplicate(self, nums, k, t):
        """
        :type nums: List[int]
        :type k: int
        :type t: int
        :rtype: bool
        """
        def getIdx(u):
            return ((u + 1) // size) - 1 if u < 0 else u // size   
        map = {}
        size = t + 1
        for i,u in enumerate(nums):
            idx = getIdx(u)
            # 目标桶已存在（桶不为空），说明前面已有 [u - t, u + t] 范围的数字
            if idx in map:
                return True
            # 检查相邻的桶
            l, r = idx - 1, idx + 1
            if l in map and abs(u - map[l]) <= t:
                return True
            if r in map and abs(u - map[r]) <= t:
                return True
            # 建立目标桶
            map[idx] = u
            # 维护个数为k
            if i >= k:
                map.pop(getIdx(nums[i-k]))
        return False
执行用时：40 ms, 在所有 Python 提交中击败了88.14%的用户
内存消耗：14.9 MB, 在所有 Python 提交中击败了81.13%的用户
```

## 排序（非题目，纯知识点）

### 插入排序

* 第i个元素左边的元素已经排序，将第i个元素与前边的已排序序列进行比较，将其插入到前面已排序的序列中比它小的数的前面（如果有）。
* 遍历整个数组，完成排序
* 时间复杂度：平均O(n^2), 最坏O(n^2), 最好O(n)
* 稳定的排序算法

```python
def InsertSort(numbers:List[int]):
    if len(numbers) <= 1:
        return numbers
    for right in range(1, len(numbers)): 
        targrt = numbers[right]
        for left in range (right):
            #第i个元素比之前的元素小，进行交换
            if targrt < numbers[left]:
                numbers[left+1:right+1] = numbers[left:right]
                numbers[left] = target
                break
```

### 希尔排序

* 希尔排序也是一种插入排序，它是简单插入排序经过改进之后的一个更高效的版本，也称为缩小增量排序，同时该算法是冲破O(n2)的第一批算法之一。
* 等间隔取元素组成子序列进行排序，间隔逐渐变小，序列的长度逐渐增长
* 如果所取的间隔互相不互质，那么小间隔的一部分序列则不起作用，效果变坏
* 时间复杂度：平均O(n^1.3), 最坏O(n^2), 最好O(n)
* 不稳定的排序算法

以下引用自：https://blog.csdn.net/double_sweet1/article/details/101476886

![图解算法---希尔排序](https://imgconvert.csdnimg.cn/aHR0cDovL3AxLnBzdGF0cC5jb20vbGFyZ2UvcGdjLWltYWdlLzE1MjM3NjMzOTA0MTE3MzFjNGQ0ODZk?x-oss-process=image/format,png)

![图解算法---希尔排序](https://imgconvert.csdnimg.cn/aHR0cDovL3AzLnBzdGF0cC5jb20vbGFyZ2UvcGdjLWltYWdlLzE1MjM3NjMzOTA2MjE4Mzg5ZjQ2ZTI0?x-oss-process=image/format,png)

![图解算法---希尔排序](https://imgconvert.csdnimg.cn/aHR0cDovL3AzLnBzdGF0cC5jb20vbGFyZ2UvcGdjLWltYWdlLzE1MjM3NjMzOTA1ODRkOTY2MDRiNzAx?x-oss-process=image/format,png)

![图解算法---希尔排序](https://imgconvert.csdnimg.cn/aHR0cDovL3A5LnBzdGF0cC5jb20vbGFyZ2UvcGdjLWltYWdlLzE1MjM3NjMzOTA2MDVmMWFjYzY1NGM4?x-oss-process=image/format,png)

![图解算法---希尔排序](https://imgconvert.csdnimg.cn/aHR0cDovL3AxLnBzdGF0cC5jb20vbGFyZ2UvcGdjLWltYWdlLzE1MjM3NjMzOTA2MzI4OWM0ZjI4N2Ez?x-oss-process=image/format,png)

![图解算法---希尔排序](https://imgconvert.csdnimg.cn/aHR0cDovL3AzLnBzdGF0cC5jb20vbGFyZ2UvcGdjLWltYWdlLzE1MjM3NjMzOTA2ODc2ZDFkYjk0Y2Y5?x-oss-process=image/format,png)

![图解算法---希尔排序](https://imgconvert.csdnimg.cn/aHR0cDovL3A5LnBzdGF0cC5jb20vbGFyZ2UvcGdjLWltYWdlLzE1MjM3NjMzOTA3NjFmN2YzMmRmNTI4?x-oss-process=image/format,png)

![图解算法---希尔排序](https://imgconvert.csdnimg.cn/aHR0cDovL3AzLnBzdGF0cC5jb20vbGFyZ2UvcGdjLWltYWdlLzE1MjM3NjMzOTA4NzA3MjcwMjc4MmM1?x-oss-process=image/format,png)

​                                                                                                         **对这仅有的一组数据进行排序，排序完成**

```python
def ShellSort(arr:List[int]): 

    n = len(arr)
    gap = int(n/2) ## 取整

    while gap > 0: 

        for i in range(gap,n): 

            temp = arr[i] 
            j = i 
            while  j >= gap and arr[j-gap] >temp: 
                arr[j] = arr[j-gap] 
                j -= gap 
            arr[j] = temp 
        gap = int(gap/2)
```

### 选择排序

选择排序（Selection sort）是一种简单直观的排序算法。它的工作原理如下。首先在未排序序列中找到最小（大）元素，存放到排序序列的起始位置，然后，再从剩余未排序元素中继续寻找最小（大）元素，然后放到已排序序列的末尾。以此类推，直到所有元素均排序完毕。

![img](https://www.runoob.com/wp-content/uploads/2019/03/selectionSort.gif)

```python
def SelectSort(numbers:List[int]):
    for i in range(len(numbers)-1):
        min_idx = i
        for j in range(i+1, len(numbers)):
            if numbers[min_idx] > numbers[j]:
                min_idx = j

        numbers[i], numbers[min_idx] = numbers[min_idx], numbers[i]
```

### 冒泡排序

原理：比较两个相邻的元素，将值大的元素交换到右边

思路：依次比较相邻的两个数，将比较小的数放在前面，比较大的数放在后面。

　　　　(1)第一次比较：首先比较第一和第二个数，将小数放在前面，将大数放在后面。

　　　　(2)比较第2和第3个数，将小数放在前面，大数放在后面。

　　　　......

　　　　(3)如此继续，直到比较到最后的两个数，将小数放在前面，大数放在后面，重复步骤，直至全部排序完成

　　　　(4)在上面一趟比较完成后，最后一个数一定是数组中最大的一个数，所以在比较第二趟的时候，最后一个数是不参加比较的。

　　　　(5)在第二趟比较完成后，倒数第二个数也一定是数组中倒数第二大数，所以在第三趟的比较中，最后两个数是不参与比较的。

　　　　(6)依次类推，每一趟比较次数依次减少一个。

```python
def BubbleSort(numbers):

    for i in range(1, len(numbers)):
        for j in range(len(numbers) - i) :
            if numbers[j] > numbers[j + 1] :
                numbers[j + 1], numbers[j] = numbers[j], numbers[j + 1]
```

### 堆排序

**知识点来源**：知乎https://zhuanlan.zhihu.com/p/128892381

博客https://www.cnblogs.com/chengxiao/p/6129630.html

**堆排序**是一种原地、时间复杂度O(nlogn)的排序算法，它借助了一种数据结构-堆(heap)。

#### 什么是堆？

首先堆是一种树，一种满足以下特质的树结构：

堆是一个完全二叉树
堆中每一个节点的值必须大于或者等于（小于或者等于）其左右子树的值。【大顶堆、小顶堆】
![img](https://images2015.cnblogs.com/blog/1024555/201612/1024555-20161217182750011-675658660.png)

#### 如何实现一个堆

存储一个完全二叉树，最适合使用数组，因为它相比链表不需要存储左、右子树的指针，更加节省内存空间，通过数组索引即可以随机访问到对应元素。

![img](https://images2015.cnblogs.com/blog/1024555/201612/1024555-20161217182857323-2092264199.png)

该数组从逻辑上讲就是一个堆结构，我们用简单的公式来描述一下堆的定义就是：

大顶堆：arr[i] >= arr[2i+1] && arr[i] >= arr[2i+2]  

小顶堆：arr[i] <= arr[2i+1] && arr[i] <= arr[2i+2] 

#### 常见的堆操作

##### 往堆里面插入一个元素

当我们往堆里面插入一个元素后，我们需要对新的堆进行调整，使其能够满足上述的两个特性，那么这个过程我们称为堆化。

堆化实际上有两个操作，一个是从下往上、一个是从上往下。

**从下往上**

![img](https://pic4.zhimg.com/80/v2-bef40b645a15edce0ff7bf753ad70df7_720w.jpg)

从下往上堆化就是指把该元素与其父节点对比、满足条件后交换位置、继续对比、交换，直到不能交换了。

![img](https://pic3.zhimg.com/80/v2-0ba8e907b1808075eadc85df033a2666_720w.jpg)

**删除堆顶元素**

我们都知道大顶堆、小顶堆的堆顶位置存储的分别是最大值、最小值。

假设我们构造的是一个大顶堆，当我们删除堆顶元素后，我们需要把之前堆内第二大元素移到堆顶位置，依次类推，我们依次删除第二大节点、第三大节点、直到叶子节点。

![img](https://pic4.zhimg.com/80/v2-a82535873f443fb915d383a7acf81443_720w.jpg)

上面的思路其实就是：当我们删除堆顶元素时候，直接把最后一个节点放到堆顶位置，然后从上往下进行堆化。如果父节点的值小于其子节点的值，则进行交换、继续往下对比。

#### 建堆

我们把无序数组构建成一个二叉堆：

如果你需要对无序数组进行从小到大排序，那么你应该构建为大顶堆；

如果你需要对无序数组进行从大到小排序，那么你应该构建为小顶堆。

假设建堆结束之后，数组中的数据已经是按照大顶堆的特性来组织的。

数组中的第一个元素就是堆顶，也就是最大的元素。我们把它跟最后一个元素交换，那最大元素就放到了下标为 n 的位置。这个过程有点类似上面讲的“删除堆顶元素”的操作，当堆顶元素移除之后，我们把下标为 n 的元素放到堆顶，然后再通过堆化的方法，将剩下的 n−1 个元素重新构建成堆。

堆化完成之后，我们再取堆顶的元素，放到下标是 n−1 的位置，一直重复这个过程，直到最后堆中只剩下标为 1 的一个元素，排序工作就完成了。

```python
def heapify(arr, n, i): 
    largest = i  
    l = 2 * i + 1     # left = 2*i + 1 
    r = 2 * i + 2     # right = 2*i + 2 

    if l < n and arr[i] < arr[l]: 
        largest = l 

    if r < n and arr[largest] < arr[r]: 
        largest = r 

    if largest != i: 
        arr[i],arr[largest] = arr[largest],arr[i]  # 交换

        heapify(arr, n, largest) 

def heapSort(arr): 
    n = len(arr) 

    # Build a maxheap. 
    for i in range(n//2-1, -1, -1): # 一个节点的左右子节点的下标分别为2i+1和2i+2，因此只需要考虑前一半元素即可
        heapify(arr, n, i) 

    # 一个个交换元素
    for i in range(n-1, 0, -1): 
        arr[i], arr[0] = arr[0], arr[i]   # 交换
        heapify(arr, i, 0) 
```

### 归并排序

**来源**：归并排序（MERGE-SORT）是利用**归并**的思想实现的排序方法，该算法采用经典的**分治**（divide-and-conquer）策略（分治法将问题**分**(divide)成一些小的问题然后递归求解，而**治(conquer)**的阶段则将分的阶段得到的各答案"修补"在一起，即分而治之)。

![img](https://images2015.cnblogs.com/blog/1024555/201612/1024555-20161218163120151-452283750.png)

可以看到这种结构很像一棵完全二叉树，本文的归并排序我们采用递归去实现（也可采用迭代的方式去实现）。分阶段可以理解为就是递归拆分子序列的过程，递归深度为log2n。

再来看看治阶段，我们需要将两个已经有序的子序列合并成一个有序序列，比如上图中的最后一次合并，要将[4,5,7,8]和[1,2,3,6]两个已经有序的子序列，合并为最终序列[1,2,3,4,5,6,7,8]，来看下实现步骤。

![img](https://images2015.cnblogs.com/blog/1024555/201612/1024555-20161218194508761-468169540.png)

```python
#利用递归来实现拆分

def Merge(left, right):
    """
    合并两个已排序子序列
    :param left: 第一个已排序的子序列
    :param right: 第二个已排序的子序列
    :return:  合并后的排序序列
    """
    i = 0
    j = 0
    result = []
    length_left = len(left)
    length_right = len(right)

    while i < length_left and j < length_right:
        # 逐个比较两个列表的元素
        # 小的添加进新列表，大的留下继续比较
        if left[i] <= right[j]:
            result.append(left[i])
            i += 1
        else:
            result.append(right[j])
            j += 1

    # 最后加上未比较的元素
    result.extend(left[i:])
    result.extend(right[j:])
    return result


def MergeSort(numbers):
    """
    归并排序算法入口，采用递归拆分列表
    :param numbers: 输入待排序数组
    :return:  排序后的数组
    """
    # 递归退出条件判断
    length = len(numbers)
    if length <= 1:
        return numbers

    # 递归拆分，取整
    mid = length // 2
    left = MergeSort(numbers[:mid])
    right = MergeSort(numbers[mid:])

    # 合并排序（归并排序）
    return Merge(left, right)
```

### 快速排序

* 分而治之思想
* 类似于归并排序
* 首先找到一个基准值，一般将序列的第一个元素作为基准值
* 将比基准值小的元素均放置在基准值左边，大于基准值的放置在右边
* 然后递归处理左边与右边的序列
* 时间复杂度：平均O(nlogn), 最坏O(nlogn), 最好O(nlogn)
* 不稳定

![img](https://www.runoob.com/wp-content/uploads/2019/03/quickSort.gif)

```python
def partition(arr,low,high): 
    i = low-1         # 最小元素索引
    pivot = arr[high]     

    for j in range(low, high): 

        # 当前元素小于或等于 pivot 
        if arr[j] <= pivot: 

            i = i+1 
            arr[i],arr[j] = arr[j],arr[i] 

    arr[i+1],arr[high] = arr[high],arr[i+1] 
    return i+1


# arr[] --> 排序数组
# low  --> 起始索引
# high  --> 结束索引

# 快速排序函数
def quickSort(arr,low,high): 
    if low < high: 

        pi = partition(arr,low,high) 

        quickSort(arr, low, pi-1) 
        quickSort(arr, pi+1, high) 
```

### 计数排序

* 统计每个数字出现的次数，然后进行重新赋值，完成排序
* 时间复杂度：平均O(n+k), 最坏O(n+k), 最好O(n+k)
* 稳定

![img](https://www.runoob.com/wp-content/uploads/2019/03/countingSort.gif)

```python
def CountSort(nums):
    bucket = [0] * (max(nums) + 1) # 桶的个数
    for num in nums:  # 将元素值作为键值存储在桶中，记录其出现的次数
        bucket[num] += 1
    i = 0  # nums 的索引
    for j in range(len(bucket)):
        while bucket[j] > 0:
            nums[i] = j
            bucket[j] -= 1
            i += 1
```

### 桶排序

* 计数排序的升级版，采用桶映射函数
* 将数组中最大值与最小值中间的元素，放置在对应的桶中，然后对于每个桶中的元素进行排序
* 映射函数的选取，影响了性能，尽可能均匀的将元素放置在桶中
* 最好的情况：均匀放置，最差的情况：所有数据放置在一个桶中
* 时间复杂度：平均O(n+k), 最坏O(n^2), 最好O(n)
* 稳定

![img](https://img-blog.csdnimg.cn/20190219081232815.png)

```python
## 借用了上边的快排
from quick_sort import QuickSort

def BucketSort(nums, defaultBucketSize = 100):
    """
    桶排序
    :param nums: 输入的待排序数组
    :param defaultBucketSize: 桶数目，不指定时为100
    :return: inplace 操作
    """
    maxVal, minVal = max(nums), min(nums)

    bucketSize = defaultBucketSize  # 如果没有指定桶的大小，则默认为100

    bucketCount = ((maxVal - minVal) // bucketSize) + 1  # 数据分为 bucketCount 组

    buckets = []  # 二维桶

    for i in range(bucketCount):
        buckets.append([])

    # 利用函数映射将各个数据放入对应的桶中
    for num in nums:
        buckets[(num - minVal) // bucketSize].append(num)

    nums.clear()  # 清空 nums
    # print(buckets)

    # 对每一个二维桶中的元素进行排序
    for bucket in buckets:
        QuickSort(bucket)  # 使用快速排序

        nums.extend(bucket)    # 将排序好的桶依次放入到 nums 中
```

### 基数排序

* 桶排序的一种扩展
* 仅仅使用10个桶，标号为0-9
* 分别对待排序数组，个位，十位， ... 分别进行桶排序
* 时间复杂度：平均O(nk), 最坏O(nk), 最好O(nk)
* 稳定

![img](https://www.runoob.com/wp-content/uploads/2019/03/radixSort.gif)

```Python
def RadixSort(numbers):
    """
    基数排序
    :param numbers:输入待排序数组
    :return:
    """
    bucket_count = 10 ##桶的数目
    div = 1  # 除数，去除数组中的元素，得到相应的位数
    loop_count = len(str(max(numbers))) #最大数字的位数决定了循环的次数

    #创建bucket_count个空桶
    buckets = [[] for i in range(bucket_count)]

    while loop_count:
        # 将数据存入桶中
        for num in numbers:
            buckets[num // div % bucket_count].append(num)

        #将数据依次取出
        i = 0  # 待排序数组numbers的索引
        for bucket in buckets:
            while bucket:
                numbers[i] = bucket.pop(0)  # 依次取出
                i += 1

        div *= 10
        loop_count -= 1
```

## 二分查找

**二分查找用途** 对于一个排序列表，在其中查找指定的元素，采用二分查找，时间复杂度为O(logN)

![img](https://www.runoob.com/wp-content/uploads/2019/05/Binary_search_into_array.png)

二分查找基本代码实现思路：https://www.runoob.com/python3/python-binary-search.html

```python
# 返回 x 在 arr 中的索引，如果不存在返回 -1
def binarySearch (arr, l, r, x): 

    # 基本判断
    if r >= l: 

        mid = int(l + (r - l)/2)

        # 元素整好的中间位置
        if arr[mid] == x: 
            return mid 

        # 元素小于中间位置的元素，只需要再比较左边的元素
        elif arr[mid] > x: 
            return binarySearch(arr, l, mid-1, x) 

        # 元素大于中间位置的元素，只需要再比较右边的元素
        else: 
            return binarySearch(arr, mid+1, r, x) 

    else: 
        # 不存在
        return -1

# 测试数组
arr = [ 2, 3, 4, 10, 40 ] 
x = 10

# 函数调用
result = binarySearch(arr, 0, len(arr)-1, x) 

if result != -1: 
    print ("元素在数组中的索引为 %d" % result )
else: 
    print ("元素不在数组中")
```

### [69. x 的平方根](https://leetcode-cn.com/problems/sqrtx/)

```Python
class Solution(object):
    def mySqrt(self, x):
        """
        :type x: int
        :rtype: int
        """
        l, r = 0, x // 2 + 1
        while l <= r:
            m = l + (r-l)//2
            s1 = m ** 2
            s2 = (m+1) ** 2
            if s1 < x and s2 > x or s1 == x:
                return m
            if s2 == x:
                return m + 1
            if s1 > x:
                r = m - 1
            elif s2 < x:
                l = m + 1
        return m
执行用时：28 ms, 在所有 Python 提交中击败了75.78%的用户
内存消耗：12.9 MB, 在所有 Python 提交中击败了86.18%的用户
```

### [744. 寻找比目标字母大的最小字母](https://leetcode-cn.com/problems/find-smallest-letter-greater-than-target/)

```Python
class Solution(object):
    def nextGreatestLetter(self, letters, target):
        """
        :type letters: List[str]
        :type target: str
        :rtype: str
        """
        l, r = 0,len(letters)-1
        while l <= r:
            m = l + (r-l)//2
            if letters[m] <= target:
                l = m + 1
            else:
                r = m - 1
        return letters[l] if l<len(letters) else letters[0]
执行用时：24 ms, 在所有 Python 提交中击败了94.89%的用户
内存消耗：14.3 MB, 在所有 Python 提交中击败了64.23%的用户
```

### [540. 有序数组中的单一元素](https://leetcode-cn.com/problems/single-element-in-a-sorted-array/)

**解法**：由于题目限定是只有一个元素只出现一次，其余元素必定出现两次，可以从下标0开始比较下一位是否和自己相同，不是指针前移2位，继续比较。但是不满足O(logn)的时间要求。

```python
class Solution(object):
    def singleNonDuplicate(self, nums):
        """
        :type nums: List[int]
        :rtype: int
        """
        n = len(nums)
        index = 0
        while index < n-1:
            if nums[index + 1] == nums[index]:
                index += 2
            else:
                break
        return nums[index]
执行用时：16 ms, 在所有 Python 提交中击败了90.57%的用户
内存消耗：14.2 MB, 在所有 Python 提交中击败了93.71%的用户
```

**解法**：二分查找。由于只有一个元素是出现一次，那么数组长度必定为奇数，利用中间值检查，如果中间值和左右都不相等，则是我们要寻找的目标；如果和左边相等，则目标值在左边；如果和右边相等，则目标值在右边。

```Python
class Solution:
    def singleNonDuplicate(self, nums: List[int]) -> int:
        '''
        题目要求时间复杂度为log(n)，因此采用二分法
        利用二分法，待查找的元素在左右两边奇数那个子列中，这样比顺序查找降低时间复杂度
        '''
        l = 0
        r = len(nums) - 1   
        while l < r:
            m = l + (r - l) // 2
            ## 右边子序列为偶数
            halves_are_even = (r - m) % 2 == 0

            if nums[m + 1] == nums[m]:
            # 如果中间的元素与右边元素相等，且右边为偶数，那么去除一个之后，右边序列变为奇数，待查找的元素在右侧子列
                if halves_are_even:
                    l = m + 2
                ## 右侧为奇数列，去除一个之后变成偶数，那么待查找的元素位于左侧子列
                else:
                    r = m - 1
            elif nums[m - 1] == nums[m]:
                if halves_are_even:
                    r = m - 2
                else:
                    l = m + 1
            ## 中间元素刚好为待查找的元素，直接返回
            else:
                return nums[m]
        return nums[l] ## return[r]也可以，此时必是左右指针都指向了目标元素
执行用时: 8 ms, 在所有 Python 提交中击败了100%的用户
内存消耗: 14.4 MB, 在所有 Python 提交中击败了52%的用户
```

### [278. 第一个错误的版本](https://leetcode-cn.com/problems/first-bad-version/)

```python
# The isBadVersion API is already defined for you.
# @param version, an integer
# @return a bool
# def isBadVersion(version):

class Solution(object):
    def firstBadVersion(self, n):
        """
        :type n: int
        :rtype: int
        """
        if n < 2:
            return n
        l, r = 1, n
        while l <= r:
            m = l + (r-l)//2
            if isBadVersion(m): ## 第一个错误的版本必定在r的右边
                r = m - 1
            else:
                l = m + 1
        return l
执行用时：20 ms, 在所有 Python 提交中击败了39.75%的用户
内存消耗：12.9 MB, 在所有 Python 提交中击败了85.38%的用户
## 删除第一个判断会快很多
执行用时：12 ms, 在所有 Python 提交中击败了90.81%的用户
内存消耗：13 MB, 在所有 Python 提交中击败了50.70%的用户
```

### [153. 寻找旋转排序数组中的最小值](https://leetcode-cn.com/problems/find-minimum-in-rotated-sorted-array/)

```python
class Solution(object):
    def findMin(self, nums):
        """
        :type nums: List[int]
        :rtype: int
        """
        if nums[-1] > nums[0] or len(nums) == 1:
            return nums[0]
        l, r = 0, len(nums)-1
        while l<r:
            m = l + (r-l)//2
            if nums[m] < nums[m-1]:
                return nums[m]
            if nums[m] > nums[m+1]:
                return nums[m+1]
            if nums[m] > nums[-1]: ## 比最后一个大，证明最小值在m的右边
                l = m + 1
            elif nums[m] < nums[-1]:
                r = m - 1
执行用时：16 ms, 在所有 Python 提交中击败了83.89%的用户
内存消耗：13 MB, 在所有 Python 提交中击败了83.89%的用户
```

### [34. 在排序数组中查找元素的第一个和最后一个位置](https://leetcode-cn.com/problems/find-first-and-last-position-of-element-in-sorted-array/)

```python
class Solution(object):
    def searchRange(self, nums, target):
        """
        :type nums: List[int]
        :type target: int
        :rtype: List[int]
        """
        """
        两次二分查找，一次找左边界，一次找右边界
        """
        out = []
        if len(nums) < 1:
            return [-1, -1]
        l, r = 0, len(nums)-1
        while l <= r: ## 找左边界
            m = l + (r-l)//2
            if nums[m] == target:
                if m == 0 or nums[m-1] != target:
                    out.append(m)
                    break
                else:
                    r = m-1
            elif nums[m] > target:
                r = m - 1
            else:
                l = m + 1
        l, r = 0, len(nums)-1
        while l <= r: ## 找右边界
            m = l + (r-l)//2
            if nums[m] == target:
                if m == len(nums)-1 or nums[m+1] != target:
                    out.append(m)
                    break
                else:
                    l = m+1
            elif nums[m] > target:
                r = m - 1
            else:
                l = m + 1
        return out if out else [-1, -1]
```

## 数组和矩阵

### [283. 移动零](https://leetcode-cn.com/problems/move-zeroes/)

**解法**：快慢指针

```python
class Solution(object):
    def moveZeroes(self, nums):
        """
        :type nums: List[int]
        :rtype: None Do not return anything, modify nums in-place instead.
        """
        if len(nums) < 2:
            return nums
        slow, fast = 0, 1
        while fast < len(nums):
            if nums[slow] == 0 and nums[fast] != 0:
                nums[slow], nums[fast] = nums[fast], nums[slow]
                slow += 1
                fast += 1
            elif nums[slow] == 0:
                fast += 1
            else:
                fast += 1
                slow += 1
        return nums
执行用时：16 ms, 在所有 Python 提交中击败了93.53%的用户
内存消耗：13.7 MB, 在所有 Python 提交中击败了67.02%的用户
```

### [566. 重塑矩阵](https://leetcode-cn.com/problems/reshape-the-matrix/)

**解法**：将对应元素直接填入。

```Python
class Solution(object):
    def matrixReshape(self, mat, r, c):
        """
        :type mat: List[List[int]]
        :type r: int
        :type c: int
        :rtype: List[List[int]]
        """
        row, column = len(mat), len(mat[0])
        if row * column != r * c:
            return mat
        out = [[0]*c for _ in range(r)]
        for i in range(row):
            for j in range(column):
                index = i * column + j
                out[index // c][index % c] = mat[i][j]
        return out 
执行用时：20 ms, 在所有 Python 提交中击败了90.53%的用户
内存消耗：13.7 MB, 在所有 Python 提交中击败了74.21%的用户
```

### [485. 最大连续 1 的个数](https://leetcode-cn.com/problems/max-consecutive-ones/)

```python
class Solution(object):
    def findMaxConsecutiveOnes(self, nums):
        """
        :type nums: List[int]
        :rtype: int
        """
        out = 0
        res = 0
        for i in range(len(nums)):
            if nums[i] == 1:
                res += 1
            else:
                if res > out:
                    out = res
                res = 0
        if res > out:
            out = res
        return out
执行用时：40 ms, 在所有 Python 提交中击败了90.82%的用户
内存消耗：13.4 MB, 在所有 Python 提交中击败了33.25%的用户
```

### [240. 搜索二维矩阵 II](https://leetcode-cn.com/problems/search-a-2d-matrix-ii/)

```python
class Solution(object):
    def searchMatrix(self, matrix, target):
        """
        :type matrix: List[List[int]]
        :type target: int
        :rtype: bool
        """
        """
        从右上角开始比较
        """
        if not matrix:
            return False
        i, j = 0, len(matrix[0]) -1
        while(i < len(matrix) and j >= 0):
            if matrix[i][j] == target:
                return True
            elif target > matrix[i][j]:
                i += 1
            else:
                j -= 1
        return False
执行用时：132 ms, 在所有 Python 提交中击败了91.99%的用户
内存消耗：19 MB, 在所有 Python 提交中击败了51.32%的用户
```

### [378. 有序矩阵中第 K 小的元素](https://leetcode-cn.com/problems/kth-smallest-element-in-a-sorted-matrix/)

**解法**：堆排序

```Python
class Solution(object):
    def kthSmallest(self, matrix, k):
        """
        :type matrix: List[List[int]]
        :type k: int
        :rtype: int
        """
        '''
        前k小的元素，一看就是利用堆排序，构建大根堆，自己实现构建堆排序，找第k小元素
        '''
        nums = [y for x in matrix for y in x]
        left = lambda i: 2 * i + 1
        right = lambda i: 2 * i + 2
        self.heap_size = 0
        def HeapSort(nums):
            BuildMaxHeap(nums)
            for i in range(len(nums)-1, -1, -1):
                nums[i], nums[0] = nums[0], nums[i]
                self.heap_size -= 1
                KeepHeap(nums, 0)

        def BuildMaxHeap(nums):
            self.heap_size = len(nums)
            for i in range(len(nums)//2-1, -1, -1):
                KeepHeap(nums, i)

        def KeepHeap(nums, i):
            l, r = left(i), right(i)
            largest = l if l < self.heap_size and nums[l] > nums[i] else i
            largest = r if r < self.heap_size and nums[r] > nums[largest] else largest
            if i != largest:
                nums[i], nums[largest] = nums[largest], nums[i]
                KeepHeap(nums, largest)

        HeapSort(nums)
        return nums[k-1]
执行用时：636 ms, 在所有 Python 提交中击败了8.54%的用户
内存消耗：16.5 MB, 在所有 Python 提交中击败了99.50%的用户
```

**归并排序**

```Python
class Solution:
    def kthSmallest(self, matrix: List[List[int]], k: int) -> int:
        n = len(matrix) #注：题目中这个矩阵是n*n的，所以长宽都是n

        pq = [(matrix[i][0], i, 0) for i in range(n)] #取出第一列候选人
        #matrix[i][0]是具体的值，后面的(i,0)是在记录候选人在矩阵中的位置，方便每次右移添加下一个候选人

        heapq.heapify(pq) #变成一个heap

        for i in range(k - 1): #一共弹k次：这里k-1次，return的时候1次
            num, x, y = heapq.heappop(pq) #弹出候选人里最小一个
            if y != n - 1: #如果这一行还没被弹完
                heapq.heappush(pq, (matrix[x][y + 1], x, y + 1)) #加入这一行的下一个候选人

        return heapq.heappop(pq)[0]
执行用时：76 ms, 在所有 Python 提交中击败了46.23%的用户
内存消耗：16.8 MB, 在所有 Python 提交中击败了62.31%的用户
```

**二分查找**

```python
class Solution(object):
    def kthSmallest(self, matrix, k):
        """
        :type matrix: List[List[int]]
        :type k: int
        :rtype: int
        """
        n = len(matrix)
        def check(mid):
            i, j = n-1, 0
            num = 0
            while i >=0 and j < n:
                if matrix[i][j] <= mid:
                    num += i + 1
                    j += 1
                else:
                    i -= 1
            return num >= k

        left, right = matrix[0][0], matrix[-1][-1]
        while left < right:
            mid = (left + right) // 2
            if check(mid):
                right = mid
            else:
                left = mid + 1
        return left
执行用时：28 ms, 在所有 Python 提交中击败了86.43%的用户
内存消耗：17.1 MB, 在所有 Python 提交中击败了21.11%的用户
```

### [645. 错误的集合](https://leetcode-cn.com/problems/set-mismatch/)

**解法**：https://leetcode-cn.com/problems/set-mismatch/solution/645cuo-wu-de-ji-he-shu-zu-bian-li-ha-xi-94hcp/

纯数学的角度解题：
sum(nums) - sum(set(nums)) = 重复的数字
(1 + len(nums)) * len(nums) // 2 - sum(set(nums)) = 丢失的数字

```python
class Solution(object):
    def findErrorNums(self, nums):
        """
        :type nums: List[int]
        :rtype: List[int]
        """
        l, total = len(nums), sum(set(nums))
        return [sum(nums)-total, (l+1)*l//2 - total]
```

### [287. 寻找重复数](https://leetcode-cn.com/problems/find-the-duplicate-number/)

**解法**：二分法寻找。采用二分法，多次遍历数组，以时间换取空间。具体操作：数字在1到n之间，二分整个区间，得到中间元素mid=(n+1)//2；如果数组中小于mid的数字的个数大于mid，那么重复的数字存在于0到mid之间

```python
class Solution(object):
    def findDuplicate(self, nums):
        """
        :type nums: List[int]
        :rtype: int
        """
        n = len(nums)
        l, r = 1, n
        while l < r:
            mid = l + (r-l) // 2
            count = 0
            for num in nums:
                if num <= mid:
                    count += 1
            if count > mid:
                r = mid
            else:
                l = mid + 1
        return l
执行用时：240 ms, 在所有 Python 提交中击败了27.31%的用户
内存消耗：21 MB, 在所有 Python 提交中击败了76.42%的用户
```

**暴力**：直接把找到的数字加入到新的数组中，当遇到重复数字时返回。

```python
class Solution(object):
    def findDuplicate(self, nums):
        """
        :type nums: List[int]
        :rtype: int
        """
        l = set()
        for num in nums:
            if num not in l:
                l.add(num)
            else:
                return num
执行用时：64 ms, 在所有 Python 提交中击败了89.38%的用户
内存消耗：27.3 MB, 在所有 Python 提交中击败了7.45%的用户
```

**双指针**：

```python
class Solution(object):
    def findDuplicate(self, nums):
        """
        :type nums: List[int]
        :rtype: int
        """
        slow, fast = 0, 0
        while True:
            slow, fast = nums[slow], nums[nums[fast]]
            if slow == fast:
                break
        slow = 0
        while slow != fast:
            slow, fast = nums[slow], nums[fast]
        return slow
执行用时：72 ms, 在所有 Python 提交中击败了85.66%的用户
内存消耗：21 MB, 在所有 Python 提交中击败了78.49%的用户
```
