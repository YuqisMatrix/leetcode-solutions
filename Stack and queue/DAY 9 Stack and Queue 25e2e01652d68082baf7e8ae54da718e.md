# DAY 9 : Stack and Queue

# 1. Stack: Last in first Out

- **O(1) push, pop, and random access, O(n) search**
- **peek() is used in Java**

## Basic Stack Operations

1. **Push**
    
    Add an element to the **top** of the stack.
    
    ```python
    stack.append(x)  # push x
    
    ```
    
2. **Pop**
    
    Remove and return the element from the **top** of the stack.
    
    ```python
    top = stack.pop()
    
    ```
    
3. **Peek / Top**
    
    Return the element at the top **without removing it**.
    
    ```python
    top = stack[-1]  # just look, don’t pop
    
    ```
    
4. **isEmpty**
    
    Check if the stack is empty.
    
    ```python
    len(stack) == 0   # True if empty
    
    ```
    
5. **Size**
    
    Get the number of elements in the stack.
    
    ```python
    size = len(stack)
    
    ```
    

---

## 🔹 Example in Python

```python
stack = []

# Push
stack.append(1)
stack.append(2)
stack.append(3)   # stack = [1, 2, 3]

# Peek
print(stack[-1])  # 3

# Pop
print(stack.pop())  # 3, stack = [1, 2]

# Size
print(len(stack))   # 2

# isEmpty
print(len(stack) == 0)  # False

```

---

# 2. Queue: First in First out

### 1. First in first out, 队列 先进先出

### **Advantages of a Queue:**

### 1. **Constant Time Operations (O(1)):**

- **Adding an Element (`enqueue`)**:
    - In most queue implementations (e.g., with `deque` or a linked list), adding an element at the back of the queue takes constant time.
- **Removing an Element (`dequeue`)**:
    - Removing an element from the front of the queue also takes constant time.

There is also a data structure called a **deque**, short for double-ended queue, and pronounced "deck". In a deque, you can add or delete elements from both ends. A normal queue designates adding to one end and deleting to another end.

### 2. Implementation

import different mode uses different methods

- Generally, we use **deque** module
- **Operations**: ALL IN CONSTANT TIME
    - `append()` is used to add an element to the **right** (end) of the deque. 从队尾加入元素
    - `appendleft()` is used to add an element to the **left** (front) of the deque.
    - `popleft()` is used to remove an element from the **left** (front). 删除队头元素并且返回它
    - `pop()` is used to remove an element from the **right** (end). 删除队尾元素并且返回

## Basic Queue Operations

1. **Enqueue (add)**
    
    Add an element to the **back (rear)** of the queue.
    
    ```python
    from collections import deque
    q = deque()
    q.append(x)   # enqueue x
    
    ```
    
2. **Dequeue (remove)**
    
    Remove and return the element from the **front** of the queue.
    
    ```python
    front = q.popleft()
    
    ```
    
3. **Peek / Front**
    
    Return the element at the **front** without removing it.
    
    ```python
    front = q[0]
    
    ```
    
4. **isEmpty**
    
    Check if the queue is empty.
    
    ```python
    len(q) == 0
    
    ```
    
5. **Size**
    
    Get the number of elements in the queue.
    
    ```python
    size = len(q)
    
    ```
    

---

## 🔹 Example in Python

```python
from collections import deque

q = deque()

# Enqueue
q.append(1)
q.append(2)
q.append(3)   # queue = [1, 2, 3]

# Peek
print(q[0])   # 1

# Dequeue
print(q.popleft())  # 1, queue = [2, 3]

# Size
print(len(q))   # 2

# isEmpty
print(len(q) == 0)  # False

```

---

# 3. 232 **Implement Queue using Stacks**

https://leetcode.com/problems/implement-queue-using-stacks/description/

Implement a first in first out (FIFO) queue using only two stacks. The implemented queue should support all the functions of a normal queue (`push`, `peek`, `pop`, and `empty`).

Implement the `MyQueue` class:

- `void push(int x)` Pushes element x to the back of the queue.
- `int pop()` Removes the element from the front of the queue and returns it.
- `int peek()` Returns the element at the front of the queue.
- `boolean empty()` Returns `true` if the queue is empty, `false` otherwise.

**Notes:**

- You must use **only** standard operations of a stack, which means only `push to top`, `peek/pop from top`, `size`, and `is empty` operations are valid.
- Depending on your language, the stack may not be supported natively. You may simulate a stack using a list or deque (double-ended queue) as long as you use only a stack's standard operations.

**Example 1:**

```
Input
["MyQueue", "push", "push", "peek", "pop", "empty"]
[[], [1], [2], [], [], []]
Output
[null, null, null, 1, 1, false]

Explanation
MyQueue myQueue = new MyQueue();
myQueue.push(1); // queue is: [1]
myQueue.push(2); // queue is: [1, 2] (leftmost is front of the queue)
myQueue.peek(); // return 1
myQueue.pop(); // return 1, queue is [2]
myQueue.empty(); // return false
```

# 心得：

1. 这道题用两个stack 模拟 queue 先进先出的模式。 两个stack：in stack and out stack。 
2. 在push数据的时候，只要数据放进输入栈就好
3. 在pop 的时候，我们需要先
    - **判断 当前出栈是否为空，** **（这一次做忘记做出栈的分类讨论了，一股脑把入栈元素全加进去了，然后再弹，但是当出栈不为空直接弹出才是对的）**
        - 如果为空： 那需要先把入栈的元素都加进出栈里， 然后再弹**出栈**顶元素。 比如进栈元素【1，2，3】， 此时3 为栈顶， 那么出栈元素【3，2，1】， 1 是栈顶。
        - 如果出栈不为空， 那么直接弹出出栈顶元素！
4. peek，只看， 不弹出，队列元素。其实和上面的pop逻辑一样，另外一种写法更简洁，就是直接调用pop(). 
- `peek` 的 `self.pop()` 先把队首元素 **真的取出来**；
- 但为了不破坏队列状态，又 `append` 回 出栈；
- 所以最终返回的是队首元素，但队列不变。

**那么问题来了**： 为啥 队列自己pop， 然后又append 回出栈就不变啊？ intuition：队列自己pop，是从出栈pop， 那就是拿走栈顶元素， 那再append 回去，相当于装回去，没变。

假设 `stack_out = [3, 2, 1]` （队列实际内容是 `[1, 2, 3]`）。

1. **pop**
    
    ```python
    ans = self.stack_out.pop()
    
    ```
    
    - 弹出 `1`，现在 `stack_out = [3, 2]`。
2. **append**
    
    ```python
    self.stack_out.append(ans)
    
    ```
    
    - 把 `1` 放回去，`stack_out = [3, 2, 1]`。

结果：队列恢复原样

1. 什么时候queue 是空：`只要in或者out有元素，说明队列不为空`。 

```python
class MyQueue:

    def __init__(self):
        self.enquestack = []
        self.dequestack = []
        

    def push(self, x: int) -> None:
        self.enquestack.append(x)
        

    def pop(self) -> int:
        # put elements in enquetack to dequestack 
        #!!!如果出栈为空，那么要把入栈放进出栈
        if self.empty():
            return None 
        if self.dequestack:
            return self.dequestack.pop()
        else:
            for i in range(len(self.enquestack)):
                self.dequestack.append(self.enquestack.pop())
            return self.dequestack.pop()
        

    def peek(self) -> int:
        res = self.pop()
        self.dequestack.append(res)
        return res
        

    def empty(self) -> bool:
        return len(self.enquestack) == 0 and len(self.dequestack) == 0
```

---

# 4. Implementing Stack using queue

https://leetcode.com/problems/implement-stack-using-queues/description/

Implement a last-in-first-out (LIFO) stack using only two queues. The implemented stack should support all the functions of a normal stack (`push`, `top`, `pop`, and `empty`).

Implement the `MyStack` class:

- `void push(int x)` Pushes element x to the top of the stack.
- `int pop()` Removes the element on the top of the stack and returns it.
- `int top()` Returns the element on the top of the stack.
- `boolean empty()` Returns `true` if the stack is empty, `false` otherwise.

**Notes:**

- You must use **only** standard operations of a queue, which means that only `push to back`, `peek/pop from front`, `size` and `is empty` operations are valid.
- Depending on your language, the queue may not be supported natively. You may simulate a queue using a list or deque (double-ended queue) as long as you use only a queue's standard operations.

**Example 1:**

```
Input
["MyStack", "push", "push", "top", "pop", "empty"]
[[], [1], [2], [], [], []]
Output
[null, null, null, 2, 2, false]

Explanation
MyStack myStack = new MyStack();
myStack.push(1);
myStack.push(2);
myStack.top(); // return 2
myStack.pop(); // return 2
myStack.empty(); // return False
```

**队列模拟栈，其实一个队列就够了.** 

### Use 2 queues to implement stack

Always make sure that the **most recent element pushed** ends up at the **front of one queue**, so that we can `pop()` it directly.

- `queue_in`：平时存所有数据，**push 的时候直接加进去**。
- `queue_out`：辅助队列，**只在 pop 或 top 时使用**。

`pop()`

核心逻辑：

1. 把 `queue_in` 的前 `n-1` 个元素依次出列，放进 `queue_out`。
    - 这样 `queue_in` 最后剩下的那个，就是 **最新 push 的** → 也就是栈顶。
2. 交换 `queue_in` 和 `queue_out`（让 `queue_in` 重新作为主存储）。
3. `popleft()` 取出 `queue_out` 里唯一的元素（就是栈顶）。

这样就模拟出了 **LIFO**

```python
from collections import deque

class MyStack:

    def __init__(self):
        """
        Python普通的Queue或SimpleQueue没有类似于peek的功能
        也无法用索引访问，在实现top的时候较为困难。

        用list可以，但是在使用pop(0)的时候时间复杂度为O(n)
        因此这里使用双向队列，我们保证只执行popleft()和append()，因为deque可以用索引访问，可以实现和peek相似的功能

        in - 存所有数据
        out - 仅在pop的时候会用到
        """
        self.queue_in = deque()
        self.queue_out = deque()

    def push(self, x: int) -> None:
        """
        直接append即可
        """
        self.queue_in.append(x)

    def pop(self) -> int:
        """
        1. 首先确认不空
        2. 因为队列的特殊性，FIFO，所以我们只有在pop()的时候才会使用queue_out
        3. 先把queue_in中的所有元素（除了最后一个），依次出列放进queue_out
        4. 交换in和out，此时out里只有一个元素
        5. 把out中的pop出来，即是原队列的最后一个
        
        tip：这不能像栈实现队列一样，因为另一个queue也是FIFO，如果执行pop()它不能像
        stack一样从另一个pop()，所以干脆in只用来存数据，pop()的时候两个进行交换
        """
        if self.empty():
            return None

        for i in range(len(self.queue_in) - 1):
            self.queue_out.append(self.queue_in.popleft())
        
        self.queue_in, self.queue_out = self.queue_out, self.queue_in    # 交换in和out，这也是为啥in只用来存
        return self.queue_out.popleft()

    def top(self) -> int:
        """
        写法一：
        1. 首先确认不空
        2. 我们仅有in会存放数据，所以返回第一个即可（这里实际上用到了栈）
        写法二：
        1. 首先确认不空
        2. 因为队列的特殊性，FIFO，所以我们只有在pop()的时候才会使用queue_out
        3. 先把queue_in中的所有元素（除了最后一个），依次出列放进queue_out
        4. 交换in和out，此时out里只有一个元素
        5. 把out中的pop出来，即是原队列的最后一个，并使用temp变量暂存
        6. 把temp追加到queue_in的末尾
        """
        # 写法一：
        # if self.empty():
        #     return None
        
        # return self.queue_in[-1]    # 这里实际上用到了栈，因为直接获取了queue_in的末尾元素

        # 写法二：
        if self.empty():
            return None

        for i in range(len(self.queue_in) - 1):
            self.queue_out.append(self.queue_in.popleft())
        
        self.queue_in, self.queue_out = self.queue_out, self.queue_in 
        temp = self.queue_out.popleft()   
        self.queue_in.append(temp)
        return temp

    def empty(self) -> bool:
        """
        因为只有in存了数据，只要判断in是不是有数即可
        """
        return len(self.queue_in) == 0
```

## Using one queue to implement stack:

**一个队列在模拟栈弹出元素的时候只要将队列头部的元素（除了最后一个元素外） 重新添加到队列尾部，此时再去弹出元素就是栈的顺序了.** 

pop（） 的过程：**队尾的元素 就是栈顶的元素**

- 比如我们的stack：【1，2，3】.栈顶元素是3， 我们想pop 3，
- queue 里的元素：【1，2，3】，那怎么弹出3 呢？也就是队尾的元素。我们把队尾之前的元素弹出去，再append回队尾， 然后弹出最后一个元素3.
- top 就是 队尾的元素

```python
class MyStack:
    from collections import deque

    def __init__(self):
        self.q = deque()
        

    def push(self, x: int) -> None:
        self.q.append(x)

    def pop(self) -> int:
        length = len(self.q)
        print(length)
        for _ in range(length-1):
            x = self.q.popleft()
            self.q.append(x)
        return self.q.popleft()
            
        

    def top(self) -> int:
        return self.q[-1]

        

    def empty(self) -> bool:
        return len(self.q) == 0
```

---

# 5. 20 Valid Parentheses

https://leetcode.com/problems/valid-parentheses/

Given a string `s` containing just the characters `'('`, `')'`, `'{'`, `'}'`, `'['` and `']'`, determine if the input string is valid.

An input string is valid if:

1. Open brackets must be closed by the same type of brackets.
2. **Open brackets must be closed in the correct order.**
3. Every close bracket has a corresponding open bracket of the same type.

**Example 1:**

**Input:** s = "()"

**Output:** true

**Example 2:**

**Input:** s = "()[]{}"

**Output:** true

**Example 3:**

**Input:** s = "(]"

**Output:** false

**Example 4:**

**Input:** s = "([])"

**Output:** true

**Example 5:**

**Input:** s = "([)]"

**Output:** false

**Constraints:**

- `1 <= s.length <= 104`
- `s` consists of parentheses only `'()[]{}'`.

# 心得

1. 这道题为什么可以用栈？ 因为题目中说order matters, 也就是越往后的左括号要越先被合上。那就是模拟栈的last in first out， 后进栈的括号要先被弹出去。 核心思想是遇到左括号， 把对应的右括号放进stack 里。当遍历到右括号看是否和栈顶元素pop 相对应，不对应就false，对应就pop 栈顶元素，继续遍历。
2. 到底有几种情况return false？ 可以先进行剪枝：if len(s) is odd, return false. 
    - case 1: left parentheses leftover: 对应的是遍历完string，stack 不为空
    - mismatch， 当遍历到右括号，和栈顶弹出的左括号不是对应的，case2
    - case3: 右边括号多余，在遍历中栈都空了，对应的左括号都被pop掉了，还有没被处理的右括号
3. 什么时候return true呢？遍历结束了，stack 为空，说明左右括号完全匹配。

### 思路： 用栈模拟3种括号不匹配的情况：

1. 第一种情况，字符串里左方向的括号多余了 ，所以不匹配。
    
    ![括号匹配1](https://file1.kamacoder.com/i/algo/2020080915505387.png)
    
2. 第二种情况，括号没有多余，但是 括号的类型没有匹配上。
    
    ![括号匹配2](https://file1.kamacoder.com/i/algo/20200809155107397.png)
    
3. 第三种情况，字符串里右方向的括号多余了，所以不匹配。
    
    ![括号匹配3](https://file1.kamacoder.com/i/algo/20200809155115779.png)
    

遇到一个左括号，把相对应的右括号加入到栈里。 

比如**第一个情况**，栈里就是【 ), ], }, 】. 然后遍历input 里面右括号， 找到栈顶相对的，弹出，最后栈里只有一个 ）。 发现有没match 的元素，return false。多了一个左括号

比如第二个情况， 先放），】， }, into the stack. and 遍历右括号，遍历到第二个}， 发现mismatch，应该是]. 

第三个情况：放 ), ], }.  发现遍历到右括号，栈全弹出来了为空，但是input 里还有没处理完的右括号。 不匹配

```python
class Solution(object):
    def isValid(self, s):
        """
        :type s: str
        :rtype: bool
        """
        stack = []
        if len(s) % 2 == 1: #of len is odd, return false
            return False 
        for i in range(len(s)):
            if s[i] == '(':
                stack.append(')')
            elif s[i] == '[':
                stack.append(']')
            elif s[i] == '{':
                stack.append('}') #if we encounter a left paretheses, 
                #push the right one to the stack
            elif  not stack or s[i] != stack[-1]:#case 3 and 2: 
                return False 
            else: #when stack top == s[i]
                stack.pop()
        if stack: #more left paretheses
            return False
        return True

```

第一种情况：已经遍历完了字符串，但是栈不为空，说明有相应的左括号没有右括号来匹配，所以return false

第二种情况：遍历字符串匹配的过程中，发现栈里没有要匹配的字符。所以return false

第三种情况：遍历字符串匹配的过程中，栈已经为空了，没有匹配的字符了，说明右括号没有找到对应的左括号return false

**那么什么时候说明左括号和右括号全都匹配了呢，就是字符串遍历完之后，栈是空的，就说明全都匹配了**。

这行要先判断stack 是不是为空，不然会报错

```python
elif  not stack or s[i] != stack[-1]
```

---

### 2. use dictionary to match parethesis

这个是之前做过的题， 为什么可以用stack？ 因为stack last in first out. 最重要的是题目中说 **Open brackets must be closed in the correct order. 这就意味着在栈顶的元素，就是在string里靠后的左括号，就是要先被右括号给合上。** 

counter example：

 s = "( [ ) ]"

最后要注意： **当遍历完string， stack 为空， 表示left 括号都pop 掉，才表示所有括号都match**。 如果遍历完stack不为空，那还有剩余的左括号没有匹配， 多出来的，也就是case1 ， return false。 这次写把这个情况给忘记了。 

```python
class Solution(object):
    def isValid(self, s):
        """
        :type s: str
        :rtype: bool
        """
        stack = []
        mapping = {
            '(': ')',
            '[': ']',
            '{': '}'
        }
        for item in s:
            if item in mapping.keys():
                stack.append(mapping[item])
            elif not stack or stack[-1] != item: 
                return False
            else: 
                stack.pop()
        #after interation, remember to check if stack has leftover element 
        if stack:
            return False 
        return True
```

---

# 6. **Remove All Adjacent Duplicates In String**

https://leetcode.com/problems/remove-all-adjacent-duplicates-in-string/submissions/1753065273/

You are given a string `s` consisting of lowercase English letters. A **duplicate removal** consists of choosing two **adjacent** and **equal** letters and removing them.

We repeatedly make **duplicate removals** on `s` until we no longer can.

Return *the final string after all such duplicate removals have been made*. It can be proven that the answer is **unique**.

**Example 1:**

```
Input: s = "abbaca"
Output: "ca"
Explanation:
For example, in "abbaca" we could remove "bb" since the letters are adjacent and equal, and this is the only possible move.  The result of this move is that the string is "aaca", of which only "aa" is possible, so the final string is "ca".

```

**Example 2:**

```
Input: s = "azxxzy"
Output: "ay"
```

# 心得：

这道题和上道题思路类似。

1. 栈里存放什么？ 存遍历过的元素。
2. 我们要remove 相临元素，那就遍历元素的时候 peek 一下栈顶，如果当前元素和栈顶元素一样，那就pop栈顶，继续遍历
3. 如果当前元素不和栈顶元素一样怎么办？加入栈。
4. 最后遍历完了，栈里的元素就是没match 的元素

```python
class Solution(object):
    def removeDuplicates(self, s):
        """
        :type s: str
        :rtype: str
        """
        if not s or len(s) < 2:
            return False
        #print(s)
        stack = []
        stack.append(s[0])
        for i in range(1,len(s)):
            if stack and s[i] == stack[-1]:
                stack.pop()
            else:
                stack.append(s[i])
        return "".join(stack)

```

solution2: 2 pointers。 这个需要再理解一下

```python
# 方法二，使用双指针模拟栈，如果不让用栈可以作为备选方法。
class Solution:
    def removeDuplicates(self, s: str) -> str:
        res = list(s)
        slow = fast = 0
        length = len(res)

        while fast < length:
            # 如果一样直接换，不一样会把后面的填在slow的位置
            res[slow] = res[fast]
            
            # 如果发现和前一个一样，就退一格指针
            if slow > 0 and res[slow] == res[slow - 1]:
                slow -= 1
            else:
                slow += 1
            fast += 1
            
        return ''.join(res[0: slow])
```