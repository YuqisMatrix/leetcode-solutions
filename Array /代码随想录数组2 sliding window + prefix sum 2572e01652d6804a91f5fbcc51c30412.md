# 代码随想录数组2 sliding window + prefix sum

### 1. 209 Minimum Size Subarray

https://leetcode.com/problems/minimum-size-subarray-sum/

# [**209. Minimum Size Subarray Sum**](https://leetcode.com/problems/minimum-size-subarray-sum/)

Given an array of positive integers `nums` and a positive integer `target`, return *the **minimal length** of a subarray whose sum is greater than or equal to* `target`. If there is no such subarray, return `0` instead.

**Example 1:**

```
Input: target = 7, nums = [2,3,1,2,4,3]
Output: 2
Explanation: The subarray [4,3] has the minimal length under the problem constraint.

```

**Example 2:**

```
Input: target = 4, nums = [1,4,4]
Output: 1

```

**Example 3:**

```
Input: target = 11, nums = [1,1,1,1,1,1,1,1]
Output: 0

```

**Constraints:**

- `1 <= target <= 109`
- `1 <= nums.length <= 105`
- `1 <= nums[i] <= 104`

**Follow up:**

If you have figured out the

```
O(n)
```

solution, try coding another solution of which the time complexity is

```
O(n log(n))
```

### 1. Brute force

```python
class Solution:
    def minSubArrayLen(self, target: int, nums: List[int]) -> int:
        if sum(nums) < target:
            return 0
        res = float('inf')
        for i in range(len(nums)):
            sub_sum = 0
            for j in range(i, len(nums)):
                sub_sum += nums[j]
                if sub_sum >= target:
                    leng = j - i + 1
                    res = min(leng, res)
                    
        return res
```

use 2 for loops to iterate each subarray, and find the minimum size subarray with sum ≥ target. 

为什么在第一个for loop 里 要initialize sub_sum = 0? 因为要考虑array len = 1 的情况

Time complexity: O(n^2), space complexity: O(1)

### 2. Sliding Window

为啥今天没想出来？ 到底要怎么construct window? 

滑动窗口，**就是不断的调节子序列的起始位置和终止位置，从而得出我们想要的结果**

sliding window 可以用一个for loop 解决这道题。那for j in range 这个j到底是window 起始点还是终止点？

答： 终止点。如果for 循环是 循环起始点那还是和上面brute force 一样还需要再循环终止点。 

- 那么如何移动起始位置？ 终止位置随着for loop 移动。 如果sum of subarray ≥ target, then it is a valid array, so we move the start pointer, and see if the next subarray is valid.
- the main point of this problem is how to move the start pointer to collect the subarray sum
    
    
1. What is the window? 窗口就是 满足其和 ≥ s 的长度最小的 连续 子数组。
2. How to move the start of the window?  如果当前窗口的值大于等于s了，窗口就要向前移动了（也就是该缩小了）。
3. How to move the end of the window?  窗口的结束位置就是遍历数组的指针，也就是for循环里的索引。

pseudo code 

```python
function minSubArrayLen(target, nums):
    start = 0                  # left boundary of the window
    window_sum = 0             # current window sum
    min_length = infinity      # result

    for end from 0 to len(nums)-1:
        window_sum += nums[end]   # expand window to the right

        while window_sum >= target:   # shrink from the left if condition met
            current_length = end - start + 1
            min_length = min(min_length, current_length)

            window_sum -= nums[start]   # shrink window
            start += 1

    if min_length == infinity:
        return 0
    else:
        return min_length

	
```

### warning: check condition 为什么是用while 不是用if？

say [1,1,1,1,1,100] **用while 是一个持续移动的过程**， 比如j is at the end of the array, right now sum = 105, so we keep moving the start pointer until the sum of the subarray is less than the target. 

如果用if 就判断一次sum of subarray 就直接结束了，没有持续移动start 指针的过程， 判断一下subarray 是不是valid 就结束了。 

```python
class Solution:
    def minSubArrayLen(self, target: int, nums: List[int]) -> int:
        res = float('inf') #length of window
        left = 0 
        curr_sum = 0 
        for right in range(len(nums)):
            curr_sum += nums[right]
            while curr_sum >= target:
                res = min(res, right-left + 1) #update window size 
                curr_sum -= nums[left]
                left += 1
        return res if res < float('inf') else 0  
```

Time Complexity: O(n)

Space Complexity: O(1)

更新的具体步骤的顺序也很重要：

- 先check window while loop
    - update window size， 现在的窗口是valid的 所以记录窗口大小
    - 然后 先更新curr sum, 因为现在开始指针还是原来的那个， 所以先减掉 nums[left]
    - 然后再改变起始指针， 指针往后移

---

# 2. 59: Spiral Matrix 2

https://leetcode.com/problems/spiral-matrix-ii/description/

Given a positive integer `n`, generate an `n x n` `matrix` filled with elements from `1` to `n2` in spiral order.

**Example 1:**

![](https://assets.leetcode.com/uploads/2020/11/13/spiraln.jpg)

```
Input: n = 3
Output: [[1,2,3],[8,9,4],[7,6,5]]

```

**Example 2:**

```
Input: n = 1
Output: [[1]]

```

**Constraints:**

- `1 <= n <= 20`

本题目难点： 边界处理。 正方形矩阵四个角到底要怎么处理？ 要怎么遍历每一条边呢

求解本题依然是要坚持循环不变量原则。

模拟顺时针画矩阵的过程:

- 填充上行从左到右
- 填充右列从上到下
- 填充下行从右到左
- 填充左列从下到上

由外向内一圈一圈这么画下去

这道题要左闭右开。 每条边都剩最后一个节点留给下一个边处理

![](https://file1.kamacoder.com/i/algo/20220922102236.png)

这里每一种颜色，代表一条边，我们遍历的长度，可以看出每一个拐角处的处理规则，拐角处让给新的一条边来继续画。

这也是坚持了每条边左闭右开的原则。

那到底怎么实现？ 要转几圈？ 转n/2 这么多圈

如果n 是奇数字，那要最后再赋值最中间的那个格子

define variable offset: 

- `offset`：偏移量，决定本圈的“右边界/下边界”。
- 外层 `offset=1` 表示第一圈；
    
    下一次 `offset=2` 表示第二圈，依次往里收缩
    
    `startx, starty` → 当前圈的左上角起点。
    
    - **loop**：表示螺旋要绕多少“圈”。
        - 一个 `n×n` 矩阵，大概需要 `n//2` 层。
        - 比如 `n=4`，有 `2` 圈（外圈 + 内圈）。
        - 比如 `n=5`，有 `2` 圈 + 中心点。
    - **mid**：当 `n` 是奇数时，最后会剩一个中心点，这个变量用来直接定位它。

![IMG_2533.jpg](%E4%BB%A3%E7%A0%81%E9%9A%8F%E6%83%B3%E5%BD%95%E6%95%B0%E7%BB%842%20sliding%20window%20+%20prefix%20sum%202572e01652d6804a91f5fbcc51c30412/IMG_2533.jpg)

```
class Solution:
    def generateMatrix(self, n: int) -> List[List[int]]:
        startx = 0
        starty = 0
        loop = n // 2
        mid = n // 2
        count = 1 
        nums = [[0] * n for _ in range(n)]
        for offset in range(1, loop + 1):
            #filling the first row from left to right
            for i in range(starty, n - offset):
                nums[startx][i] = count 
                count += 1
            # the right col from up to down 
            for i in range(startx, n- offset):
                nums[i][n - offset] = count 
                count += 1
            # the bottom line from right to left
            for i in range(n-offset, starty, -1):
                nums[n-offset][i] = count 
                count += 1
            for i in range(n-offset, startx, -1):
                nums[i][starty] = count 
                count += 1
            #update the starting point
            startx += 1
            starty += 1
        if n % 2 == 1:
            nums[mid][mid] = count 
        return nums 
```

# 3. prefix sum

**PREFIX SUM: 前缀和 在涉及计算区间和的问题时非常有用！**

我们要统计 vec[i] 这个数组上的区间和。

我们先做累加，即 p[i] 表示 下标 0 到 i 的 vec[i] 累加 之和。

如图：

![](https://file1.kamacoder.com/i/algo/20240627110604.png)

如果，我们想统计，在vec数组上 下标 2 到下标 5 之间的累加和，那是不是就用 p[5] - p[1] 就可以了。

为什么呢？

`p[1] = vec[0] + vec[1];`

`p[5] = vec[0] + vec[1] + vec[2] + vec[3] + vec[4] + vec[5];`

`p[5] - p[1] = vec[2] + vec[3] + vec[4] + vec[5];`

如图所示：

![](https://file1.kamacoder.com/i/algo/20240627111319.png)

`p[5] - p[1]` 就是 红色部分的区间和。

而 p 数组是我们之前就计算好的累加和，所以后面每次求区间和的之后 我们只需要 O(1) 的操作。

**特别注意**： 在使用前缀和求解的时候，要特别注意 求解区间。

如上图，如果我们要求 区间下标 [2, 5] 的区间和，那么应该是 p[5] - p[1]，而不是 p[5] - p[2]。

**重点是：**

1. **先initialize prefix sum array**
2. **if we want to know sum(array from [index a to index b ]) : sum = sum_array[b] - sumarray[a-1]**

## https://programmercarl.com/kamacoder/0058.%E5%8C%BA%E9%97%B4%E5%92%8C.html#%E6%80%9D%E8%B7%AF

题目描述

给定一个整数数组 Array，请计算该数组在每个指定区间内元素的总和。

输入描述

第一行输入为整数数组 Array 的长度 n，接下来 n 行，每行一个整数，表示数组的元素。随后的输入为需要计算总和的区间，直至文件结束。

输出描述

输出每个指定区间内元素的总和。

输入示例

```
5
1
2
3
4
5
0 1
1 3

```

输出示例

```
3
9

```

数据范围：

0 < n <= 100000

### 输入

```
5          ← 数组长度 n=5
1          ← Array[0]
2          ← Array[1]
3          ← Array[2]
4          ← Array[3]
5          ← Array[4]
0 1        ← 查询区间 a=0, b=1
1 3        ← 查询区间 a=1, b=3

```

所以数组是：

```
Array = [1, 2, 3, 4, 5]

```

```python
import sys
input = sys.stdin.read

def main():
    data = input().split() #split the data first
    index = 0
    n = int(data[index]) # get the size of the array 
    index += 1
    vec = []
    for i in range(n):
        vec.append(int(data[index + i])) # get the element of the array 
    index += n

    p = [0] * n # prefix sum part
    presum = 0
    for i in range(n):
        presum += vec[i]
        p[i] = presum

    results = [] # result list 
    while index < len(data): # get the index [a,b]
        a = int(data[index])
        b = int(data[index + 1])
        index += 2

        if a == 0:
            sum_value = p[b]
        else:
            sum_value = p[b] - p[a - 1] #get the sum of between [a,b]

        results.append(sum_value)

    for result in results:
        print(result)

if __name__ == "__main__":
    main()
```

# prefix sum 在matrix中的变种

https://kamacoder.com/problempage.php?pid=1044

**44. 开发商购买土地（第五期模拟笔试）**

### **题目描述**

在一个城市区域内，被划分成了n * m个连续的区块，每个区块都拥有不同的权值，代表着其土地价值。目前，有两家开发公司，A 公司和 B 公司，希望购买这个城市区域的土地。

现在，需要将这个城市区域的所有区块分配给 A 公司和 B 公司。

然而，由于城市规划的限制，只允许将区域按横向或纵向划分成两个子区域，而且每个子区域都必须包含一个或多个区块。 为了确保公平竞争，你需要找到一种分配方式，使得 A 公司和 B 公司各自的子区域内的土地总价值之差最小。

注意：区块不可再分。

### **输入描述**

第一行输入两个正整数，代表 n 和 m。

接下来的 n 行，每行输出 m 个正整数。

### **输出描述**

请输出一个整数，代表两个子区域内土地总价值之间的最小差距。

### **输入示例**

`3 3
1 2 3
2 1 3
1 2 3`

### **输出示例**

`0`

### **提示信息**

如果将区域按照如下方式划分：

1 2 | 3

2 1 | 3

1 2 | 3

两个子区域内土地总价值之间的最小差距可以达到 0。

数据范围：

1 <= n, m <= 100；

n 和 m 不同时为 1。

## **题意翻译**

- 我们有一个 `n * m` 的矩阵，每个格子有一个权值（代表土地价值）。
- 要把整个矩阵分给 A 公司和 B 公司。
- **限制条件**：只能用 **一条横线** 或 **一条竖线** 把矩阵一分为二。
    - 不能随意切成小块。
    - 每个子区域必须至少有一行/一列。
- 目标：让两部分的价值总和的差值 **最小**。

---

## **例子**

输入：

```
3 3
1 2 3
2 1 3
1 2 3

```

矩阵是：

```
1 2 3
2 1 3
1 2 3

```

---

在第 2 列和第 3 列之间切：

```
左半部分 = (1+2+2 + 2+1+1) = 9
右半部分 = (3+3+3) = 9
差值 = |9-9| = 0

```

1. BRUTE FORCE: 每次切割都重新扫一遍 → 需要双重循环累加。time complexity o(n^3)
2. 前缀和的思路来求解，先将 行方向，和 列方向的和求出来，这样可以方便知道 划分的两个区间的和.预处理一次后，每个切割直接 O(1) 得到区域和 → 不用双重循环

核心思想： 

先把矩阵变成一行行、一列列的和，然后尝试横切或竖切，比较差值，最后取最小的。

- **Parse input → build matrix**
    - 把输入转成二维数组 `vec`，同时求出整个矩阵的 `sum`（总和）。
- **Row sums & column sums**
    - `horizontal[i]` = 第 i 行的总和
    - `vertical[j]` = 第 j 列的总和
- **Try horizontal cuts**
    - 从上往下逐行累加 → 得到「上半部分的和」
    - 用公式 `abs(sum - 2*horizontalCut)` 算差值
- **Try vertical cuts**
    - 从左往右逐列累加 → 得到「左半部分的和」
    - 同样用公式 `abs(sum - 2*verticalCut)` 算差值. why?
    - LET HORIZONTAL BE (6,6,6)
    - so suppose we wanna get the difference between row1 and (row2+ row3), it would be (sum - row1 - row1) = abs(sum - 2 * cut row)
- **Pick the minimum difference**
    - 输出所有切法中的最小差值。

```python
def main():
    import sys
    input = sys.stdin.read
    data = input().split()

    idx = 0
    n = int(data[idx])
    idx += 1
    m = int(data[idx])
    idx += 1
    sum = 0
    vec = []
    for i in range(n):
        row = []
        for j in range(m):
            num = int(data[idx])
            idx += 1
            row.append(num)
            sum += num
        vec.append(row)

    # 统计横向
    horizontal = [0] * n
    for i in range(n):
        for j in range(m):
            horizontal[i] += vec[i][j]

    # 统计纵向
    vertical = [0] * m
    for j in range(m):
        for i in range(n):
            vertical[j] += vec[i][j]

    result = float('inf')
    horizontalCut = 0
    for i in range(n):
        horizontalCut += horizontal[i]
        result = min(result, abs(sum - 2 * horizontalCut))

    verticalCut = 0
    for j in range(m):
        verticalCut += vertical[j]
        result = min(result, abs(sum - 2 * verticalCut))

    print(result)

if __name__ == "__main__":
    main()

```

English Version:

You are given an `n x m` integer matrix representing land values in a city. You want to split the matrix into **two parts** such that:

- The split must be **horizontal (between two rows)** or **vertical (between two columns)**.
- Both resulting regions must be non-empty.

Return the **minimum possible absolute difference** between the total values of the two regions.

## Approach

1. Compute the **total sum** of the matrix.
2. Precompute row sums and column sums.
3. Try every possible horizontal cut (prefix of rows) and every possible vertical cut (prefix of columns).
    - Let `prefixSum` = sum of one part.
    - The other part is `total - prefixSum`.
    - Difference = `abs(total - 2 * prefixSum)`.
4. Return the minimum difference.

```python
def minDifference(matrix):
    """
    Given an n x m matrix, split it horizontally or vertically into two
    non-empty parts. Return the minimum absolute difference of their sums.
    """
    n = len(matrix)
    m = len(matrix[0])

    # Step 1: compute total sum
    total_sum = sum(sum(row) for row in matrix)

    # Step 2: precompute row sums and column sums
    row_sums = [sum(matrix[i]) for i in range(n)]
    col_sums = [sum(matrix[i][j] for i in range(n)) for j in range(m)]

    min_diff = float('inf')

    # Step 3a: horizontal cuts (split between row r and r+1)
    prefix_rows = 0
    for r in range(n - 1):   # ensure both top and bottom are non-empty
        prefix_rows += row_sums[r]
        diff = abs(total_sum - 2 * prefix_rows)
        min_diff = min(min_diff, diff)

    # Step 3b: vertical cuts (split between column c and c+1)
    prefix_cols = 0
    for c in range(m - 1):   # ensure both left and right are non-empty
        prefix_cols += col_sums[c]
        diff = abs(total_sum - 2 * prefix_cols)
        min_diff = min(min_diff, diff)

    return min_diff

```

**Input**

```python
matrix = [
    [1, 2, 3],
    [2, 1, 3],
    [1, 2, 3]
]

```

**Output**

```
0

```

**Explanation**

- Total sum = 18
- If you split between the 2nd and 3rd columns:
    - Left part = 9
    - Right part = 9
    - Difference = 0 (minimum possible).
- **Time:** `O(n · m)`
- **Space:** `O(n + m)`