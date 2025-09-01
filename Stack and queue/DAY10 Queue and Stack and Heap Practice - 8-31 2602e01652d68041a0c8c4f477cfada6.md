# DAY10 Queue and Stack and Heap Practice - 8-31

# 1.  150 Evaluate Reverse Polish Notation

You are given an array of strings `tokens` that represents an arithmetic expression in a [Reverse Polish Notation](http://en.wikipedia.org/wiki/Reverse_Polish_notation).

Evaluate the expression. Return *an integer that represents the value of the expression*.

**Note** that:

- The valid operators are `'+'`, `'-'`, `'*'`, and `'/'`.
- Each operand may be an integer or another expression.
- The division between two integers always **truncates toward zero**.
- There will not be any division by zero.
- The input represents a valid arithmetic expression in a reverse polish notation.
- The answer and all the intermediate calculations can be represented in a **32-bit** integer.

**Example 1:**

```
Input: tokens = ["2","1","+","3","*"]
Output: 9
Explanation: ((2 + 1) * 3) = 9

```

**Example 2:**

```
Input: tokens = ["4","13","5","/","+"]
Output: 6
Explanation: (4 + (13 / 5)) = 6

```

**Example 3:**

```
Input: tokens = ["10","6","9","3","+","-11","*","/","*","17","+","5","+"]
Output: 22
Explanation: ((10 * (6 / ((9 + 3) * -11))) + 17) + 5
= ((10 * (6 / (12 * -11))) + 17) + 5
= ((10 * (6 / -132)) + 17) + 5
= ((10 * 0) + 17) + 5
= (0 + 17) + 5
= 17 + 5
= 22

```

**Constraints:**

- `1 <= tokens.length <= 104`
- `tokens[i]` is either an operator: `"+"`, `"-"`, `"*"`, or `"/"`, or an integer in the range `[-200, 200]`.

https://leetcode.com/problems/evaluate-reverse-polish-notation/description/

### 啥是逆序表达式？

- 逆波兰表达式是一种 **不需要括号** 的数学表达式表示法。
- 运算符写在操作数的 **后面**（所以也叫 **后缀表达式**）。
- 常见的 **中缀表达式** 是我们平时用的，比如 `3 + 4`。
- 逆波兰表达式就会写成：`3 4 +`。

中缀表达式（正常写法）：

```
(3 + 4) * 5 - 6

```

转成逆波兰表达式：

```
3 4 + 5 * 6 -

```

用 **栈（stack）** 来计算：

1. 读到数字 → 入栈
2. 读到运算符 → 弹出栈顶两个数，做运算，再把结果压回栈

过程：

- 读 `3` → 入栈 `[3]`
- 读 `4` → 入栈 `[3, 4]`
- 读 `+` → 弹出 `4` 和 `3`，算 `3+4=7` → 入栈 `[7]`
- 读 `5` → 入栈 `[7, 5]`
- 读  → 弹出 `5` 和 `7`，算 `7*5=35` → 入栈 `[35]`
- 读 `6` → 入栈 `[35, 6]`
- 读  → 弹出 `6` 和 `35`，算 `35-6=29` → 入栈 `[29]`

后缀表达式，也就是逆波兰表达式 可以不用加括号就出结果。计算机就是按这种表达式运算。

### 如何用栈来计算这个后缀表达式？

**遇到数字加入到栈里，然后遇到操作符，弹出栈里的两个数字，然后计算结果。** 

比如（1+2） * （3+4）

后缀表达式 1 2 + 3 4 + * 

顺序： 1，2 先入栈， 遇到+，弹出 计算结果 3， 再入栈

3， 4， 入栈， 此时遇到+， 计算结果7， 再入栈

然后遇到乘号，弹出两个元素 结果 3 * 7 = 21， 再入栈。 

那么最后的结果就是stack 里的唯一剩下一个元素。 

### **这道题和括号匹配思路差不多， 这道题是两个数字遇到一个符号，就消除。 就是消消乐的过程， 之前stack 的题就是 遇到match 的括号，消除，或者遇到相同的字符 消除。 栈这种数据结构很适合做相邻的结构的消除。**

### implementation details:

1. 遇到符号，我们弹出栈顶两个元素，nums1 = stack,pop() , nums2 = stack.pop() 但是遇到- 或者 除法 到底是 nums1 / nums 2 还是 nums 2 / nums1？
    - 看题目要求， intuition:  是先进栈的 数字 在expression 左边， 所以是nums2 / nums1
    - example: 是 13 / 5 ， 而不是 5 / 13
    
    ```python
    Input: tokens = ["4","13","5","/","+"]
    Output: 6
    Explanation: (4 + (13 / 5)) = 6
    ```
    

1. 题目说 The division between two integers always **truncates toward zero**.

两个整数相除后，**如果有小数部分，直接去掉**，并且结果往 0 靠近。

用int 截断小数部分

```python
int(7 / 2)     # 3
int(-7 / 2)    # -3   ✅ (toward 0)
int(6 / -132)  # 0    ✅ (toward 0)

```

1. 又搞不清楚floor division and true division 了

### `/` —— 真除法 (true division)

- **总是返回浮点数** (`float`)
- 不管除得尽不尽，结果都会带小数

```python
7 / 2     # 3.5
-7 / 2    # -3.5
6 / -132  # -0.045454545454545456

```

---

### `//` —— 地板除法 (floor division)

- **向下取整**（floor，往负无穷方向靠近）
- 结果是 **整数**（如果两边是 int）
- 对负数尤其要小心

```python
7 // 2     # 3   ✅ (floor(3.5) = 3)
-7 // 2    # -4  ⚠️ (floor(-3.5) = -4，不是 -3)
6 // -132  # -1  ⚠️ (floor(-0.045...) = -1，不是 0)

```

代码实现，其实理解了思路就好写了， 要注意 list 里面都是string, 处理数字的时候要转化成int

```python
class Solution:
    def evalRPN(self, tokens: List[str]) -> int:
        stack = []
        for s in tokens:
            if s == "+" or s =="-" or s == "*" or s == "/":
                nums1 = stack.pop()
                nums2 = stack.pop()
                if s == "+":
                    stack.append(int(nums1) + int(nums2))
                if s == "-":
                    stack.append(int(nums2) - int(nums1))
                if s == "*":
                    stack.append(int(nums1) * int(nums2))
                    print('multiplication',stack)
                if s == "/":
                    stack.append(int(int(nums2) / int(nums1)))
            else:
                stack.append(int(s))
            
          
                
                
        return int(stack.pop())
```

计算机可以利用栈来顺序处理，不需要考虑优先级了。也不用回退了， **所以后缀表达式对计算机来说是非常友好的。**

可以说本题不仅仅是一道好题，也展现出计算机的思考方式。

在1970年代和1980年代，惠普在其所有台式和手持式计算器中都使用了RPN（后缀表达式），直到2020年代仍在某些模型中使用了RPN。

---

# 2. 239 Sliding Window Maximum

[[**239. Sliding Window Maximum**](https://leetcode.com/problems/sliding-window-maximum/)](https://www.notion.so/239-Sliding-Window-Maximum-18b2e01652d6809182afce0070f5104c?pvs=21) 

You are given an array of integers `nums`, there is a sliding window of size `k` which is moving from the very left of the array to the very right. You can only see the `k` numbers in the window. Each time the sliding window moves right by one position.

Return *the max sliding window*.

**Example 1:**

```
Input: nums = [1,3,-1,-3,5,3,6,7], k = 3
Output: [3,3,5,5,6,7]
Explanation:
Window position                Max
---------------               -----
[1  3  -1] -3  5  3  6  73
 1 [3  -1  -3] 5  3  6  73
 1  3 [-1  -3  5] 3  6  7 5
 1  3  -1 [-3  5  3] 6  75
 1  3  -1  -3 [5  3  6] 76
 1  3  -1  -3  5 [3  6  7]7
```

**Example 2:**

```
Input: nums = [1], k = 1
Output: [1]

```

Brute force time complexity: O(n*k)

**问题： 能不能用线性的时间复杂度来解决这道题？**

队列：

**其实队列没有必要维护窗口里的所有元素，只需要维护有可能成为窗口里最大值的元素就可以了，同时保证队列里的元素数值是由大到小的。**

**那么这个维护元素单调递减的队列就叫做单调队列，即单调递减或单调递增的队列。** 

### 思路：

1. 在队列出口处维护最大值
2. 队列是monotonic decreasing, 如果push 进来的元素比前面的都大， 那就要弹出前面所有比这个新元素小的值， 然后再push 新数字
3. pop 队列中什么时候要弹出队头最大的那个位置？ 如果滑出窗口的数等于队头，就把队头弹掉。
4. get 队列max value就是queue 的头元素。 

这样每个元素只会 **进队一次，出队一次** → 总复杂度 `O(n)`

### 为什么pop 功能用 popleft(), push 用pop（）？

### `pop()` 从队尾remove，所以push 的时候要维护单调的特性，遇到比新元素小的数字要从队尾弹出

- Removes and returns the **last element** (right end).
- Think of it as the **stack style** (LIFO).

```python
from collections import deque

q = deque([1, 2, 3, 4])
print(q.pop())   # 4  ← removed from the right
print(q)         # deque([1, 2, 3])

```

---

### 2. `popleft()`  从队头删除， 在pop 的时候移动窗口要删除的元素等于 queue 的最大值的时候要从队头删除。

- Removes and returns the **first element** (left end).
- Think of it as the **queue style** (FIFO).

```python
q = deque([1, 2, 3, 4])
print(q.popleft())  # 1  ← removed from the left
print(q)            # d

```

### 总结

- **`popleft()`** → 用在 **窗口左端移出元素**（队头）。
- **`pop()`** → 用在 **维持单调递减队列时移除队尾**

然后遍历input list， 先把 前k 个元素push 到queue 里， append 第一个最大的值， 然后遍历剩余的list 里的元素。 

先pop， 移动k 窗口影响不影响 queue 最大的元素， 然后push 新元素， 再 append queue 队头最大的元素

```python
from collections import deque

class MyQueue: #单调队列（从大到小
    def __init__(self):
        self.queue = deque() #这里需要使用deque实现单调队列，直接使用list会超时
    
    #每次弹出的时候，比较当前要弹出的数值是否等于队列出口元素的数值，如果相等则弹出。
    #同时pop之前判断队列当前是否为空。
    def pop(self, value):
        if self.queue and value == self.queue[0]:
            self.queue.popleft()#list.pop()时间复杂度为O(n),这里需要使用collections.deque()
            
    #如果push的数值大于入口元素的数值，那么就将队列后端的数值弹出，直到push的数值小于等于队列入口元素的数值为止。
    #这样就保持了队列里的数值是单调从大到小的了。
    def push(self, value):
        while self.queue and value > self.queue[-1]:
            self.queue.pop()
        self.queue.append(value)
        
    #查询当前队列里的最大值 直接返回队列前端也就是front就可以了。
    def front(self):
        return self.queue[0]
    
class Solution:
    def maxSlidingWindow(self, nums: List[int], k: int) -> List[int]:
        que = MyQueue()
        result = []
        for i in range(k): #先将前k的元素放进队列
            que.push(nums[i])
        result.append(que.front()) #result 记录前k的元素的最大值
        for i in range(k, len(nums)):
            que.pop(nums[i - k]) #滑动窗口移除最前面元素
            que.push(nums[i]) #滑动窗口前加入最后面的元素
            result.append(que.front()) #记录对应的最大值
        return result
```

---

# 3.  347 Top K elements

https://leetcode.com/problems/top-k-frequent-elements/submissions/1570736359/

Given an integer array `nums` and an integer `k`, return *the* `k` *most frequent elements*. You may return the answer in **any order**.

**Example 1:**

**Input:** nums = [1,1,1,2,2,3], k = 2

**Output:** [1,2]

**Example 2:**

**Input:** nums = [1], k = 1

**Output:** [1]

**Example 3:**

**Input:** nums = [1,2,1,2,1,2,3,1,3,2], k = 2

**Output:** [1,2]

1. how to get the frequency? use map 
2. how to order by frequency and get the top k elements? 

# 心得

如何维护k 个 有序元素的 排序？ 用heap 那这道题用min heap 还是max heap？ 

我做过这道题 但是第一反应看之前写的代码又忘记是用min heap 还是max heap了。 

In **Python**, `heapq` is always a **min-heap** (smallest element at the top)

- 啥时候用heap？ 一个很大的数据集里用前k个高频 或者 低频的结果。
- min heap and max heap:
    - max heap: root is max .
    - min heap: root is min .

- 那这道题用min heap or max heap?
    - 如果用 max heap 维护 length 为k， 加进来一个元素， 那就要 pop 嘛，可是heap 只能从 堆顶pop，就直接把 最频繁的元素拿出去了，所以是错的. 这样子堆里留下的正好是top k least common element
    - 我们要维护的是数值大的，所以要弹出去数值小的，所以用min heap.
    - 遍历整个数组 get key as number, value as frequency, and use the frequency to construct the heap.
    
    heap 就维护 k 个元素， heap 本身是一种二叉树binary tree, so adding an element takes log(k). 
    

So overall time complexity is O(n*log(k)). 

- priority queue : A heap is a specific type
- A heap is a specific type of [tree data structure](https://www.reddit.com/search/?q=tree+data+structure+types&cId=a0505af5-28ce-4da4-909d-74d93eab6509&iId=9e3f242c-c68d-498e-bb75-58ecc86dbf8a) that is often used to implement a [priority queue](https://www.reddit.com/search/?q=priority+queue+data+structure&cId=3503daac-cbea-499c-a1ed-db33a5314096&iId=3a78239f-ac29-49ae-a6db-450b8072da28). A priority queue is a data structure that allows you to store elements with priorities, and then efficiently retrieve the element with the highest priority.

以下这个solution 没有sort， 因为题目中说要in any order. 

如果要排序：堆里是min， 所以先弹小的元素，要倒叙排一下

```python
res = []
        for pair in heap:
            res.append(pair[1])
        return res[::-1] 
```

```python
from collections import Counter
import heapq
class Solution:
    def topKFrequent(self, nums: List[int], k: int) -> List[int]:
        counts = Counter(nums)
        heap = [] # min heap
        for key, val in counts.items():
            heapq.heappush(heap, (val, key))
        while len(heap) > k:
                heapq.heappop(heap) #pop smallest element
        return [pair[1] for pair in heap]

        
```

another solution: 上面是先全进堆， 然后再弹出。

下面这个是维护长度为k 的堆，如果 长度大于k，弹出min element

然后再倒叙遍历 get most frequent k. 

```python
#时间复杂度：O(nlogk)
#空间复杂度：O(n)
import heapq
class Solution:
    def topKFrequent(self, nums: List[int], k: int) -> List[int]:
        #要统计元素出现频率
        map_ = {} #nums[i]:对应出现的次数
        for i in range(len(nums)):
            map_[nums[i]] = map_.get(nums[i], 0) + 1
        
        #对频率排序
        #定义一个小顶堆，大小为k
        pri_que = [] #小顶堆
        
        #用固定大小为k的小顶堆，扫描所有频率的数值
        for key, freq in map_.items():
            heapq.heappush(pri_que, (freq, key))
            if len(pri_que) > k: #如果堆的大小大于了K，则队列弹出，保证堆的大小一直为k
                heapq.heappop(pri_que)
        
        #找出前K个高频元素，因为小顶堆先弹出的是最小的，所以倒序来输出到数组
        result = [0] * k
        for i in range(k-1, -1, -1):
            result[i] = heapq.heappop(pri_que)[1]
        return result
```

Notes