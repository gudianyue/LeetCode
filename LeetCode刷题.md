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

