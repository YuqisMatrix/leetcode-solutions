# Day 7 String and Array

# 1. Reverse String

https://leetcode.com/problems/reverse-string/description/

Write a function that reverses a string. The input string is given as an array of characters `s`.

You must do this by modifying the input array [in-place](https://en.wikipedia.org/wiki/In-place_algorithm) with `O(1)` extra memory.

**Example 1:**

```
Input: s = ["h","e","l","l","o"]
Output: ["o","l","l","e","h"]

```

**Example 2:**

```
Input: s = ["H","a","n","n","a","h"]
Output: ["h","a","n","n","a","H"]
```

# 注意： 这题input 不是 string 而是 list，所以可以交换位置。 string本身是immutable 的

HOW TO SOLVE WITHOUT USING reverse()? 

Use 2 pointers, 

1. Put one pointer (`left`) at the **start** (`0`)
2. Put another pointer (`right`) at the **end** (`len(s)-1`)
3. While `left < right`:
    - Swap `s[left]` and `s[right]`
    - Move `left += 1` and `right -= 1`
4. Done — the array is reversed.

```python
class Solution:
    def reverseString(self, s: List[str]) -> None:
        """
        Do not return anything, modify s in-place instead.
        """
        left, right = 0, len(s) - 1
        while left < right:
            s[left], s[right] = s[right], s[left]  # swap
            left += 1
            right -= 1

```

Why do we use `left < right`? For even-length strings, the pointers stop at `left == right - 1` after the last swap; for odd-length strings, they meet at the middle (`left == right`), which doesn’t need a swap. This condition swaps each pair exactly once and leaves the middle element (if any) untouched.

 

### using stack: Last in first out

```python
class Solution:
    def reverseString(self, s: List[str]) -> None:
        """
        Do not return anything, modify s in-place instead.
        """
        stack = []
        for char in s:
            stack.append(char)
        for i in range(len(s)):
            s[i] = stack.pop()
       
```

---

# 2. 541 reverse string 2

https://leetcode.com/problems/reverse-string-ii/description/

Given a string `s` and an integer `k`, reverse the first `k` characters for every `2k` characters counting from the start of the string.

If there are fewer than `k` characters left, reverse all of them. If there are less than `2k` but greater than or equal to `k` characters, then reverse the first `k` characters and leave the other as original.

从字符串开头算起, 每计数至 2k 个字符，就反转这 2k 个字符中的前 k 个字符。

如果剩余字符少于 k 个，则将剩余字符全部反转。

如果剩余字符小于 2k 但大于或等于 k 个，则反转前 k 个字符，其余字符保持原样

**Example 1:**

```
Input: s = "abcdefg", k = 2
Output: "bacdfeg"

```

**Example 2:**

```
Input: s = "abcd", k = 2
Output: "bacd"
```

# 心得：

string 是immutable 的， 所以要先把str 变成 list。

`''.join(l)` 把**可迭代对象 `l` 里的字符串**按指定分隔符**拼接成一个新字符串**

### 小例子

```python
l = ['a', 'number', 'b', 'number', 'c', 'number']
''.join(l)
# -> 'anumberbnumbercnumber'

```

换个分隔符：

```python
' '.join(['hello', 'world'])   # -> 'hello world'
','.join(['a','b','c'])        # -> 'a,b,c'
'\n'.join(['line1','line2'])   # -> 'line1\nline2'

```

如果列表里有数字，先转成字符串：

```python
items = ['id', 123, 456]
''.join(map(str, items))   # -> 'id123456'

```

这道题的重点是， 找到

i += (2 * k)，i 每次移动 2 * k 就可以了，然后判断是否需要有反转的区间。

因为要找的也就是每2 * k 区间的起点。 

思路：

- 用上题的reverse string 的思路， 找到需要reverse 的k 的string的反转结果。
- 要把string 转化成list
- 找到需要operate 的 i = 2k index ， reverse
- use “”join() to get the string result

```python
class Solution:
    def reverseStr(self, s: str, k: int) -> str:
        """
        1. 使用range(start, end, step)来确定需要调换的初始位置
        2. 对于字符串s = 'abc'，如果使用s[0:999] ===> 'abc'。字符串末尾如果超过最大长度，则会返回至字符串最后一个值，这个特性可以避免一些边界条件的处理。
        3. 用切片整体替换，而不是一个个替换.
        """
        def reverse_substring(text):
            left, right = 0, len(text) - 1
            while left < right:
                text[left], text[right] = text[right], text[left]
                left += 1
                right -= 1
            return text
        
        res = list(s)

        for cur in range(0, len(s), 2 * k):
            res[cur: cur + k] = reverse_substring(res[cur: cur + k])
        
        return ''.join(res)
```

---

# **替换数字**

给定一个字符串 s，它包含小写字母和数字字符，请编写一个函数，将字符串中的字母字符保持不变，而将每个数字字符替换为number。

例如，对于输入字符串 "a1b2c3"，函数应该将其转换为 "anumberbnumbercnumber"。

对于输入字符串 "a5b"，函数应该将其转换为 "anumberb"

输入：一个字符串 s,s 仅包含小写字母和数字字符。

输出：打印一个新的字符串，其中每个数字字符都被替换为了number

样例输入：a1b2c3

样例输出：anumberbnumbercnumber

## Problem

Given a string `s` containing only lowercase letters and digits, replace **every digit character** with the word `"number"` while leaving letters unchanged.

- **Input:** A single string `s` (lowercase letters and digits only).
- **Output:** Print a new string where each digit is replaced with `"number"`.

**Example**

- Input: `a1b2c3`
    
    Output: `anumberbnumbercnumber`
    
- Input: `a5b`
    
    Output: `anumberb`
    

Python solution (O(n) time, O(n) space)

```python
s = input().strip()

result = ''.join('number' if ch.isdigit() else ch for ch in s)
print(result)

```

By **reading from the original end** (`old_index`) and **writing to the new end** (`new_index`):

- You **never overwrite unread data**.
- Each character (or block) is written **exactly once** to its final position.

```python
class Solution(object):
    def subsitute_numbers(self, s):
        """
        :type s: str
        :rtype: str
        """
        
        count = sum(1 for char in s if char.isdigit()) # 统计数字的个数
        expand_len = len(s) + (count * 5)  # 计算扩充后字符串的大小， x->number， 每有一个数字就要增加五个长度
        res = [''] * expand_len
        
        new_index = expand_len - 1 # 指向扩充后字符串末尾
        old_index = len(s) - 1 # 指向原字符串末尾
        
        while old_index >= 0: # 从后往前， 遇到数字替换成“number”
            if s[old_index].isdigit():
                res[new_index-5:new_index+1] = "number"
                new_index -= 6
            else:
                res[new_index] = s[old_index]
                new_index -= 1
            old_index -= 1
        
        return "".join(res)
        
if __name__ == "__main__":
    solution = Solution()

    while True:
        try:
            s = input()
            result = solution.subsitute_numbers(s)
            print(result)
        except EOFError:
            break

```

### 手动走一遍（`"a1b2c3"`）

- `n=6, cnt=3 → expand_len=6+15=21`
- 写入顺序与位置（含区间是左闭右开）：
    1. 遇到 `3` → 写 `res[15:21] = "number"`，`new_index=14`
    2. 写 `c` 到 `res[14]`，`new_index=13`
    3. 遇到 `2` → 写 `res[8:14] = "number"`，`new_index=7`
    4. 写 `b` 到 `res[7]`，`new_index=6`
    5. 遇到 `1` → 写 `res[1:7] = "number"`，`new_index=0`
    6. 写 `a` 到 `res[0]`，`new_index=-1`
- 最终 `res` 满格：
    
    `['a','n','u','m','b','e','r','b','n','u','m','b','e','r','c','n','u','m','b','e','r']`