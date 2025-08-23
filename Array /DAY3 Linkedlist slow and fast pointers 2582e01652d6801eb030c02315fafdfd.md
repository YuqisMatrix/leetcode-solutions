# DAY3 Linkedlist : slow and fast pointers

# 1. https://leetcode.com/problems/remove-linked-list-elements/description/

Given the `head` of a linked list and an integer `val`, remove all the nodes of the linked list that has `Node.val == val`, and return *the new head*.

**Example 1:**

![](https://assets.leetcode.com/uploads/2021/03/06/removelinked-list.jpg)

```
Input: head = [1,2,6,3,4,5,6], val = 6
Output: [1,2,3,4,5]

```

**Example 2:**

```
Input: head = [], val = 1
Output: []

```

**Example 3:**

```
Input: head = [7,7,7,7], val = 7
Output: [
```

1. Deleting target value from the original linked list : 头节点需要单独check 一次 

来看第一种操作：直接使用原来的链表来进行移除。

LINKED LIST: 1 →4 →2 →4, TARGET VAL = 1

移除头结点和移除其他节点的操作是不一样的，因为链表的其他节点都是通过前一个节点来移除当前节点，而头结点没有前一个节点。

所以头结点如何移除呢，其实只要将头结点向后移动一位就可以，这样就从链表中移除了一个头结点。

![203_链表删除元素4](https://file1.kamacoder.com/i/algo/20210316095512470.png)

依然别忘将原头结点从内存中删掉。

![203_链表删除元素5](https://file1.kamacoder.com/i/algo/20210316095543775.png)

```python
# Definition for singly-linked list.
# class ListNode:
#     def __init__(self, val=0, next=None):
#         self.val = val
#         self.next = next
class Solution:
    def removeElements(self, head: Optional[ListNode], val: int) -> Optional[ListNode]:
        if not head:
            return 
        if head and head.val == val:
            head = head.next 
        curr = head 
        while curr and curr.next:
            if curr.next.val == val:
                curr.next = curr.next.next 
            else:
                curr = curr.next 
        return head
```

Time complexity: O(n)
Space complexity: O(1)

okay what is wrong this code? 

**Input**

head =

[7,7,7,7]

val =

7

**Output**

[7]

**Expected**

[]

when checking the head node value: if head and head.val == val:, it should be **while loop, not if loop.** 

The code above only removes the head once even if there are multiple leading nodes equal to val.

In this case, the first 7 is removed, we need to remove the new head node. In the code `while` loop only checks `curr.next`, so it never deletes the new head itself.

```python
# Definition for singly-linked list.
# class ListNode:
#     def __init__(self, val=0, next=None):
#         self.val = val
#         self.next = next
class Solution:
    def removeElements(self, head: Optional[ListNode], val: int) -> Optional[ListNode]:
        if not head:
            return 
        while head and head.val == val:#we need to continuously checking the new head
            head = head.next 
        curr = head 
        while curr and curr.next:
            if curr.next.val == val:
                curr.next = curr.next.next 
            else:
                curr = curr.next 
        return head
```

1. CREATE A DUMMY NODE, WHICH POINTS TO THE HEAD NODE, AND TREAT EACH NODE IN ONE WHILE LOOP

```python
版本一）虚拟头节点法
# Definition for singly-linked list.
# class ListNode:
#     def __init__(self, val=0, next=None):
#         self.val = val
#         self.next = next
class Solution:
    def removeElements(self, head: Optional[ListNode], val: int) -> Optional[ListNode]:
        # 创建虚拟头部节点以简化删除过程
        dummy_head = ListNode(next = head)
        
        # 遍历列表并删除值为val的节点
        current = dummy_head
        while current.next:
            if current.next.val == val:
                current.next = current.next.next
            else:
                current = current.next
        
        return dummy_head.next
```

---

# 2. Design a Linkedlist

https://leetcode.com/problems/design-linked-list/submissions/1744961895/

Design your implementation of the linked list. You can choose to use a singly or doubly linked list.

A node in a singly linked list should have two attributes: `val` and `next`. `val` is the value of the current node, and `next` is a pointer/reference to the next node.

If you want to use the doubly linked list, you will need one more attribute `prev` to indicate the previous node in the linked list. Assume all nodes in the linked list are **0-indexed**.

Implement the `MyLinkedList` class:

- `MyLinkedList()` Initializes the `MyLinkedList` object.
- `int get(int index)` Get the value of the `indexth` node in the linked list. If the index is invalid, return `1`.
- `void addAtHead(int val)` Add a node of value `val` before the first element of the linked list. After the insertion, the new node will be the first node of the linked list.
- `void addAtTail(int val)` Append a node of value `val` as the last element of the linked list.
- `void addAtIndex(int index, int val)` Add a node of value `val` before the `indexth` node in the linked list. If `index` equals the length of the linked list, the node will be appended to the end of the linked list. If `index` is greater than the length, the node **will not be inserted**.
- `void deleteAtIndex(int index)` Delete the `indexth` node in the linked list, if the index is valid.

**Example 1:**

```
Input
["MyLinkedList", "addAtHead", "addAtTail", "addAtIndex", "get", "deleteAtIndex", "get"]
[[], [1], [3], [1, 2], [1], [1], [1]]
Output
[null, null, null, null, 2, null, 3]

Explanation
MyLinkedList myLinkedList = new MyLinkedList();
myLinkedList.addAtHead(1);
myLinkedList.addAtTail(3);
myLinkedList.addAtIndex(1, 2);    // linked list becomes 1->2->3
myLinkedList.get(1);              // return 2
myLinkedList.deleteAtIndex(1);    // now the linked list is 1->3
myLinkedList.get(1);              // return 3

```

```python
class MyLinkedList:

    def __init__(self):
        

    def get(self, index: int) -> int:
        

    def addAtHead(self, val: int) -> None:
        

    def addAtTail(self, val: int) -> None:
        

    def addAtIndex(self, index: int, val: int) -> None:
        

    def deleteAtIndex(self, index: int) -> None:
        

# Your MyLinkedList object will be instantiated and called as such:
# obj = MyLinkedList()
# param_1 = obj.get(index)
# obj.addAtHead(val)
# obj.addAtTail(val)
# obj.addAtIndex(index,val)
# obj.deleteAtIndex(index)
```

---

## 心得： 这道题最关键的是

### 遍历： add/del at index. 如果我们要操作第index th 的node， 那么curr 一定是 index-1 th node， 第index th node [一定是curr.next](http://一定是curr.next)

### 赋值： 连接新的节点，一定要先连接后面的 再连接前面的，因为要保证curr.next 是旧的后面的节点。

1. get : 获取第几个节点的值  def get(self, index: int) -> int:

链表index从0 开始。 

- check bounds: if index < 0 or index ≥ size: return -1
- set curr = dummy node, points to head node,
- for _ in range(index)
    - curr = [curr.next](http://curr.next)
- return curr.val

想一个极端数值： return value of the 0th index， so curr points to head, returns head node 

1. 头部插入节点def addAtHead(self, val: int) -> None:
- 先定义一个new listnode,
- new listnode points to dummy [node.next](http://node.next) **先连接新node 到 旧的头节点. [因为旧的头节点可以用dummynode.next](http://因为旧的头节点可以用dummynode.next) 表示 如果先连前面的， dummy node.next will be the new listnode**
- dummy node points to new listnode **再连接dummy 节点.**
- size += 1

1. 尾部插入节点  def addAtTail(self, val: int) -> None:

**啥是尾部？if [curr.next](http://curr.next) is empty, then curr is the last element in the linked list** 

插入新的new node, so curr points to new node, and size += 1

1. 在n个节点前插入节点 def addAtIndex(self, index: int, val: int) -> None:

如果要在n个节点前插入， we need to get the n-1th and n+1 th node. **so curr node is n-1th node.** 

- current = dummy
- 找到第n -1 个node
- 极端例子： index = 0, **the for loop runs 0 time, newnode points to headnode, dummy node points to new node**
- size += 1

```python
curr = self.dummy 
for _ in range(index):
     curr = curr.next 
n = ListNode(val)
n.next = curr.next
curr.next = n
self.size += 1
```

1. `def deleteAtIndex(self, index: int) -> None:`  删除 index th node 

 Delete the `indexth` node in the linked list, if the index is valid.

- check bound
- 要删除index th node, we need to know **index-1 th node, which is curr. so we can have [curr.next](http://curr.next) = [curr.next.next](http://curr.next.next)**
- 找第index-1 节点
    
    ```python
    if index < 0 or index >= self.size:
                return
            curr = self.dummy 
            for _ in range(index):
                curr = curr.next 
            curr.next = curr.next.next
            self.size -= 1
    ```
    
- edge case: index = 0, so we do not run the for loop, curr is dummy node, dummy [node.next] is head.next, so we delete the head node.

```python
class ListNode:
    def __init__(self, val=0, next=None):
        self.val = val
        self.next = next


class MyLinkedList:

    def __init__(self):
        self.dummy = ListNode(0)
        self.size = 0

    def get(self, index: int) -> int:
        if index < 0 or index >= self.size:
            return -1
        curr = self.dummy.next
        for _ in range(index):
            curr = curr.next
        return curr.val

    def addAtHead(self, val: int) -> None:
        headnode = ListNode(val)
        headnode.next = self.dummy.next
        self.dummy.next = headnode
        self.size += 1

    def addAtTail(self, val: int) -> None:
        curr = self.dummy
        for _ in range(self.size):
            curr = curr.next
        tailnode = ListNode(val)
        curr.next = tailnode
        self.size += 1

    def addAtIndex(self, index: int, val: int) -> None:
        if index < 0 or index > self.size:
            return
        curr = self.dummy
        for _ in range(index):
            curr = curr.next
        n = ListNode(val)
        n.next = curr.next
        curr.next = n
        self.size += 1

    def deleteAtIndex(self, index: int) -> None:
        if index < 0 or index >= self.size:
            return
        curr = self.dummy
        for _ in range(index):
            curr = curr.next
        curr.next = curr.next.next
        self.size -= 1
```








---

# 3. Reverse Linkedlist

https://leetcode.com/problems/reverse-linked-list/submissions/1745018293/

Given the `head` of a singly linked list, reverse the list, and return *the reversed list*.

**Example 1:**

![](https://assets.leetcode.com/uploads/2021/02/19/rev1ex1.jpg)

```
Input: head = [1,2,3,4,5]
Output: [5,4,3,2,1]

```

**Example 2:**

![](https://assets.leetcode.com/uploads/2021/02/19/rev1ex2.jpg)

```
Input: head = [1,2]
Output: [2,1]

```

**Example 3:**

```
Input: head = []
Output: []

```

**Constraints:**

- The number of nodes in the list is the range `[0, 5000]`.
- `5000 <= Node.val <= 5000`

**Follow up:** A linked list can be reversed either iteratively or recursively. Could you implement both?

### **这道题做过 但是今天在prev 指针initialization卡住了，我以为是prev = dummy node, 但是这个题要反转列表，我之前没懂为什么prev要设置成 none**

其实只需要改变链表的next指针的指向，直接将链表反转 ，而不用重新定义一个新的链表，如图所示:

![206_反转链表](https://file1.kamacoder.com/i/algo/20210218090901207.png)

1. curr = head, head 往前指 指向nul , so prev = null. 初始化是为了head指向 prev。 反转以后head 节点就是尾节点， 所以prev = null . 看例子和code， prev 是当前curr 要向后指的node， 原来第一个node 要变成尾巴了， next 要指向null, 所以初始化要是none.  也可以理解为prev 是慢指针
2. while loop 什么时候算遍历结束呢？ 当current指向空指针结束， while current。
    - 赋值： 用一个临时指针temp 要在没赋值之前， 也就是current还和curr next 连着的时候保存curr next. temp = [curr.next](http://curr.next)
    - curr = prev
    - and then move curr and prev pointers 整体向后移动， prev = curr, curr = temp. **一定要先移动prev 再移动curr。 为什么？先移动curr 的话 curr = temp， prev 指向的curr。**
3. 新链表头节点 return prev

```python
class Solution:
    def reverseList(self, head: Optional[ListNode]) -> Optional[ListNode]:
        if not head:
            return 
        dummy = ListNode(0)
        dummy.next = head 
        prev = None
        curr = head 

        while curr:
            temp = curr.next 
            curr.next = prev
            prev = curr 
            curr = temp
        return prev
```

用 `1->2->3` 走一遍：

- 初始：prev=None, curr=1
- 第1步：temp=2，1.next=None，prev=1，curr=2
- 第2步：temp=3，2.next=1，prev=2，curr=3
- 第3步：temp=None，3.next=2，prev=3，curr=None → 返回 prev（3->2->1）

---

USING RECURSION:

```python
# Definition for singly-linked list.
# class ListNode:
#     def __init__(self, val=0, next=None):
#         self.val = val
#         self.next = next
class Solution:
    def reverseList(self, head: ListNode) -> ListNode:
        return self.reverse(head, None)
    def reverse(self, cur: ListNode, pre: ListNode) -> ListNode:#用来反转列表的
        if cur == None:
            return pre
        temp = cur.next
        cur.next = pre
        return self.reverse(temp, cur)
```

reverse 传入（cur, pre）所以就是（head, none）， 初始化

什么时候循环终止？ curr 为空： if cur = none, return pre 头节点

然后current and pre 改变方向：temp = [cur.next](http://cur.next), cur.next = pre. 然后移动指针， 就是reverse(temp, cur) current 赋值给pre，tmp赋值给cur。 逻辑和上面的是一样的