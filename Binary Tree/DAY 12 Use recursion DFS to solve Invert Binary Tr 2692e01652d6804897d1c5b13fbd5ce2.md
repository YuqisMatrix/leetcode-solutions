# DAY 12 Use recursion DFS to solve: Invert Binary Tree, Symmetric Tree, Max & Min Depth of a Binary Tree

# 1. 226 Invert Binary Tree

https://leetcode.com/problems/invert-binary-tree/

Given the `root` of a binary tree, invert the tree, and return *its root*.

**Example 1:**

![](https://assets.leetcode.com/uploads/2021/03/14/invert1-tree.jpg)

```
Input: root = [4,2,7,1,3,6,9]
Output: [4,7,2,9,6,3,1]

```

**Example 2:**

![](https://assets.leetcode.com/uploads/2021/03/14/invert2-tree.jpg)

```
Input: root = [2,1,3]
Output: [2,3,1]

```

**Example 3:**

```
Input: root = []
Output: []

```

### 1. Use recursion

交换孩子是指针 不是数值。 

用哪个顺序遍历呢？ 用前序和后序都可以 但中序是最绕的。 具体咋实现的？ 

递归三部曲：

1. 确定递归函数的参数和返回值

参数就是要传入节点的指针，不需要其他参数了，通常此时定下来主要参数，如果在写递归的逻辑中发现还需要其他参数的时候，随时补充。

```python
def invertTree(self,  roo: TreeNode)：
```

1. base case:

当前节点为空的时候，就返回

```python
if not node:
	return 
```

1. 确定单层递归的逻辑

前序： root, left, right. 

处理逻辑： 

中： 我们要交换root 节点的两个孩子， 所以node.left, node.right = node.right, node.left

然后遍历左树，遍历右树

```python
root.left, root.right = root.right, root.left
self.invertTree(root.left)
self.invertTree(root.right)
return root
```

后序遍历： 左 右 中 也可以 为啥中序遍历不行？ 

**因为使用递归的中序遍历，某些节点的左右孩子会翻转两次。**

所以如果中序遍历，先处理左子树，然后swap， swap 完了 右子树是处理过的，左子树没处理过， 所以

要接着处理左子树

```python
class Solution:
    def invertTree(self, root: TreeNode) -> TreeNode:
        if not root:
            return None
        self.invertTree(root.left)
        root.left, root.right = root.right, root.left
        self.invertTree(root.left)
        return root
```

### 2. Use stack:

preorder: root, left, right 

**We push right child before the left child**, so we can pop the left one first on the stack!!

```python
class Solution:
    def invertTree(self, root: Optional[TreeNode]) -> Optional[TreeNode]:
        if not root:
            return None 
        stack = [root]
        while stack:
            cur = stack.pop()
            cur.left,cur.right = cur.right,cur.left 
            if cur.right:
                stack.append(cur.right)
            if cur.left:
                stack.append(cur.left)
        return root
```

---

# 2. Symmetric Tree

https://leetcode.com/problems/symmetric-tree/

Given the `root` of a binary tree, *check whether it is a mirror of itself* (i.e., symmetric around its center).

**Example 1:**

![](https://assets.leetcode.com/uploads/2021/02/19/symtree1.jpg)

```
Input: root = [1,2,2,3,4,4,3]
Output: true

```

**Example 2:**

![](https://assets.leetcode.com/uploads/2021/02/19/symtree2.jpg)

```
Input: root = [1,2,2,null,3,null,3]
Output: false

```

### 本题关键：

1. 判断对称二叉树就是判断左右孩子是否可以反转
2. 遍历的顺序， 我们比较外侧 和 内侧： 左孩子的左孩子 vs 右孩子的右孩子， 左孩子的右孩子vs 右孩子的左孩子

**判断对称二叉树要比较的是哪两个节点，要比较的可不是左右节点！**

对于二叉树是否对称，要比较的是根节点的左子树与右子树是不是相互翻转的，理解这一点就知道了**其实我们要比较的是两个树（这两个树是根节点的左右子树）**，所以在递归遍历的过程中，也是要同时遍历两棵树。

**比较外侧的节点是否相等**

**比较内侧的节点是否相等**

![101. 对称二叉树1](https://file1.kamacoder.com/i/algo/20210203144624414.png)

那么遍历的顺序应该是什么样的呢？

### **最关键的一步： 要如何遍历？**

递归：只能用后序 左右中。 为啥？**我们要收集左右孩子的信息返回给上一个节点**

**正是因为要遍历两棵树而且要比较内侧和外侧节点，所以准确的来说是一个树的遍历顺序是左右中，一个树的遍历顺序是右左中。** 

**什么时候用后序遍历？需要收集孩子信息然后向上级返回**

### 1. 递归第一步确定递归函数的参数以及返回值

```python
def compare(self, left, right):
```

### 2. stop condition:

左为空 右不为空 return false 

左右都为空： return true 

左不为空 右为空： return false 

值不想等： 左右节点值 不同。 

```python
#首先排除空节点的情况
        if left == None and right != None: return False
        elif left != None and right == None: return False
        elif left == None and right == None: return True
        #排除了空节点，再排除数值不相同的情况
        elif left.val != right.val: return False
```

### 3. 当左右值相等的时候才继续递归逻辑

做下一层的判断

比较外侧节点：左孩子的左孩子 与 右孩子的右孩子进行比较

比较内侧节点：左孩子的右孩子 与 右孩子的左孩子进行比较

外侧 和 内侧都相等的情况下 返回

```python
#此时就是：左右节点都不为空，且数值相同的情况
        #此时才做递归，做下一层的判断
        outside = self.compare(left.left, right.right) #左子树：左、 右子树：右
        inside = self.compare(left.right, right.left) #左子树：右、 右子树：左
        isSame = outside and inside #左子树：中、 右子树：中 （逻辑处理）
        return isSame
```

完整代码

```python
class Solution:
    def isSymmetric(self, root: TreeNode) -> bool:
        if not root:
            return True
        return self.compare(root.left, root.right)
        
    def compare(self, left, right):
        #首先排除空节点的情况
        if left == None and right != None: return False
        elif left != None and right == None: return False
        elif left == None and right == None: return True
        #排除了空节点，再排除数值不相同的情况
        elif left.val != right.val: return False
        
        #此时就是：左右节点都不为空，且数值相同的情况
        #此时才做递归，做下一层的判断
        outside = self.compare(left.left, right.right) #左子树：左、 右子树：右
        inside = self.compare(left.right, right.left) #左子树：右、 右子树：左
        isSame = outside and inside #左子树：中、 右子树：中 （逻辑处理）
        return isSame
```

本体考察两个二叉树的遍历过程 和对应过程。 

左子树是： 左 右 中 遍历顺序

右子树是 右 左 中  遍历顺序

都是后序遍历。 中序遍历 前序遍历都不行，**只有左右中 后序遍历才能是遍历完左孩子 遍历完右孩子 然后再返回root。**

中序遍历 中 左右孩子都没遍历 没法比较。 

### 迭代法也是 比较左节点左孩子 vs 右节点右孩子， 右节点右孩子 vs 左节点左孩子

```python
import collections
class Solution:
    def isSymmetric(self, root: TreeNode) -> bool:
        if not root:
            return True
        queue = collections.deque()
        queue.append(root.left) #将左子树头结点加入队列
        queue.append(root.right) #将右子树头结点加入队列
        while queue: #接下来就要判断这这两个树是否相互翻转
            leftNode = queue.popleft()
            rightNode = queue.popleft()
            if not leftNode and not rightNode: #左节点为空、右节点为空，此时说明是对称的
                continue
            
            #左右一个节点不为空，或者都不为空但数值不相同，返回false
            if not leftNode or not rightNode or leftNode.val != rightNode.val:
                return False
            queue.append(leftNode.left) #加入左节点左孩子
            queue.append(rightNode.right) #加入右节点右孩子
            queue.append(leftNode.right) #加入左节点右孩子
            queue.append(rightNode.left) #加入右节点左孩子
        return True
```

正确写法2

```python
class Solution:
    def isSymmetric(self, root: Optional[TreeNode]) -> bool:
        if not root:
            return True

        def compare(left, right):   # 这里是局部函数，不需要 self
            if not left and right:
                return False
            elif not left and not right:
                return True
            elif not right and left:
                return False
            elif left.val != right.val:
                return False
            outside = compare(left.left, right.right)
            inside = compare(left.right, right.left)
            return outside and inside

        return compare(root.left, root.right)

```

### 🟢 用 `self` 的情况

1. **调用类里面的其他方法时**
    - 因为方法属于类的实例对象，要通过 `self` 来访问。
    
    ```python
    class Solution:
        def isSymmetric(self, root):
            return self.compare(root.left, root.right)
    
        def compare(self, left, right):
            ...
    
    ```
    
    🔑 这里 `compare` 是类方法（定义时带 `self`），所以必须写 `self.compare(...)`。
    
2. **访问类里面的属性（成员变量）时**
    - 比如 `self.val`、`self.head`。
    - 因为这些属性绑定在对象上，而不是局部变量。

---

### 🔴 不用 `self` 的情况

1. **函数是局部函数（nested function）时**
    - 你在一个方法里面 `def compare(...): ...`，它只是一个普通的函数，不属于类的实例。
    - 所以调用时直接写 `compare(...)`，不能加 `self`。
    
    ```python
    class Solution:
        def isSymmetric(self, root):
            def compare(left, right):  # 局部函数
                ...
            return compare(root.left, root.right)
    
    ```
    
2. **函数或变量是局部变量时**
    - 比如 `count = 0`，调用时直接用 `count`。
    - `self.count` 指的就是类里定义的属性，不是局部变量

---

# 3. 104 Maximum Depth of Binary Tree

https://leetcode.com/problems/maximum-depth-of-binary-tree/

### 1. Depth vs Height

| Concept | Definition | Root | Leaf |
| --- | --- | --- | --- |
| **Depth** | Distance from root (edges upward) | `0` | Equal to its level in the tree |
| **Height** | Longest path to a leaf (edges downward) | Equal to tree height | `0` |

Tree:

```
       1
      / \
     2   3
    / \
   4   5

```

- Depth:
    - Node `1` → depth = 0
    - Node `2` → depth = 1
    - Node `4` → depth = 2
- Height:
    - Node `4` → height = 0
    - Node `2` → height = 1 (max path: 2→4 or 2→5)
    - Node `1` → height = 2 (max path: 1→2→4)

**高度：通过后序 左右中， 叶子节点返回root node 然后再加1**

**深度：前序遍历：中 左 右**

所以这道题要求depth, which is the height of root. 

1. recursion parameter and return 

```python
def get_height(self,node):
		
```

1. stop condition 

```python
if not node:
	return 0 
```

1. recursion logic

```python
leftheight = self.get_height(node.left)  #左
rightheight = self.get_height(node.right) #右
height = 1 + max(leftheight,righthright) #中
return height 
```

**后序遍历** 从下往上计数

```python
class Solution:
    def maxdepth(self, root: treenode) -> int:
        return self.getdepth(root)
        
    def getdepth(self, node):
        if not node:
            return 0
        leftheight = self.getdepth(node.left) #左
        rightheight = self.getdepth(node.right) #右
        height = 1 + max(leftheight, rightheight) #中
        return height
```

递归法：精简代码 但看不出后序遍历

```python
class Solution:
    def maxdepth(self, root: treenode) -> int:
        if not root:
            return 0
        return 1 + max(self.maxdepth(root.left), self.maxdepth(root.right))
```

层序遍历迭代法：

用队列 遍历每一层， 每遍历一层，计数+1

```python
class Solution:
    def maxDepth(self, root: TreeNode) -> int:
        if not root:
            return 0
        
        depth = 0
        queue = collections.deque([root])
        
        while queue:
            depth += 1
            for _ in range(len(queue)):
                node = queue.popleft()
                if node.left:
                    queue.append(node.left)
                if node.right:
                    queue.append(node.right)
        
        return depth

```

---

# 4. 111 Min depth of Binary Tree

https://leetcode.com/problems/minimum-depth-of-binary-tree/

Given a binary tree, find its minimum depth.

The minimum depth is the number of nodes along the shortest path from the root node down to the nearest leaf node.

**Note:** A leaf is a node with no children.

**Example 1:**

![](https://assets.leetcode.com/uploads/2020/10/12/ex_depth.jpg)

```
Input: root = [3,9,20,null,null,15,7]
Output: 2

```

**Example 2:**

```
Input: root = [2,null,3,null,4,null,5,null,6]
Output: 5

```

根节点到最近的叶子节点的距离. 

本题依然是前序遍历和后序遍历都可以，前序求的是深度，后序求的是高度。

- 二叉树节点的深度：指从根节点到该节点的最长简单路径边的条数或者节点数（取决于深度从0开始还是从1开始）
- 二叉树节点的高度：指从该节点到叶子节点的最长简单路径边的条数或者节点数（取决于高度从0开始还是从1开始）

那么使用后序遍历，其实求的是根节点到叶子节点的最小距离，就是求高度的过程，不过这个最小距离 也同样是最小深度。

![111.二叉树的最小深度](https://file1.kamacoder.com/i/algo/111.%E4%BA%8C%E5%8F%89%E6%A0%91%E7%9A%84%E6%9C%80%E5%B0%8F%E6%B7%B1%E5%BA%A6.png)

这就重新审题了，题目中说的是：**最小深度是从根节点到最近叶子节点的最短路径上的节点数量。注意是叶子节点**。

**后序： 左右中 height**

1. recursion parameter and return 

```python
def getDepth(self, node):
```

1. stop condition 

```python
if node is None:
            return 0
```

1. recursive logic 

左 右 中

```python
leftDepth = self.getDepth(node.left)  # 左
        rightDepth = self.getDepth(node.right)  # 右
        
        # 当一个左子树为空，右不为空，这时并不是最低点
        if node.left is None and node.right is not None:
            return 1 + rightDepth
        
        # 当一个右子树为空，左不为空，这时并不是最低点
        if node.left is not None and node.right is None:
            return 1 + leftDepth
        
        result = 1 + min(leftDepth, rightDepth)
        return result
```

以下这么写为啥不对？

```python
class Solution:
    def minDepth(self, root: Optional[TreeNode]) -> int:
        # logic is return min(left, right) + 1
        if not root:
            return 0
        return 1 + min(self.minDepth(root.left), self.minDepth(root.right))
        
```

这个代码就犯了此图中的误区：

![111.二叉树的最小深度](https://file1.kamacoder.com/i/algo/111.%E4%BA%8C%E5%8F%89%E6%A0%91%E7%9A%84%E6%9C%80%E5%B0%8F%E6%B7%B1%E5%BA%A6.png)

如果这么求的话，没有左孩子的分支会算为最短深度。

所以，如果左子树为空，右子树不为空，说明最小深度是 1 + 右子树的深度。

反之，右子树为空，左子树不为空，最小深度是 1 + 左子树的深度。 最后如果左右子树都不为空，返回左右子树深度最小值 + 1

### 如何处理：

**如果左子树空 右子树不为空： return 1 + 右子树的最小高度**

**如果右子树为空 左子树不为空： return 1 + 左子树的最小高度**

正确代码

```python
class Solution:
    def getDepth(self, node):
        if node is None:
            return 0
        leftDepth = self.getDepth(node.left)  # 左
        rightDepth = self.getDepth(node.right)  # 右
        
        # 当一个左子树为空，右不为空，这时并不是最低点
        if node.left is None and node.right is not None:
            return 1 + rightDepth
        
        # 当一个右子树为空，左不为空，这时并不是最低点
        if node.left is not None and node.right is None:
            return 1 + leftDepth
        
        result = 1 + min(leftDepth, rightDepth)
        return result

    def minDepth(self, root):
        return self.getDepth(root)
```

Use queue: when we had a node that does not have any child node, we get the level number. 

```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    def minDepth(self, root: Optional[TreeNode]) -> int:
        # logic is return min(left, right) + 1
        if not root:
            return 0
        queue = collections.deque([root])
        result = []
        while queue:
            level = []
            for _ in range(len(queue)):
                cur = queue.popleft()
                level.append(cur.val)
                if not cur.left and not cur.right:
                    return len(result) + 1
                if cur.left:
                    queue.append(cur.left)
                if cur.right:
                    queue.append(cur.right)
            result.append(level)

        
```