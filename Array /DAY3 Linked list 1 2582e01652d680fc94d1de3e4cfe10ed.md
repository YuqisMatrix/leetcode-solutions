# DAY3: Linked list 1

# 1. Types of Linked list

### **单链表**

刚刚说的就是单链表。

### [**#](https://programmercarl.com/%E9%93%BE%E8%A1%A8%E7%90%86%E8%AE%BA%E5%9F%BA%E7%A1%80.html#%E5%8F%8C%E9%93%BE%E8%A1%A8)双链表**

单链表中的指针域只能指向节点的下一个节点。

双链表：每一个节点有两个指针域，一个指向下一个节点，一个指向上一个节点。

双链表 既可以向前查询也可以向后查询。

如图所示：

![链表2](https://file1.kamacoder.com/i/algo/20200806194559317.png)

### [**#](https://programmercarl.com/%E9%93%BE%E8%A1%A8%E7%90%86%E8%AE%BA%E5%9F%BA%E7%A1%80.html#%E5%BE%AA%E7%8E%AF%E9%93%BE%E8%A1%A8)循环链表**

循环链表，顾名思义，就是链表首尾相连。

循环链表可以用来解决约瑟夫环问题。

![链表4](https://file1.kamacoder.com/i/algo/20200806194629603.png)

链表中的节点在内存中不是连续分布的 ，而是散乱分布在内存中的某地址上，分配机制取决于操作系统的内存管理

### ARRAY VS LINKED LIST

**Advantages and disadvantages compared to arrays**

LinkedList: **add and remove elements in any position in O(1)**. But we need to have a reference to that node, otherwise it is still linear. Array requires **O(n)** for adding and removing for any arbitrary position.

The main disadvantage of a linked list is **that there is no random access.** If you have a large linked list and want to access the 150,000th element, then there usually isn't a better way than to start at the head and iterate 150,000 times. **So while an array has O(1) indexing, a linked list could require O(n) to access an element at a given position.** 

linked lists have the advantage of not having fixed sizes. but more overhead than arrays, every element needs to have extra storage for the pointers.

![Screenshot 2025-08-22 at 7.53.39 PM.png](DAY3%20Linked%20list%201%202582e01652d680fc94d1de3e4cfe10ed/Screenshot_2025-08-22_at_7.53.39_PM.png)

数组在定义的时候，长度就是固定的，如果想改动数组的长度，就需要重新定义一个新的数组。

链表的长度可以是不固定的，并且可以动态增删， 适合数据量不固定，频繁增删，较少查询的场景

## Initialize a Linked list

```sql
class ListNode:
    def __init__(self, val, next=None):
        self.val = val
        self.next = next
```

1. traverse

```python
def run_linkedlist_quiz_1():
    print('LinkedList quiz 1')
    node_1 = ListNode(1)
    node_2 = ListNode(3)
    node_3 = ListNode(5)
    node_4 = ListNode(7)

    node_1.next = node_2
    node_2.next = node_3
    node_3.next = node_4

    cur = node_1
    while cur is not None:
        print(cur.val, end=' ')
        cur = cur.next
    print('\n')

cur = node_1
while cur is not None:
	print(cur.val, end=' ')
	# next 存的下一个节点的地址
	cur = cur.next

```

1. 我们需要修改next属性来改变链表 如果只修改node 不修改next，因为traverse需要next属性， 遍历下来链表不会改变

```python
def run_linkedlist_quiz_1():
    print('LinkedList quiz 1')
    node_1 = ListNode(1)
    node_2 = ListNode(3)
    node_3 = ListNode(5)
    node_4 = ListNode(7)

    node_1.next = node_2
    node_2.next = node_3
    node_3.next = node_4

    node_2 = node_3

    cur = node_1
    while cur is not None:
        print(cur.val, end=' ')
        cur = cur.next
    print('\n')
# this will print 1,3,5,7,
```

1. quiz 2. Since we change the head node, the linkedlist will start from node2. The following have the result 3,5,7

```python
def run_linkedlist_quiz_2():
    print('LinkedList quiz 2')
    node_1 = ListNode(1)
    node_2 = ListNode(3)
    node_3 = ListNode(5)
    node_4 = ListNode(7)

    node_1.next = node_2
    node_2.next = node_3
    node_3.next = node_4

    node_1 = node_2

    cur = node_1
    while cur is not None:
        print(cur.val, end=' ')
        cur = cur.next
    print('\n')
```

1. Delete node 2

```python
def run_linkedlist_quiz_3():
    # this function delets node2
    print('LinkedList quiz 3')
    node_1 = ListNode(1)
    node_2 = ListNode(3)
    node_3 = ListNode(5)
    node_4 = ListNode(7)

    node_1.next = node_2
    node_2.next = node_3
    node_3.next = node_4

    node_1.next = node_3

    cur = node_1
    while cur is not None:
        print(cur.val, end=' ')
        cur = cur.next
    print('\n')

if __name__ == '__main__':
    run_linkedlist_quiz_3()
```

1. add, get, set, and remove implementation 

```python
class ListNode:

    def __init__(self, val):
        self.val = val
        self.next = None

class MyLinkedList:

    def __init__(self):
        self.head = None

    def get(self, location):
        cur = self.head
				#since our node starts from 0 , we can use range(location)
        for i in range(location):
            cur = cur.next
        return cur.val

    def add(self, location, val):
 # insert in the middle
# find prev, which is the node left to new node
# find the new node
#disconnect prev with the original prev.next 
# connect the new linked list prev -> new node -> prev.next
        if location > 0:
            pre = self.head
# why range is location-1? because prev is the node left to new node
            for i in range(location - 1):
                pre = pre.next
            new_node = ListNode(val)
            new_node.next = pre.next
            pre.next = new_node
# insert the head node 
        elif location == 0:
            new_node = ListNode(val)
            new_node.next = self.head
            self.head = new_node

    def set(self, location, val):
        cur = self.head
        for i in range(location):
            cur = cur.next
        cur.val = val

    def remove(self, location):
        if location > 0:
            pre = self.head
            for i in range(location - 1):
                pre = pre.next

            pre.next = pre.next.next

        elif location == 0:
            self.head = self.head.next

    def traverse(self):
        cur = self.head
        while cur is not None:
            print(cur.val, end=' ')
            cur = cur.next
        print()

    def is_empty(self):
        return self.head is None

if __name__ == '__main__':
    ll = MyLinkedList()
    ll.add(0, 1)
    ll.add(1, 3)
    ll.add(2, 5)
    ll.add(3, 7)

    ll.add(0, 9)
    ll.add(1, 100)
    ll.traverse()

    print(ll.get(1))
    print(ll.get(3))

    ll.set(0, -100)
    ll.set(2, 32)
    ll.traverse()

    ll.remove(2)
    ll.traverse()
```