# LeetCode刷题

## 双指针（对撞指针）

### 167.有序数组的加和（easy）

题目链接：https://leetcode-cn.com/problems/two-sum-ii-input-array-is-sorted/

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

### 633.两数平方和（medium）

题目链接：https://leetcode-cn.com/problems/sum-of-square-numbers/

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

### 345.反转元音字符（easy）

题目链接：https://leetcode-cn.com/problems/reverse-vowels-of-a-string/

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

### 125.验证回文串（easy）

题目链接：https://leetcode-cn.com/problems/valid-palindrome/

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

