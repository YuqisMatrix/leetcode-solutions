# DAY 5: Hashing 1: 2 sum

# 1. Hashing Summary

**当我们遇到了要快速判断一个元素是否出现集合里的时候，就要考虑哈希法**。

但是哈希法也是**牺牲了空间换取了时间**，因为我们要使用额外的数组，set或者是map来存放数据，才能实现快速的查找。

We can reduce time complexity by a factor of O(n) for a huge amount of problems. 

A hash map is an unordered data structure that stores key-value pairs. Add and remove elements in O(1), as well as update values associated with a key and check if a key exists, also in O(1). We can iterate both the key and values of a hash map, but the iteration won’t necessarily follow any order.

An ordered data structure is one where the insertion order is "remembered". An unordered data structure is one where the insertion order is not relevant.

### 1. 2. Comparison with arrays

In terms of time complexity, hash maps blow arrays out of the water. The following operations are all O(1) for a hash map:

- Add an element and associate it with a value
- Delete an element if it exists
- Check if an element exists

A hash map also has many of the same useful properties as an array with the same time complexity:

- Find length/number of elements
- Updating values
- Iterate over elements

if we do not know the max size of key, we should use hash maps, since the key will be converted to a new integer within the size limit anyways.

But any disadvantages of hashmaps?

1. for smaller input sizes, they can be slower due to overhead. Also collision 
2. Takes more space. Hash table also uses a fixed size array. But resizing a hash table is more expensive because every existing key needs to be re-hashed, and also a hash table may use an array that is significantly larger than the number of elements stored, resulting in a huge waste of space. 
    - when you don't know how many elements you need to store, arrays are more flexible with resizing and not wasting space. 比如有10 000 个元素，我们只需要存10 个， 但也许下一次就需要存100，000个元素呢？

 The constant time operations are only constant **relative to the size of the map**.

### 1.3 How to deal with collision?

Hash collisions occur when two different inputs produce the same hash value. Here are the main approaches to handle them:

### **拉链法 Separate Chaining (Open Hashing)**

刚刚小李和小王在索引1的位置发生了冲突，发生冲突的元素都被存储在链表中。 这样我们就可以通过索引找到小李和小王了

![哈希表4](https://file1.kamacoder.com/i/algo/20210104235015226.png)

（数据规模是dataSize， 哈希表的大小为tableSize）

其实拉链法就是要选择适当的哈希表的大小，这样既不会因为数组空值而浪费大量内存，也不会因为链表太长而在查找上浪费太多时间。

**Separate Chaining (Open Hashing)**

Each bucket in the hash table contains a linked list (or other data structure) to store multiple key-value pairs that hash to the same index.

```python
class HashTable:
    def __init__(self, size=10):
        self.size = size
        self.table = [[] for _ in range(size)]
    
    def hash_function(self, key):
        return hash(key) % self.size
    
    def insert(self, key, value):
        index = self.hash_function(key)
        bucket = self.table[index]
        
        *# Check if key already exists*
        for i, (k, v) in enumerate(bucket):
            if k == key:
                bucket[i] = (key, value)
                return
        
        *# Add new key-value pair*
        bucket.append((key, value))
    
    def get(self, key):
        index = self.hash_function(key)
        bucket = self.table[index]
        
        for k, v in bucket:
            if k == key:
                return v
        raise KeyError(key)

*# Example usage*
ht = HashTable(5)
ht.insert("apple", 10)
ht.insert("banana", 20)
ht.insert("grape", 30)  *# May collide with "apple" or "banana"*
```

### **Open Addressing (Closed Hashing) 线性探测法**

使用线性探测法，一定要保证tableSize大于dataSize。 我们需要依靠哈希表中的空位来解决碰撞问题。

例如冲突的位置，放了小李，那么就向下找一个空位放置小王的信息。所以要求tableSize一定要大于dataSize ，要不然哈希表上就没有空置的位置来存放 冲突的数据了。如图所示：

![哈希表5](https://file1.kamacoder.com/i/algo/20210104235109950.png)

## Open Addressing (Closed Hashing)

When a collision occurs, find another empty slot using a probing sequence.

### Linear Probing

```python
class LinearProbingHashTable:
    def __init__(self, size=10):
        self.size = size
        self.keys = [None] * size
        self.values = [None] * size
    
    def hash_function(self, key):
        return hash(key) % self.size
    
    def insert(self, key, value):
        index = self.hash_function(key)
        
        # Linear probing to find empty slot
        while self.keys[index] is not None:
            if self.keys[index] == key:
                self.values[index] = value  # Update existing
                return
            index = (index + 1) % self.size
        
        self.keys[index] = key
        self.values[index] = value
    
    def get(self, key):
        index = self.hash_function(key)
        
        while self.keys[index] is not None:
            if self.keys[index] == key:
                return self.values[index]
            index = (index + 1) % self.size
        
        raise KeyError(key)
```

---

当我们想使用哈希法来解决问题的时候，我们一般会选择如下三种数据结构。

- **数组**
- **set （集合）**
- **map(映射)**

Hash-based data structures provide **O(1) average time complexity** for all three fundamental operations: insertion, lookup, deletion. 

worst case O(n)

When checking existence in a **list**, the time complexity is **O(n)** - linear time. 

- **Array / List**
    - Checking existence (`x in arr`) requires scanning each element until a match is found.
    - **Time complexity:** **O(n)**
- **Hash Map / Hash Table / Dict / Set**
    - Uses a **hash function** to map the key directly to an index (bucket).
    - Existence check is just:
        1. Compute the hash → get bucket index.
        2. Look inside the bucket.
    - **Time complexity (average case):** **O(1)**
    - **Worst case:** **O(n)** if everything collides into the same bucket (rare if the hash function is good and table is sized properly).

所以是array 用index access 是 o(1), searching is o(n)

- **数组/列表**：
    - 下标访问 → O(1)
    - `in` 查存在性 → O(n)
- **集合/字典**：
    - `in` 查存在性 → O(1) 平均情况（哈希表）
- **字符串**：
    - 索引访问 → O(1)
    - 子串搜索 → O(m·n)

| 数据结构 | **按下标访问** | **`in` 操作 (查存在性)** | 说明 |
| --- | --- | --- | --- |
| **List (动态数组)** | O(1) | O(n) | 下标访问直接通过偏移量计算地址；`in` 要线性扫描 |
| **Set (哈希表实现)** | 不支持下标 | O(1) 平均，O(n) 最坏 | `in` 用哈希表查找，平均常数时间 |
| **Dict (哈希表实现)** | 不支持下标（只能通过 key） | O(1) 平均，O(n) 最坏 | `in` 检查的是 **key** 是否存在 |
| **String** | O(1)（单字符索引） | O(m·n) | 子串查找，n=原串长度，m=子串长度 |