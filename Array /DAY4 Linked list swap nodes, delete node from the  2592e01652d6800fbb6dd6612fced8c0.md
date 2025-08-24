# DAY4 Linked list: swap nodes, delete node from the end

# 1. Swap 2 nodes

https://leetcode.com/problems/swap-nodes-in-pairs/submissions/1746206385/

Given a linked list, swap every two adjacent nodes and return its head. You must solve the problem without modifying the values in the list's nodes (i.e., only nodes themselves may be changed.)

**Example 1:**

**Input:** head = [1,2,3,4]

**Output:** [2,1,4,3]

**Explanation:**

![](https://assets.leetcode.com/uploads/2020/10/03/swap_ex1.jpg)

**Example 2:**

**Input:** head = []

**Output:** []

**Example 3:**

**Input:** head = [1]

**Output:** [1]

**Example 4:**

**Input:** head = [1,2,3]

**Output:** [2,1,3]

# 心得和注意事项

这次重新做了一遍，知道用nxt 和 first 储存下一个cycle 的两个节点。但是还没写对, test case 第一个没完全过， 我哪里错了呢？ 

```python
class Solution:
    def swapPairs(self, head: Optional[ListNode]) -> Optional[ListNode]:
        if not head:
            return 
        dummy = ListNode(0)
        dummy.next = head 
        prev = dummy 
        curr = head
        while curr.next and curr.next.next:
            #print(curr.val)
            nxt = curr.next.next 
            first = curr.next 
            #step1
            prev.next = first 
            #step2 
            first.next = curr
            curr.next = nxt
            prev = curr
            curr = nxt
        return dummy.next

```

![Screenshot 2025-08-23 at 10.42.16 PM.png](DAY4%20Linked%20list%20swap%20nodes,%20delete%20node%20from%20the%20%202592e01652d6800fbb6dd6612fced8c0/Screenshot_2025-08-23_at_10.42.16_PM.png)

答案是我的while loop 循环 结束的条件没写对。为什么没写对？因为我还是没完全掌握指针的修改方式：

比如当前linkedlist dummy → node 1 → node 2 →node3 →node 4. 

如果我们要修改node1 and node 2 这两个的指针，那我们一定要知道前一个node 的指向 因为prev node points to node 2. 所以还是**和昨天的问题一样， 我们当前的node （虽然code 里叫prev），一定是要指向要*反转的node 的前一个node的指针***

所以这个code while 终止的条件是while [prev.next](http://prev.next) and [prev.next.next](http://prev.next.next). 这样才会swap node 3 and node 4. 

- follow up question: 如果要swap node 3 and node 4, 当前操作指针指向哪个node？

第二个node。 

剩下的就是反转的过程， 如何让dummy → node 1 → node 2 →node3 →node 4， 变成[2,1,4,3]？ 

我们管当前指针叫做curr，从dummy 开始

### **swap 两个node 有三个步骤：比如swap node 1 and 2**

1. curr points to node 2. 
2. node 2 points to node 1. 
3. node 1 points to node 3. 

初始时，cur指向虚拟头结点，然后进行如下三步：

![24.两两交换链表中的节点1](https://file1.kamacoder.com/i/algo/24.%E4%B8%A4%E4%B8%A4%E4%BA%A4%E6%8D%A2%E9%93%BE%E8%A1%A8%E4%B8%AD%E7%9A%84%E8%8A%82%E7%82%B91.png)

操作之后，链表如下：

![24.两两交换链表中的节点2](https://file1.kamacoder.com/i/algo/24.%E4%B8%A4%E4%B8%A4%E4%BA%A4%E6%8D%A2%E9%93%BE%E8%A1%A8%E4%B8%AD%E7%9A%84%E8%8A%82%E7%82%B92.png)

![24.两两交换链表中的节点3](https://file1.kamacoder.com/i/algo/24.%E4%B8%A4%E4%B8%A4%E4%BA%A4%E6%8D%A2%E9%93%BE%E8%A1%A8%E4%B8%AD%E7%9A%84%E8%8A%82%E7%82%B93.png)

实现步骤： 

1. dummy head points to head 
2. curr node points to dummy (上面的code 里 这个variable 叫prev， 都是一样的道理。 **重要的是为什么从dummy 开始呢？ 因为我们要操作的node 是前两个**)
3. iteration process. 遍历过程什么时候停止呢？ 本题关键
    - 如果node num is even, if [curr.next](http://curr.next) is null, end.
        - **Input:** head = [1,2,3,4] **Output:** [2,1,4,3]， curr 从dummy 指向2 指向4， 4 后面没有node 了，遍历结束。
    - 如果node num is odd, if [curr.next.next](http://curr.next.next) is null, end.
        - **Input:** head = [1,2,3], **Output:** [2,1,3] 我们就交换前两个node， when curr points to node 2, [cur.next.next](http://cur.next.next) 是空， 留下node3 不动就可以
4. while [curr.next](http://curr.next) 不为空 **and** [curr.next.next](http://curr.next.next) 不为空
    
      swap:
    
    - 
        - temp = curr. next (node1)
        - temp1 = [curr.](http://curr.next) next . next . next (node 3) 新的pair 开始
        - 步骤1 ：curr 指向 node 2:
        
        ```python
        curr.next = curr.next.next
        ```
        
        - 步骤2: node 2 指向 node 1:
        
        ```python
        curr.next.next = temp 
        ```
        
        - 步骤3: node 1 指向 node 3:
        
        ```python
        temp.next = temp1
        ```
        
        - 移动指针： curr 往后移动两个 [到curr.next.next](http://到curr.next.next) , **也就是处理过的pair 的第二个node，也就是未处理node  pair 的前一个 node**

最重要的是遍历的终止条件： curr 在未处理的pair 之前。 如果node num is even, 那么判断下一个node 为空，如果是odd 那就是next next 为空

- 时间复杂度：O(n)
- 空间复杂度：O(1)

```python
# Definition for singly-linked list.
# class ListNode:
#     def __init__(self, val=0, next=None):
#         self.val = val
#         self.next = next
class Solution:
    def swapPairs(self, head: Optional[ListNode]) -> Optional[ListNode]:
        if not head:
            return 
        dummy = ListNode(0)
        dummy.next = head 
        curr = dummy 
        while curr.next and curr.next.next:
            first = curr.next 
            third = curr.next.next.next 
            curr.next = curr.next.next #curr node points to second node 
            curr.next.next = first #second node points to first node 
            first.next = third #first node points to third node
            curr = curr.next.next 
        return dummy.next
```

---

# 2. Remove nth node from the end of a Linked list

https://leetcode.com/problems/remove-nth-node-from-end-of-list/submissions/1746269952/

Given the `head` of a linked list, remove the `nth` node from the end of the list and return its head.

**Example 1:**

![](https://assets.leetcode.com/uploads/2020/10/03/remove_ex1.jpg)

```
Input: head = [1,2,3,4,5], n = 2
Output: [1,2,3,5]

```

**Example 2:**

```
Input: head = [1], n = 1
Output: []

```

**Example 3:**

```
Input: head = [1,2], n = 1
Output: [1]
```

# 心得与注意事项

1. 这道题之前做过 可以想到用快慢指针处理， 先让快指针提前走， 然后快慢指针同时走， 快指针走到最后一个node ， 慢指针走到要删除的node 前一个。 然后再做delete 处理
2. 没处理好的： 这两个指针从哪个node 开始走？ head还是dummy
3. 快指针具体要提前走多少步？ 

链表问题中我们指针一定要走到要处理的节点前的一个节点，比如下面例子里面我们要删除3， 那就要让slow 走到2。

- 定义fast指针和slow指针，初始值为虚拟头结点，如图：

![](https://file1.kamacoder.com/i/algo/19.%E5%88%A0%E9%99%A4%E9%93%BE%E8%A1%A8%E7%9A%84%E5%80%92%E6%95%B0%E7%AC%ACN%E4%B8%AA%E8%8A%82%E7%82%B9.png)

- fast首先走n + 1步 ，为什么是n+1呢，因为只有这样同时移动的时候slow才能指向删除节点的上一个节点（方便做删除操作），如图：
    
    ![](https://file1.kamacoder.com/i/algo/19.%E5%88%A0%E9%99%A4%E9%93%BE%E8%A1%A8%E7%9A%84%E5%80%92%E6%95%B0%E7%AC%ACN%E4%B8%AA%E8%8A%82%E7%82%B91.png)
    
- fast和slow同时移动，直到fast指向末尾，如题：
    
    ![](https://file1.kamacoder.com/i/algo/19.%E5%88%A0%E9%99%A4%E9%93%BE%E8%A1%A8%E7%9A%84%E5%80%92%E6%95%B0%E7%AC%ACN%E4%B8%AA%E8%8A%82%E7%82%B92.png)
    

**slow 和 fast 都从 dummy 出发， head 先走n + 1 步**

**因为只有这样同时移动的时候slow才能指向删除节点的上一个节点（方便做删除操作）**

- 问题： 先走n+ 1 步骤 是for n in range(n) 还是 n + 1？

```python
for i in range(n+1):
    fast = fast.next

```

当range （）的数字 是0， the loop does not execute. 

range(x)： x 是多少， loop 就执行几次： `n = 3`，那 `range(n+1) = range(4)`，也就是 `0, 1, 2, 3`

```python
class Solution:
    def removeNthFromEnd(self, head: Optional[ListNode], n: int) -> Optional[ListNode]:
        if not head:
            return 
        dummy = ListNode(0)
        dummy.next = head
        slow = dummy 
        fast = dummy 
        for _ in range(n):
            fast = fast.next 
        #print(fast.val)
        while fast.next:
            slow = slow.next 
            fast = fast.next 
        #print(slow.val)
        slow.next = slow.next.next 
        return dummy.next
```

---

# 3. Intersection of Two Linked lists

https://leetcode.com/problems/intersection-of-two-linked-lists/description/

Given the heads of two singly linked-lists `headA` and `headB`, return *the node at which the two lists intersect*. If the two linked lists have no intersection at all, return `null`.

For example, the following two linked lists begin to intersect at node `c1`:

![](https://assets.leetcode.com/uploads/2021/03/05/160_statement.png)

The test cases are generated such that there are no cycles anywhere in the entire linked structure.

**Note** that the linked lists must **retain their original structure** after the function returns.

**Custom Judge:**

The inputs to the **judge** are given as follows (your program is **not** given these inputs):

- `intersectVal` - The value of the node where the intersection occurs. This is `0` if there is no intersected node.
- `listA` - The first linked list.
- `listB` - The second linked list.
- `skipA` - The number of nodes to skip ahead in `listA` (starting from the head) to get to the intersected node.
- `skipB` - The number of nodes to skip ahead in `listB` (starting from the head) to get to the intersected node.

The judge will then create the linked structure based on these inputs and pass the two heads, `headA` and `headB` to your program. If you correctly return the intersected node, then your solution will be **accepted**.

**Example 1:**

![](https://assets.leetcode.com/uploads/2021/03/05/160_example_1_1.png)

```
Input: intersectVal = 8, listA = [4,1,8,4,5], listB = [5,6,1,8,4,5], skipA = 2, skipB = 3
Output: Intersected at '8'
Explanation: The intersected node's value is 8 (note that this must not be 0 if the two lists intersect).
From the head of A, it reads as [4,1,8,4,5]. From the head of B, it reads as [5,6,1,8,4,5]. There are 2 nodes before the intersected node in A; There are 3 nodes before the intersected node in B.
- Note that the intersected node's value is not 1 because the nodes with value 1 in A and B (2nd node in A and 3rd node in B) are different node references. In other words, they point to two different locations in memory, while the nodes with value 8 in A and B (3rd node in A and 4th node in B) point to the same location in memory.

```

**Example 2:**

![](https://assets.leetcode.com/uploads/2021/03/05/160_example_2.png)

```
Input: intersectVal = 2, listA = [1,9,1,2,4], listB = [3,2,4], skipA = 3, skipB = 1
Output: Intersected at '2'
Explanation: The intersected node's value is 2 (note that this must not be 0 if the two lists intersect).
From the head of A, it reads as [1,9,1,2,4]. From the head of B, it reads as [3,2,4]. There are 3 nodes before the intersected node in A; There are 1 node before the intersected node in B.

```

**Example 3:**

![](https://assets.leetcode.com/uploads/2021/03/05/160_example_3.png)

```
Input: intersectVal = 0, listA = [2,6,4], listB = [1,5], skipA = 3, skipB = 2
Output: No intersection
Explanation: From the head of A, it reads as [2,6,4]. From the head of B, it reads as [1,5]. Since the two lists do not intersect, intersectVal must be 0, while skipA and skipB can be arbitrary values.
Explanation: The two lists do not intersect, so return null.

```

给你两个单链表的头节点 headA 和 headB ，请你找出并返回两个单链表相交的起始节点。如果两个链表没有交点，返回 null 。

求两个链表交点节点的**指针, *交点不是数值相等，而是指针相等***

![面试题02.07.链表相交_1](https://file1.kamacoder.com/i/algo/%E9%9D%A2%E8%AF%95%E9%A2%9802.07.%E9%93%BE%E8%A1%A8%E7%9B%B8%E4%BA%A4_1.png)

我们求出两个链表的长度，并求出两个链表长度的差值，然后让curA移动到，和curB 末尾对齐的位置，如图：

![面试题02.07.链表相交_2](https://file1.kamacoder.com/i/algo/%E9%9D%A2%E8%AF%95%E9%A2%9802.07.%E9%93%BE%E8%A1%A8%E7%9B%B8%E4%BA%A4_2.png)

此时我们就可以比较curA和curB是否相同，如果不相同，同时向后移动curA和curB，如果遇到curA == curB，则找到交点。

否则循环退出返回空指针。

```python
class Solution:
    def getIntersectionNode(self, headA: ListNode, headB: ListNode) -> Optional[ListNode]:
        if not headA or not headB:
            return 
        lenA = 0
        lenB = 0
        curr = headA
        while curr:
            curr = curr.next 
            lenA += 1
        curr = headB
        while curr:
            curr = curr.next 
            lenB += 1
        curA, curB = headA, headB 
        if lenA > lenB:
            curB, curA = headA, headB
            lenB, lenA = lenA, lenB
        for _ in range(lenB-lenA):
            curB = curB.next 
        while curA:
            if curA == curB:
                return curA
            else:
                curA = curA.next 
                curB = curB.next 
        return 
```

# 为什么是比较指针相等？不是值相等？

### 1. 链表节点的本质

在 Python 里，链表节点 `ListNode` 是一个对象，每个节点有两个属性：

- `val`：保存数据的值
- `next`：指向下一个节点（另一个对象的引用）

所以：

- **指针相等**：`curA is curB` 或者 `curA == curB`（因为 Python 默认类没重载 `__eq__`，所以 `==` 比较的就是对象身份），意思是“两个变量指向**同一个内存地址的同一个节点对象**”。
- **值相等**：`curA.val == curB.val`，意思是“两个节点存的数据内容一样”，但它们可能是完全不同的节点。

---

### 2. 这题为什么要比较“指针相等”

题目要求的是：**两个链表是否“相交”**。

所谓“相交”，不是说有两个节点的值一样，而是 **两个链表在某个节点开始，共享接下来的整段链表**。

比如：

```
A: 1 → 2 → 3 ↘
                 7 → 8
B:       4 → 5 ↗

```

- 链表 A 和 B 在节点 `7` 开始相交。
- 也就是说，`7` 和 `8` 这两个节点在内存里是**同一个对象**，A 和 B 都指向它。

如果你只比较 `.val`，会误判。

比如：

```
A: 1 → 2 → 3
B: 4 → 5 → 3

```

这里虽然 `val = 3` 出现了两次，但它们是两个不同的 `ListNode(3)` 对象，**没有相交**。

---

### 3. 总结

- **值相等 (`val`)**：只说明两个节点的内容一样，不能说明它们是同一个节点。
- **指针相等 (节点对象本身相等)**：说明它们指向的就是同一个节点（即相交点）。

所以这题必须写：

```python
if curA == curB:   # 比较指针
    return curA

```

---

# 4. Linked list Cycle 2

https://leetcode.com/problems/linked-list-cycle-ii/description/

Given the `head` of a linked list, return *the node where the cycle begins. If there is no cycle, return* `null`.

There is a cycle in a linked list if there is some node in the list that can be reached again by continuously following the `next` pointer. Internally, `pos` is used to denote the index of the node that tail's `next` pointer is connected to (**0-indexed**). It is `-1` if there is no cycle. **Note that** `pos` **is not passed as a parameter**.

**Do not modify** the linked list.

**Example 1:**

![](https://assets.leetcode.com/uploads/2018/12/07/circularlinkedlist.png)

```
Input: head = [3,2,0,-4], pos = 1
Output: tail connects to node index 1
Explanation: There is a cycle in the linked list, where tail connects to the second node.

```

**Example 2:**

![](https://assets.leetcode.com/uploads/2018/12/07/circularlinkedlist_test2.png)

```
Input: head = [1,2], pos = 0
Output: tail connects to node index 0
Explanation: There is a cycle in the linked list, where tail connects to the first node.

```

**Example 3:**

![](https://assets.leetcode.com/uploads/2018/12/07/circularlinkedlist_test3.png)

```
Input: head = [1], pos = -1
Output: no cycle
Explanation: There is no cycle in the linked list.
```

# 思路：

大概思路： 定义快慢指针， 快指针一次走两步，慢指针一次走一步。 两个指针从head 出发

如果linkedlist 没 cycle 那么这两个指针 不会相遇， 如果有cycle， 那么快慢指针会进入cycle 然后相遇。 

1. 为什么快指针会在cycle遇到慢指针？ 一定是快指针先进入cycle, 就是追上慢指针的过程， **因为快指针比慢指针相对每次都多走一个节点。 其实相对于slow来说，fast是一个节点一个节点的靠近slow的**，所以fast一定可以和slow重合。

1. 怎么找到cycle 的入口处？ 

假设从头结点到环形入口节点 的节点数为x。 环形入口节点到 fast指针与slow指针相遇节点 节点数为y。 从相遇节点 再到环形入口节点节点数为 z。 

![](https://file1.kamacoder.com/i/algo/20220925103433.png)

那么相遇时： slow指针走过的节点数为: `x + y`， 

fast指针走过的节点数：`x + y + n (y + z)`，n为fast指针在环内走了n圈才遇到slow指针， （y+z）为 一圈内节点的个数A。

因为fast指针是一步走两个节点，slow指针一步走一个节点， 所以 fast指针走过的节点数 = slow指针走过的节点数 * 2：

`(x + y) * 2 = x + y + n (y + z)` 

simplify this equation:

x + y = n(y + z)

x = n(y+z) - y

x = (n-1)(y+z) + z

when n = 1

**x = z** 

这就意味着，**从头结点出发一个指针，从相遇节点 也出发一个指针，这两个指针每次只走一个节点， 那么当这两个指针相遇的时候就是 环形入口的节点**。

也就是在相遇节点处，定义一个指针index1，在头结点处定一个指针index2。

让index1和index2同时移动，每次移动一个节点， 那么他们相遇的地方就是 环形入口的节点。

那么 n如果大于1是什么情况呢，就是fast指针在环形转n圈之后才遇到 slow指针。

其实这种情况和n为1的时候 效果是一样的，一样可以通过这个方法找到 环形的入口节点，只不过，index1 指针在环里 多转了(n-1)圈，然后再遇到index2，相遇点依然是环形的入口节点。

**重要的intuition： 快指针速度是慢指针的两倍，慢指针跑一圈的时间快指针能跑两圈，所以慢指针在跑完一圈之前二者一定会相遇**

### code

1. slow = head, fast = head 
2. while fast and fast.next:
    - move pointers: fast = [fast.next.next](http://fast.next.next)
    - slow = slow .next
    - if fast = slow: 有cycle
        - index1 = fast
        - index2 = head
        - while index1 ≠ index2;
            - index1 = index1. next
            - index2 = [index2.next](http://index2.next)
        - return index 1

return none 

```python
# Definition for singly-linked list.
# class ListNode:
#     def __init__(self, x):
#         self.val = x
#         self.next = None

class Solution:
    def detectCycle(self, head: Optional[ListNode]) -> Optional[ListNode]:
        slow = head
        fast = head 
        while fast and fast.next:
            slow = slow.next 
            fast = fast.next.next 
            if slow == fast:
                index1 = head
                index2 = fast
                while index1 != index2:
                    index1 = index1.next 
                    index2 = index2.next 
                return index1
        return None
```

USING SET:

```python
# Definition for singly-linked list.
# class ListNode:
#     def __init__(self, x):
#         self.val = x
#         self.next = None

class Solution:
    def detectCycle(self, head: ListNode) -> ListNode:
        visited = set()
        
        while head:
            if head in visited:
                return head
            visited.add(head)
            head = head.next
        
        return None
```

```
visited.add(head)
```

存的是

**节点对象本身（内存地址引用）**

，不是节点的值，更不是“剩下的链表”。

这样才能判断是否走到了同一个节点，也就是检测环。

## `head` 是什么？

在这段代码里，`head` 代表的是 **当前正在遍历的链表节点对象**（`ListNode` 实例），而不是它的值，也不是“后面整条链表”。

假设链表是：

```
1 → 2 → 3
     ↑   ↓
     ←←←←

```

遍历时：

- 第一次遇到节点 `2`，`visited.add(node_2)`，集合里存的是 `node_2` 这个对象。
- 当绕环再次走到节点 `2` 时，`head` 指向的还是同一个对象 `node_2`。
- 所以 `head in visited` 为真，返回该节点。