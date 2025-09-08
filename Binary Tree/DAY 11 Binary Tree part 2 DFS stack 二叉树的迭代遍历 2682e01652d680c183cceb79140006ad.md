# DAY 11 Binary Tree part 2 DFS stack 二叉树的迭代遍历

# 1. Use stack to traverse Binary Tree

### Preorder: root, left, right

前序遍历是中左右，**每次先处理的是中间节点，那么先将根节点放入栈中，然后将右孩子加入栈，再加入左孩子**

### Why push right before left?

- Stack = **Last In, First Out (LIFO)**.
- If we push `left` first, then `right`, the right child would be popped **before** the left (wrong order).
- So we push `right` → then `left`. That way, left is processed first

```python
        5
       / \
      4   6
     / \
    1   2

```

### Preorder Traversal (Root → Left → Right) with stack

**Steps:**

1. Start: stack = `[5]`, res = `[]`
2. Pop 5 → res = `[5]`, push (6,4) → stack = `[6,4]`
3. Pop 4 → res = `[5,4]`, push (2,1) → stack = `[6,2,1]`
4. Pop 1 → res = `[5,4,1]` → stack = `[6,2]`
5. Pop 2 → res = `[5,4,1,2]` → stack = `[6]`
6. Pop 6 → res = `[5,4,1,2,6]`

```python
# Preorder with stack
def preorderTraversal(root):
    if not root:
        return []
    stack = [root]
    res = []
    while stack:
        node = stack.pop()
        res.append(node.val)
        if node.right:
            stack.append(node.right)
        if node.left:
            stack.append(node.left)
    return res

print(preorderTraversal(root))
```

先访问root, 先处理的也是root，

---

### Inorder: left, root, right

先从非根节点访问 那应该咋办

```python
        5
       / \
      4   6
     / \
    1   2
```

1,2,3,6,5

**在使用迭代法写中序遍历，就需要借用指针的遍历来帮助访问节点，栈则用来处理节点上的元素。**

指针一路往左走把沿途节点压栈，走到头后开始出栈访问节点并转向右子树，如此循环直到栈空且指针为空。

**一路访问到左叶子节点， 如果左叶子节点为空，就从栈里取元素。栈里是之前访问过的元素，转向右子树**

```python
# 中序遍历-迭代-LC94_二叉树的中序遍历
class Solution:
    def inorderTraversal(self, root: TreeNode) -> List[int]:

        if not root:
            return []
        stack = []  # 不能提前将root节点加入stack中

        result = []
        cur = root
        while cur or stack:
            # 先迭代访问最底层的左子树节点
            if cur:     
                stack.append(cur)
                cur = cur.left		
            # 到达最左节点后处理栈顶节点    
            else:		
                cur = stack.pop()
                result.append(cur.val)
                # 取栈顶元素右节点
                cur = cur.right	
        return result
```

---

### Postorder: left, right, root

再来看后序遍历，先序遍历是中左右，后序遍历是左右中，那么我们只需要调整一下先序遍历的代码顺序，就变成中右左的遍历顺序，然后在反转result数组，输出的结果顺序就是左右中了，如下图：

![前序到后序](https://file1.kamacoder.com/i/algo/20200808200338924.png)

preorder: root, left, right. 调整一下stack push 的顺序： root, right, left, and then reverse, we got: left, right, root

```python
# 后序遍历-迭代-LC145_二叉树的后序遍历
class Solution:
    def postorderTraversal(self, root: TreeNode) -> List[int]:
        if not root:
            return []
        stack = [root]
        result = []
        while stack:
            node = stack.pop()
            # 中节点先处理
            result.append(node.val)
            # 左孩子先入栈
            if node.left:
                stack.append(node.left)
            # 右孩子后入栈
            if node.right:
                stack.append(node.right)
        # 将最终的数组翻转
        return result[::-1]
```