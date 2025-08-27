# DAY6 HASHING: 3 SUM, 4 SUM

康忙逆战逆战来也

# 1. 4sum 2

https://leetcode.com/problems/4sum-ii/submissions/1749580836/

Given four integer arrays `nums1`, `nums2`, `nums3`, and `nums4` all of length `n`, return the number of tuples `(i, j, k, l)` such that:

- `0 <= i, j, k, l < n`
- `nums1[i] + nums2[j] + nums3[k] + nums4[l] == 0`

**Example 1:**

```
Input: nums1 = [1,2], nums2 = [-2,-1], nums3 = [-1,2], nums4 = [0,2]
Output: 2
Explanation:
The two tuples are:
1. (0, 0, 0, 1) -> nums1[0] + nums2[0] + nums3[0] + nums4[1] = 1 + (-2) + (-1) + 2 = 0
2. (1, 1, 0, 0) -> nums1[1] + nums2[1] + nums3[0] + nums4[0] = 2 + (-1) + (-1) + 0 = 0

```

**Example 2:**

```
Input: nums1 = [0], nums2 = [0], nums3 = [0], nums4 = [0]
Output: 1
```

找到这四个数组里每个取一个数字 四个数相加之和等于0 的 组合个数。 

# 心得

这道题理解起来不难。 在hash 的问题里要反应过来， 

1. 这道题用什么数据结构存放？ 用hash 因为可以存 sum 和 出现的次数， dictionary 存
2. 那么dict 里面key value 都是什么？ 本题要求四个array 之和， 如果用暴力解法那就4个four loop， time complexity 爆炸。
3. 主要思路：
    - 定义一个dictionary， for a in nums1, for b in nums b, key 放 a + b， value 放 a + b 出现的次数，也就是frequency， 这样做的目的是可以在遍历 num3 num4 两个array 的时候找字典里有没有0-c-d 的key。 如果有， count + **字典里的value**，也就是出现的次数。 妙啊
    - 把四个array 两个 两个遍历。 先遍历前两个array， 统计两个数组元素之和，和出现的次数，放到dic中
    - count 用来统计a+b+c+d = 0 的个数。
    - 遍历后两个array， 找到如果 0-(c+d) 在dic中出现过的话，**就用count把dic中key对应的value也就是出现次数统计出来。注意：这一不是count 加一，而是加入互补sum 出现的frequency！！！**
    
    最后返回统计值 count 就可以了
    

```python
class Solution:
    def fourSumCount(self, nums1: List[int], nums2: List[int], nums3: List[int], nums4: List[int]) -> int:
        from collections import defaultdict
        dic = defaultdict(int)
        for a in nums1:
            for b in nums2:
                dic[a+b] += 1 
        count = 0 
        for c in nums3:
            for d in nums4:
                if 0-c-d in dic:
                    count += dic[0-c-d]
        return count
```

Time Complexity: O(n^2)

- 空间复杂度: O(n^2)，最坏情况下A和B的值各不相同，相加产生的数字个数为 n^2

---

# 2. Ransom Note

https://leetcode.com/problems/ransom-note/description/

Given two strings `ransomNote` and `magazine`, return `true` *if* `ransomNote` *can be constructed by using the letters from* `magazine` *and* `false` *otherwise*.

Each letter in `magazine` can only be used once in `ransomNote`.

**Example 1:**

```
Input: ransomNote = "a", magazine = "b"
Output: false

```

**Example 2:**

```
Input: ransomNote = "aa", magazine = "ab"
Output: false

```

**Example 3:**

```
Input: ransomNote = "aa", magazine = "aab"
Output: true

```

因为题目说只有小写字母，那可以采用空间换取时间的哈希策略，用一个长度为26的数组来记录magazine里字母出现的次数。

然后再用ransomNote去验证这个数组是否包含了ransomNote所需要的所有字母。

依然是数组在哈希法中的应用。

一些同学可能想，用数组干啥，都用map完事了，**其实在本题的情况下，使用map的空间消耗要比数组大一些的，因为map要维护红黑树或者哈希表，而且还要做哈希函数，是费时的！数据量大的话就能体现出来差别了。 所以数组更加简单直接有效！**

```python
class Solution:
    def canConstruct(self, ransomNote: str, magazine: str) -> bool:
        rs = [0] * 26 
        for chr in ransomNote:
            rs[ord(chr) - ord("a")] += 1
        for chr in magazine:
            rs[ord(chr) - ord("a")] -= 1 
        for i in range(26):
            if rs[i] > 0:
                return False 
        return True
```

- 时间复杂度: O(m+n)，其中m表示ransomNote的长度，n表示magazine的长度
- 空间复杂度: O(1)

using defaultdict:

```python
class Solution:
    def canConstruct(self, ransomNote: str, magazine: str) -> bool:
        from collections import defaultdict 
        rs_dic = defaultdict(int)
        mag_dic = defaultdict(int)
        for chr in ransomNote:
            rs_dic[chr] += 1
        for chr in magazine:
            mag_dic[chr] += 1
        for key in rs_dic:
            if key not in mag_dic:
                return False 
            if rs_dic[key] > mag_dic[key]:
                return False 
        return True
```

# 3. 3 sum

去重的部分要再好好理解一下

Given an integer array nums, return all the triplets `[nums[i], nums[j], nums[k]]` such that `i != j`, `i != k`, and `j != k`, and `nums[i] + nums[j] + nums[k] == 0`.

Notice that the solution set must not contain duplicate triplets.

**Example 1:**

```
Input: nums = [-1,0,1,2,-1,-4]
Output: [[-1,-1,2],[-1,0,1]]
Explanation:
nums[0] + nums[1] + nums[2] = (-1) + 0 + 1 = 0.
nums[1] + nums[2] + nums[4] = 0 + 1 + (-1) = 0.
nums[0] + nums[3] + nums[4] = (-1) + 2 + (-1) = 0.
The distinct triplets are [-1,0,1] and [-1,-1,2].
Notice that the order of the output and the order of the triplets does not matter.

```

**Example 2:**

```
Input: nums = [0,1,1]
Output: []
Explanation: The only possible triplet does not sum up to 0.

```

**Example 3:**

```
Input: nums = [0,0,0]
Output: [[0,0,0]]
Explanation: The only possible triplet sums up to 0.

```

**双指针**

拿这个nums数组来举例，首先将数组排序，然后有一层for循环，i从下标0的地方开始，同时定一个下标left 定义在i+1的位置上，定义下标right 在数组结尾的位置上。

依然还是在数组中找到 abc 使得a + b +c =0，我们这里相当于 a = nums[i]，b = nums[left]，c = nums[right]。

接下来如何移动left 和right呢， 如果nums[i] + nums[left] + nums[right] > 0 就说明 此时三数之和大了，因为数组是排序后了，所以right下标就应该向左移动，这样才能让三数之和小一些。

如果 nums[i] + nums[left] + nums[right] < 0 说明 此时 三数之和小了，left 就向右移动，才能让三数之和大一些，直到left与right相遇为止。

### **去重逻辑的思考**

### **a的去重**

说到去重，其实主要考虑三个数的去重。 a, b ,c, 对应的就是 nums[i]，nums[left]，nums[right]

a 如果重复了怎么办，a是nums里遍历的元素，那么应该直接跳过去。

如果我们的写法是 这样：

```
if (nums[i] == nums[i + 1]) { // 去重操作
    continue;
}

```

那我们就把 三元组中出现重复元素的情况直接pass掉了。 例如{-1, -1 ,2} 这组数据，当遍历到第一个-1 的时候，判断 下一个也是-1，那这组数据就pass了。

**我们要做的是 不能有重复的三元组，但三元组内的元素是可以重复的！**

所以这里是有两个重复的维度。

那么应该这么写：

```
if (i > 0 && nums[i] == nums[i - 1]) {
    continue;
}

```

这么写就是当前使用 nums[i]，我们判断前一位是不是一样的元素，在看 {-1, -1 ,2} 这组数据，当遍历到 第一个 -1 的时候，只要前一位没有-1，那么 {-1, -1 ,2} 这组数据一样可以收录到 结果集里。

这是一个非常细节的思考过程。

### [**#](https://programmercarl.com/0015.%E4%B8%89%E6%95%B0%E4%B9%8B%E5%92%8C.html#b%E4%B8%8Ec%E7%9A%84%E5%8E%BB%E9%87%8D)b与c的去重**

很多同学写本题的时候，去重的逻辑多加了 对right 和left 的去重：（代码中注释部分）

```
while (right > left) {
    if (nums[i] + nums[left] + nums[right] > 0) {
        right--;
        // 去重 right
        while (left < right && nums[right] == nums[right + 1]) right--;
    } else if (nums[i] + nums[left] + nums[right] < 0) {
        left++;
        // 去重 left
        while (left < right && nums[left] == nums[left - 1]) left++;
    } else {
    }
}

```

但细想一下，这种去重其实对提升程序运行效率是没有帮助的。

拿right去重为例，即使不加这个去重逻辑，依然根据 `while (right > left)` 和 `if (nums[i] + nums[left] + nums[right] > 0)` 去完成right-- 的操作。

多加了 `while (left < right && nums[right] == nums[right + 1]) right--;` 这一行代码，其实就是把 需要执行的逻辑提前执行了，但并没有减少 判断的逻辑。

最直白的思考过程，就是right还是一个数一个数的减下去的，所以在哪里减的都是一样的。

所以这种去重 是可以不加的。 仅仅是 把去重的逻辑提前了而已。

```python
class Solution:
    def threeSum(self, nums: List[int]) -> List[List[int]]:
        nums = sorted(nums)
        res = []
        print(nums)
        for i in range(len(nums)):
            if nums[0] > 0:
                return [] #排序过第一个数大于0 直接return
        #去重， 判断nums
            if i > 0 and nums[i] == nums[i-1]:
                continue 
            left = i + 1 
            right = len(nums) -1 
            while left < right:
                if nums[i] + nums[left] + nums[right] > 0:
                    right -= 1
                elif nums[i] + nums[left] + nums[right] < 0:
                    left += 1
                else:
                    res.append([nums[i], nums[left], nums[right]])
                    while right > left and nums[right] == nums[right - 1]:
                        right -= 1
                    while right > left and nums[left] == nums[left + 1]:
                        left += 1
                        
                    right -= 1
                    left += 1
        return res

```

---

# 4. 4sum

https://leetcode.com/problems/4sum/

## 题目描述

给定一个整数数组 `nums` 和一个目标值 `target`，返回所有 **不重复的四元组** `[a,b,c,d]`，使得：

```
a + b + c + d == target

```

要求：

- 不能包含重复的四元组
- 答案顺序不限

### 方法二：哈希 + 三重循环 (O(n³))

- 思路：先用哈希表统计每个数的频率
- 三重循环选出 `i,j,k`，然后计算需要的 `val = target - (nums[i]+nums[j]+nums[k])`
- 检查 `val` 是否在哈希表中，并且频率足够（避免用了同一个数超过次数）
- 使用 `set` 去重（四元组排序后存入 `set`）
- **时间复杂度**：O(n³)，比暴力好，但仍可能超时

### 方法三：排序 + 双指针 (推荐，O(n³))

1. 先对数组排序
2. 外层两层循环：固定前两个数 `i, j`
3. 内层双指针：`left, right` 找到满足条件的两个数
4. 去重：
    - 跳过相同的 `i`
    - 跳过相同的 `j`
    - 跳过相同的 `left/right`
5. 这样可以避免 `set`，直接得到不重复解

```python
class Solution:
    def fourSum(self, nums: List[int], target: int) -> List[List[int]]:
        # 创建一个字典来存储输入列表中每个数字的频率
        freq = {}
        for num in nums:
            freq[num] = freq.get(num, 0) + 1
        
        # 创建一个集合来存储最终答案，并遍历4个数字的所有唯一组合
        ans = set()
        for i in range(len(nums)):
            for j in range(i + 1, len(nums)):
                for k in range(j + 1, len(nums)):
                    val = target - (nums[i] + nums[j] + nums[k])
                    if val in freq:
                        # 确保没有重复
                        count = (nums[i] == val) + (nums[j] == val) + (nums[k] == val)
                        if freq[val] > count:
                            ans.add(tuple(sorted([nums[i], nums[j], nums[k], val])))
        
        return [list(x) for x in ans]
```

2 pointers: 

```python
class Solution:
    def fourSum(self, nums: List[int], target: int) -> List[List[int]]:
        nums.sort()
        n = len(nums)
        result = []
        for i in range(n):
            if nums[i] > target and nums[i] > 0 and target > 0:# 剪枝（可省）
                break
            if i > 0 and nums[i] == nums[i-1]:# 去重
                continue
            for j in range(i+1, n):
                if nums[i] + nums[j] > target and target > 0: #剪枝（可省）
                    break
                if j > i+1 and nums[j] == nums[j-1]: # 去重
                    continue
                left, right = j+1, n-1
                while left < right:
                    s = nums[i] + nums[j] + nums[left] + nums[right]
                    if s == target:
                        result.append([nums[i], nums[j], nums[left], nums[right]])
                        while left < right and nums[left] == nums[left+1]:
                            left += 1
                        while left < right and nums[right] == nums[right-1]:
                            right -= 1
                        left += 1
                        right -= 1
                    elif s < target:
                        left += 1
                    else:
                        right -= 1
        return result

```