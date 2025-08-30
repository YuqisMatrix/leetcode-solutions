# DAY8 More String and 2 pointers

# 1. 151 Reverse Words in a String

https://leetcode.com/problems/reverse-words-in-a-string/description/

Given an input string `s`, reverse the order of the **words**.

A **word** is defined as a sequence of non-space characters. The **words** in `s` will be separated by at least one space.

Return *a string of the words in reverse order concatenated by a single space.*

**Note** that `s` may contain leading or trailing spaces or multiple spaces between two words. The returned string should only have a single space separating the words. Do not include any extra spaces.

**Example 1:**

```
Input: s = "the sky is blue"
Output: "blue is sky the"

```

**Example 2:**

```
Input: s = "  hello world  "
Output: "world hello"
Explanation: Your reversed string should not contain leading or trailing spaces.

```

输入字符串可以在前面或者后面包含多余的空格，但是反转后的字符不能包括。

**Example 3:**

```
Input: s = "a good   example"
Output: "example good a"
Explanation: You need to reduce multiple spaces between two words to a single space in the reversed string.
```

如果两个单词间有多余的空格，将反转后单词间的空格减少到只含一个

solution1: 先删除空白，然后整个反转，最后单词反转。 **因为字符串是不可变类型，所以反转单词的时候，需要将其转换成列表，然后通过join函数再将其转换成列表，所以空间复杂度不是O(1)**

```python
class Solution:
    def reverseWords(self, s: str) -> str:
        # 反转整个字符串
        s = s[::-1]
        # 将字符串拆分为单词，并反转每个单词
        # split()函数能够自动忽略多余的空白字符
        s = ' '.join(word[::-1] for word in s.split())
        return s
```

solution2: 双指针 2 pointer

- use s.split() to turn string into list and trim the space , so each element in the list is a word.

**Input**

s =

"a good   example"

**Stdout**

['a', 'good', 'example']

- AND THEN USE 2 POINTER TO REVERSE THE ORDER OF THE WORDS IN THE STRING

```python
class Solution:
    def reverseWords(self, s: str) -> str:
        # 将字符串拆分为单词，即转换成列表类型
        words = s.split()

        # 反转单词
        left, right = 0, len(words) - 1
        while left < right:
            words[left], words[right] = words[right], words[left]
            left += 1
            right -= 1

        # 将列表转换成字符串
        return " ".join(words)
```

Solution 3: 

拆分字符串 + 反转列表

```python
class Solution:
    def reverseWords(self, s):
        words = s.split() #type(words) --- list
        words = words[::-1] # 反转单词
        return ' '.join(words) #列表转换成字符串
```

Solution 4: 将字符串转换为列表后，使用双指针去除空格

- 移除多余空格
- 将每个单词反转
- 将整个字符串反转

举个例子，源字符串为："the sky is blue "

- 移除多余空格 : "the sky is blue"
- 字符串反转："eulb si yks eht"
- 单词反转："blue is sky the"

```python
class Solution:
    def reverseWords(self, s: str) -> str:
        def get_reverse(s,start,end):
            while start < end:
                s[start],s[end] = s[end], s[start]
                start += 1
                end -= 1
        
        #reverse the list of string first 
        s = list(s)
        s.reverse()
        res = ""
        fast = 0 
        #eliminate space in the string s 
        while fast < len(s):
            if s[fast] != " ":
                if len(res) != 0:
                    res += " " #add a space at the beginning of new word
                while fast < len(s) and s[fast] != " ":
                    res += s[fast]
                    fast += 1
            else:
                fast += 1
        #reverse each words 
        slow = 0
        fast = 0 
        res = list(res)
        while fast <=len(res):
            if fast == len(res) or res[fast] == " ":
                get_reverse(res,slow,fast-1)
                slow = fast + 1
                fast += 1
            else:
                fast += 1
        return "".join(res)

        
```

### **Step 2. 去掉多余空格，并逐单词重新构造字符串**

```python
res = ""
fast = 0
while fast < len(s):
    if s[fast] != " ":
        if res:      # 不是第一个单词就加空格
            res += " "
        while fast < len(s) and s[fast] != " ":
            res += s[fast]   # 一个字符一个字符拼接单词
            fast += 1
    else:
        fast += 1

```

- 遍历 `s`：
    - 跳过所有空格
    - 遇到单词 → 把单词里的字符拼出来，追加到 `res`
    - 单词之间只加一个空格
- 结果：
    
    `" eulb si   yks eht  "` → `"eulb si yks eht"`
    

### **Step 3. 翻转每个单词**

```python
res = list(res)
slow = fast = 0
while fast <= len(res):
    if fast == len(res) or res[fast] == " ":
        get_reverse(res, slow, fast-1)
        slow = fast + 1
        fast += 1
    else:
        fast += 1

```

- 用两个指针：
    - `slow` 指向单词开头
    - `fast` 一直走，直到遇到空格或结尾
    - 找到一个单词边界后，就调用 `get_reverse` 翻转单词内部
    - 然后继续处理下一个单词

例子：

- `"eulb"` → `"blue"`

---

# **28. Find the Index of the First Occurrence in a String**

Given two strings `needle` and `haystack`, return the index of the first occurrence of `needle` in `haystack`, or `-1` if `needle` is not part of `haystack`.

**Example 1:**

```
Input: haystack = "sadbutsad", needle = "sad"
Output: 0
Explanation: "sad" occurs at index 0 and 6.
The first occurrence is at index 0, so we return 0.

```

**Example 2:**

```
Input: haystack = "leetcode", needle = "leeto"
Output: -1
Explanation: "leeto" did not occur in "leetcode", so we return -1.
```

# 心得

这个code 哪里错了？

```python
class Solution:
    def strStr(self, haystack: str, needle: str) -> int:
        n = len(haystack)
        m = len(needle)
        slow = 0 
        fast = 0 
        while fast < n and slow <m:
            if haystack[fast] == needle[slow]:
                slow += 1
                fast += 1
            else:
                fast += 1
        if slow == m:
            return fast - slow 
        else:
            return -1
```

我确实想到用双指针扫 word 和target array， 但是 没扫出来结果 要怎么放指针没懂。 

- 首先， 如果找到match 的 needle ， 那么满足的条件是最后一个chr 相等 并且 slow == m， 所以这个 指针遍历完的条件要放在

 if haystack[fast] == needle[slow]: 里

- 如果fast 和 slow 指向的字母不相同该怎么办？ **也就是说mismatch的时候要重置两个指针**
    - 第一， 要重新扫slow， 从slow 第一个 chr 开始扫， 而不是继续扫
    - 那怎么放fast 指针？ 比如
        
        ```python
        haystack = "mississippi"
        needle = "issip"
        
        ```
        
        when fast = 1, slow = 0, 
        
    - 匹配 `"i"` vs `"i"` ✅
    - fast=2, slow=1
    - 匹配 `"s"` vs `"s"` ✅
    - fast=3, slow=2
    - 匹配 `"s"` vs `"s"` ✅
    - fast=4, slow=3
    - 匹配 `"i"` vs `"i"` ✅
    - fast=5, slow=4
    - 匹配 `"s"` vs `"p"` ❌ mismatch！

**注意！！！！：** 

- fast - slow = 1 是本轮开始的下标， 我们从i 开始扫， 也就是index 1.
- 那下一轮就要从index 2 开始扫， 也就是 **fast - slow + 1**

```python
class Solution:
    def strStr(self, haystack: str, needle: str) -> int:
        n = len(haystack)
        m = len(needle)
        slow = 0 
        fast = 0 
        while fast < n:
            if haystack[fast] == needle[slow]:
                slow += 1
                fast += 1
                if slow == m:
                    return fast - slow 
            else:
                fast = fast - slow + 1 
                slow = 0 
        return -1 
```

- **最坏情况：O(n * m)**
- **最好情况：O(n)**
- **空间复杂度：O(1)**

朴素解法：

当遇到 mismatch 时，应该回退 `fast` 到下一个可能的起始点，并把 `slow` 重置为 0

```python
class Solution:
    def strStr(self, haystack: str, needle: str) -> int:
        n = len(haystack)
        m = len(needle)
        for i in range(n-m+1):
            j = 0 
            while j < m and haystack[i+j] == needle[j]:
                j += 1
            if j == m:
                return i 
        return -1
```

---

# **459. Repeated Substring Pattern**

https://leetcode.com/problems/repeated-substring-pattern/description/

Given a string `s`, check if it can be constructed by taking a substring of it and appending multiple copies of the substring together.

**Example 1:**

```
Input: s = "abab"
Output: true
Explanation: It is the substring "ab" twice.

```

**Example 2:**

```
Input: s = "aba"
Output: false

```

**Example 3:**

```
Input: s = "abcabcabcabc"
Output: true
Explanation: It is the substring "abc" four times or the substring "abcabc" twice.
```

# 思路：

We only need to look at substrings where the length divides `n`, where `n` is the length of `s`.

Another thing that we can notice is that the substring would need to be a prefix of `s`, otherwise there would be an immediate mismatch.

For each prefix of length `i` that divides `n`, we would concatenate the prefix `n / i` times to generate another string. If this string generated by repeated substrings equals `s`, we return `true`.

If no prefix is found that when repeated equals `s`, we return `false`.

### **Algorithm**

1. Create an integer variable `n` equal to the length of `s`.
2. Iterate over all the prefix substrings of length `i = 1` to `n / 2`:
    - If `i` divides `n`, we declare an empty string `pattern`. Use an inner loop that iterates `n / i` times to concatenate the substring formed from the first `i` characters of `s`.
    - If `pattern` equals `s`, we return `true`.
3. There is no substring that can be repeated to form `s`. As a result, we return `false`.

```python
class Solution:
    def repeatedSubstringPattern(self, s: str) -> bool:
        n = len(s)
        for i in range(1, n // 2 + 1):
            if n % i == 0:
                pattern = s[:i] * (n // i)
                if s == pattern:
                    return True
        return False
```

### 这里string pattern = s[:i] *** (n // i)** 要用floor 因为 必须要是**整数次数**

# Python 里 `/` 和 `//` 的区别：

### 1. `/` —— 真除法（True Division）

- **总是返回浮点数**（float），即使整除。
- 例子：
    
    ```python
    5 / 2   # 2.5
    4 / 2   # 2.0   (注意结果是浮点数，不是整数)
    
    ```
    

---

### 2. `//` —— 地板除（Floor Division）

- **返回商的整数部分**（向下取整，floor）。
- 结果类型取决于操作数：
    - 如果两个数都是 int → 结果是 int
    - 如果有 float → 结果是 float
- 例子（正数）：
    
    ```python
    5 // 2   # 2
    4 // 2   # 2
    7 // 3   # 2
    
    ```
    
- 例子（负数，注意向下取整）：
    
    ```python
    -5 / 2   # -2.5
    -5 // 2  # -3   (不是 -2，而是往负无穷取整)
    
    ```
    

---

### 总结

- `/` → 真除法，结果永远是小数（float）。
- `//` → 地板除，结果是整数部分（floor），遇到负数时会“向下取整”