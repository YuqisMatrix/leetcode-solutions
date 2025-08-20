# Array Algorithms Study Guide

## August 20 Study Notes

1. Today I reviewed basic array operations and clarified that binary search requires defining intervals to determine left and right assignments. There are two methods: left-closed-right-open and left-closed-right-closed.
2. Reviewed LC 27 using two pointers to remove elements from an array. Still need to consolidate understanding of how to assign values based on the right pointer.
3. LC 977 squares of a sorted array. Given a sorted array, sort by their squares using two pointers. The largest absolute values are definitely at the ends, so use three pointers to solve, filling the answer array from back to front, then updating.

---

## 1. Array Basics

- **Arrays are collections of the same data type stored in contiguous memory space**
- **Array elements cannot be deleted, only overwritten**

### Are 2D arrays stored contiguously?

- **C++**: 2D arrays are contiguous in memory, verifiable through addresses: 0x7ffee4065820 0x7ffee4065824 0x7ffee4065828
- **Java**: No pointers, 2D arrays are arrays of arrays, each row's storage location is irregular and non-contiguous.

---

## 2. Binary Search (LC 704)

[LeetCode Link](https://leetcode.com/problems/binary-search/)

### Pre-conditions

1. Array is sorted
2. No duplicate elements

Binary search boundary errors are common. Remember: **interval definition is the loop invariant**.

Binary search boundaries often cause mistakes. For example, should it be `while(left < right)` or `while(left <= right)`? Should it be `right = middle` or `right = middle - 1`?

**The interval definition is the invariant**. During binary search, we must maintain the invariant, meaning every boundary handling in the while loop must consistently follow the interval definition. This is the **loop invariant** rule.

Two common interval definitions:
- Left-closed-right-closed `[left, right]`
- Left-closed-right-open `[left, right)`

### 2.1 Left-closed-right-closed [left, right]

- **Initialization**: `left = 0, right = len(nums) - 1`
- **Loop condition**: `while left <= right`
- **Update rules**:
  - `if nums[middle] > target: right = middle - 1`
  - `elif nums[middle] < target: left = middle + 1`
  - `else: return middle`

**Key points:**
- `left = 0`
- `right = nums.size - 1`
- `while (left <= right)` Must use `<=`, because `left == right` is meaningful, so use ≤

**How to handle in the loop**: 
- `if nums[middle] > target: right = middle - 1`. Why? Because the middle number is already larger than target, so we need to adjust right. The ending index for the next left interval search should be `middle - 1`.
- `elif nums[middle] < target: left = middle + 1`
- `else return middle`
- `return -1`

```python
class Solution:
    def search(self, nums: List[int], target: int) -> int:
        left, right = 0, len(nums) - 1  # Left-closed-right-closed interval
        while left <= right:
            middle = (left + right) // 2
            if nums[middle] > target:
                right = middle - 1
            elif nums[middle] < target:
                left = middle + 1
            else:
                return middle
        return -1
```

### 2.2 Left-closed-right-open [left, right)

- **Initialization**: `left = 0, right = len(nums)`
- **Loop condition**: `while left < right`
- **Update rules**:
  - `if nums[middle] > target: right = middle`
  - `elif nums[middle] < target: left = middle + 1`
  - `else: return middle`

**Key points:**
- `left = 0`
- `right = nums.size` (because right-open doesn't include the right boundary)
- `while (left < right)`, use `<` here, because `left == right` has no meaning in interval `[left, right)`

**How to handle in the loop**:
- `if nums[middle] > target: right = middle`. Because left-closed-right-open doesn't include the right value, the search interval already doesn't include right. Assign right based on the interval.
- `nums[middle] < target: left = middle + 1`, because it includes the left side, middle is already not the target so search from `middle + 1`.

```python
class Solution:
    def search(self, nums: List[int], target: int) -> int:
        left, right = 0, len(nums)  # Left-closed-right-open interval
        while left < right:
            middle = (left + right) // 2
            if nums[middle] > target:
                right = middle
            elif nums[middle] < target:
                left = middle + 1
            else:
                return middle
        return -1
```

- **Time Complexity**: O(log n)
- **Space Complexity**: O(1)

---

## 3. Remove Element (LC 27)

[LeetCode Link](https://leetcode.com/problems/remove-element/)

### Requirements

- Remove all elements equal to `val` in-place
- Return the new array length `k` after removal
- The first `k` elements should not equal `val`

### 3.1 Brute Force (O(n²))

```python
class Solution:
    def removeElement(self, nums: List[int], val: int) -> int:
        i, l = 0, len(nums)
        while i < l:
            if nums[i] == val: 
                for j in range(i+1, l):
                    nums[j - 1] = nums[j]
                l -= 1
                i -= 1
            i += 1
        return l
```

### 3.2 Two Pointers (O(n))

- **Fast pointer**: Traverses the entire array, looking for elements not equal to `val`
- **Slow pointer**: Marks the position of the new array

```python
class Solution:
    def removeElement(self, nums: List[int], val: int) -> int:
        fast, slow = 0, 0
        size = len(nums)
        while fast < size:
            if nums[fast] != val:
                nums[slow] = nums[fast]
                slow += 1
            fast += 1
        return slow
```

---

## 4. Squares of a Sorted Array (LC 977)

[LeetCode Link](https://leetcode.com/problems/squares-of-a-sorted-array/)

### Approach

- Array is sorted, but after squaring, negative numbers might become larger
- Maximum values are definitely at the ends
- Use **three pointers**:
  - `left = 0`
  - `right = len(nums) - 1`
  - `last = len(nums) - 1` (fill result from back to front)

### Steps

1. Compare `nums[left]^2` with `nums[right]^2`
2. Place the larger one into `ans[last]`
3. Move the corresponding pointer, `last -= 1`
4. Loop condition: `while left <= right` (must include equal sign to handle middle element)

```python
from typing import List

class Solution:
    def sortedSquares(self, nums: List[int]) -> List[int]:
        left, right = 0, len(nums) - 1
        last = len(nums) - 1
        ans = [0] * len(nums)
        
        while left <= right:
            lsq, rsq = nums[left] ** 2, nums[right] ** 2
            if lsq < rsq:
                ans[last] = rsq
                right -= 1
            else:
                ans[last] = lsq
                left += 1
            last -= 1
        return ans
```