# DAY 11 Binary Tree - Part1 and Recursive DFS

# 1. Two types of binary tree

- **Full Binary Tree 满二叉树**
    
    A binary tree in which every node has either 0 or 2 children (no node has only one child).
    
    满二叉树：如果一棵二叉树只有度为0的结点和度为2的结点，并且度为0的结点在同一层上，则这棵二叉树为满二叉树。也可以说深度为k，有2^k-1个节点的二叉树
    
- **Complete Binary Tree 完全二叉树**
    
    A binary tree in which all levels are completely filled except possibly the last level, and the last level has all keys as left as possible.
    

完全二叉树的定义如下：在完全二叉树中，除了最底层节点可能没填满外，其余每层节点数都达到最大值，并且最下面一层的节点都集中在该层最左边的若干位置。若最底层为第 h 层（h从1开始），则该层包含 1~ 2^(h-1) 个节点。

![](https://file1.kamacoder.com/i/algo/20200920221638903.png)

满二叉树一定是完全二叉树

---

# 2. Binary Search Tree and AVL

A **Binary Search Tree** is a type of binary tree with the following properties:

1. The **left child** of a node contains only nodes with values **less than** the parent’s value.
2. The **right child** of a node contains only nodes with values **greater than** the parent’s value.
3. Both left and right subtrees must also be **binary search trees**.

👉 This property makes **searching, insertion, and deletion** efficient (average time complexity: **O(log n)**).

```markdown
       8
      / \
     3   10
    / \    \
   1   6    14
      / \   /
     4   7 13

```

### AVL Tree

An **AVL Tree** is a type of **self-balancing Binary Search Tree**.

- For every node, the **height difference** between its left and right subtrees (called the **balance factor**) must be **–1, 0, or +1**.
- If an insertion or deletion makes the tree unbalanced, **rotations** (LL, RR, LR, RL) are performed to restore balance.

👉 Because of this balancing, operations like **search, insert, delete** always run in **O(log n)** time, even in the worst case (unlike a plain BST which can degrade to O(n) if it becomes skewed).

它是一棵空树或它的左右两个子树的高度差的绝对值不超过1，并且左右两个子树都是一棵平衡二叉树。

如图：

![](https://file1.kamacoder.com/i/algo/20200806190511967.png)

**C++中map、set、multimap，multiset的底层实现都是平衡二叉搜索树**，所以map、set的增删操作时间时间复杂度是logn. 

---

# 3. **二叉树的存储方式**

**二叉树可以链式存储，也可以顺序存储。**

那么链式存储方式就用指针， 顺序存储的方式就是用数组。

顾名思义就是顺序存储的元素在内存是连续分布的，而链式存储则是通过指针把分布在各个地址的节点串联一起。

### 3.1 链式存储

- 每个结点包含三个部分：
    1. 数据域（存储结点的值）
    2. 左孩子指针域（指向左子结点）
    3. 右孩子指针域（指向右子结点）

特点：

- 更灵活，不需要连续存储空间。
- 常用于实现一般的二叉树、搜索树（BST）、AVL 树等。

![](https://file1.kamacoder.com/i/algo/2020092019554618.png)

### 3.2 顺序存储

- 用 **数组** 来存储二叉树的结点。
- 如果根节点存放在下标 `i=1`（有时是 0），那么：
    - 左孩子的下标是 `2*i + 1`
    - 右孩子的下标是 `2*i + 2`
    - 父节点的下标是 `i//2`

👉 特点：

- 适合存储 **完全二叉树**（Complete Binary Tree），空间利用率高。
- 对于普通二叉树，可能会浪费很多数组空间。

![](https://file1.kamacoder.com/i/algo/20200920200429452.png)

二叉树就是一个链表，有两个pointer

```python
class TreeNode:
    def __init__(self, val, left = None, right = None):
        self.val = val
        self.left = left
        self.right = right
```

---

# 4. 二叉树的遍历方式

二叉树主要有两种遍历方式：

1. 深度优先遍历：先往深走，遇到叶子节点再往回走。
- 前序遍历（递归法，迭代法）
- 中序遍历（递归法，迭代法）
- 后序遍历（递归法，迭代法）
1. 广度优先遍历：一层一层的去遍历。
    ◦ 层次遍历（迭代法）

## 4.1 DFS traversal

**这里前中后，其实指的就是中间节点的遍历顺序**

1. **Preorder Traversal (Root → Left → Right)前**
    - Visit the root first, then traverse the left subtree, then the right subtree.
2. **Inorder Traversal (Left → Root → Right)中**
    - Traverse the left subtree, then visit the root, then the right subtree.
    - Example (for BST): produces nodes in **sorted order**
3. **Postorder Traversal (Left → Right → Root) 后**
    - Traverse the left subtree, then the right subtree, and visit the root last.

![](https://file1.kamacoder.com/i/algo/20200806191109896.png)

**栈其实就是递归的一种实现结构**，也就说前中后序遍历的逻辑其实都是可以借助栈使用递归的方式来实现的。

而广度优先遍历的实现一般使用队列来实现，这也是队列先进先出的特点所决定的，因为需要先进先出的结构，才能一层一层的来遍历二叉树。

---

# 5. 二叉树的定义

二叉树的定义 和链表是差不多的，相对于链表 ，二叉树的节点里多了一个指针， 有两个指针，指向左右孩子

```python
class TreeNode:
    def __init__(self, val, left = None, right = None):
        self.val = val
        self.left = left
        self.right = right
```

### Example Usage

```python
# Root node
root = TreeNode(10)

# Level 1 children
root.left = TreeNode(5)
root.right = TreeNode(15)

# Level 2 children under 5
root.left.left = TreeNode(3)
root.left.right = TreeNode(7)

# Level 2 children under 15
root.right.left = TreeNode(12)
root.right.right = TreeNode(18)

```

```python
          10
        /    \
       5      15
      / \    /  \
     3   7  12  18

```

```python
print(root.val)          # 10
print(root.left.val)     # 5
print(root.right.val)    # 15
print(root.left.left.val)  # 3
print(root.right.right.val) # 18

```

---

# 6. Recursive DFS 二叉树的递归遍历

144, 145, 94. 

**每次写递归，都按照这三要素来写**

以 preorder 来举例： root, left, right 

1. **确定递归函数的参数和返回值：** 确定哪些参数是递归的过程中需要处理的，那么就在递归函数里加上这个参数， 并且还要明确每次递归的返回值是什么进而确定递归函数的返回类型。
    - 递归函数是 `dfs(node)`。
    - 参数：`node`（表示当前遍历到的结点）。
    - 返回值：函数本身没有显式返回（`return`），但它通过修改外部变量 `res` 来保存结果。
        
        👉 所以返回类型是 **None**，但结果保存在 `res` 里。
        

1. **确定终止条件：** 写完了递归算法, 运行的时候，经常会遇到栈溢出的错误，就是没写终止条件或者终止条件写的不对，操作系统也是用一个栈的结构来保存每一层递归的信息，如果递归没有终止，操作系统的内存栈必然就会溢出。

base case: 

```python
if node is None:
    return
```

1. **确定单层递归的逻辑：** 确定每一层递归需要处理的信息。在这里也就会重复调用自己来实现递归的过程。

```python
res.append(node.val)   # 处理当前节点
dfs(node.left)         # 递归遍历左子树
dfs(node.right)        # 递归遍历右子树

```

preorder:

```python
# 前序遍历-递归-LC144_二叉树的前序遍历
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right

class Solution:
    def preorderTraversal(self, root: TreeNode) -> List[int]:
        res = []
        
        def dfs(node):
            if node is None:
                return
            
            res.append(node.val)
            dfs(node.left)
            dfs(node.right)
        dfs(root)
        return res
```

postorder: left, right, root

```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    def postorderTraversal(self, root: Optional[TreeNode]) -> List[int]:
        res = []
        
        def dfs(node):
            if node is None:
                return
            
            dfs(node.left)
            dfs(node.right)
            res.append(node.val)
        dfs(root)
        return res
```

inorder: left, root, right

```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    def inorderTraversal(self, root: Optional[TreeNode]) -> List[int]:
        res = []
        def dfs(node):
            if node is None:
                return 
            dfs(node.left)
            res.append(node.val)
            dfs(node.right)
        dfs(root)
        return res
        
```