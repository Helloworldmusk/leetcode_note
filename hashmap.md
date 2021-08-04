`哈希表`是一种使用`**哈希函数**`组织数据，以支持快速**插入和搜索**的数据结构。



有两种不同类型的哈希表：**哈希集合和哈希映射。**

哈希**集合**是集合数据结构的实现之一，用于存储**非重复值**。
哈希**映射**是映射 数据结构的实现之一，用于存储(key, value)键值对。



### 哈希表的原理

当我们插入一个新的键时，哈希函数将决定该键应该分配到哪个桶中，并将该键存储在相应的桶中；
当我们想要搜索一个键时，哈希表将使用相同的哈希函数来查找对应的桶，并只在特定的桶中进行搜索。



### 1. 哈希函数

散列函数将取决于`键值的范围`和`桶的数量。`

### 2. 冲突解决

让我们假设存储最大键数的桶有 N 个键。

通常，如果 N 是常数且很小，我们可以简单地使用一个数组将键存储在同一个桶中。如果 N 是可变的或很大，我们可能需要使用**高度平衡的二叉树**来代替.。



### 哈希集合的实现

```cpp
#define MAX_LEN 100000          // the amount of buckets
class MyHashSet {
private:
    vector<int> set[MAX_LEN];   // hash set implemented by array
    
    /** Returns the corresponding bucket index. */
    int getIndex(int key) {
        return key % MAX_LEN;
    }
    
    /** Search the key in a specific bucket. Returns -1 if the key does not existed. */
    int getPos(int key, int index) {
        // Each bucket contains a list. Iterate all the elements in the bucket to find the target key.
        for (int i = 0; i < set[index].size(); ++i) {
            if (set[index][i] == key) {
                return i;
            }
        }
        return -1;
    }
public:
    /** Initialize your data structure here. */
    MyHashSet() {
        
    }
    
    void add(int key) {
        int index = getIndex(key);
        int pos = getPos(key, index);
        if (pos < 0) {
            // Add new key if key does not exist.
            set[index].push_back(key);
        }
    }
    
    void remove(int key) {
        int index = getIndex(key);
        int pos = getPos(key, index);
        if (pos >= 0) {
            // Remove the key if key exists.
            set[index].erase(set[index].begin() + pos);
        }
    }
    
    /** Returns true if this set did not already contain the specified element */
    bool contains(int key) {
        int index = getIndex(key);
        int pos = getPos(key, index);
        return pos >= 0;
    }
};
```



### 哈希映射的实现

```cpp
#define MAX_LEN 100000            // the amount of buckets

class MyHashMap {
private:
    vector<pair<int, int>> map[MAX_LEN];       // hash map implemented by array
    
    /** Returns the corresponding bucket index. */
    int getIndex(int key) {
        return key % MAX_LEN;
    }
    
    /** Search the key in a specific bucket. Returns -1 if the key does not existed. */
    int getPos(int key, int index) {
        // Each bucket contains a vector. Iterate all the elements in the bucket to find the target key.
        for (int i = 0; i < map[index].size(); ++i) {
            if (map[index][i].first == key) {
                return i;
            }
        }
        return -1;
    }
    
public:
    /** Initialize your data structure here. */
    MyHashMap() {
        
    }
    
    /** value will always be positive. */
    void put(int key, int value) {
        int index = getIndex(key);
        int pos = getPos(key, index);
        if (pos < 0) {
            map[index].push_back(make_pair(key, value));
        } else {
            map[index][pos].second = value;
        }
    }
    
    /** Returns the value to which the specified key is mapped, or -1 if this map contains no mapping for the key */
    int get(int key) {
        int index = getIndex(key);
        int pos = getPos(key, index);
        if (pos < 0) {
            return -1;
        } else {
            return map[index][pos].second;
        }
    }
    
    /** Removes the mapping of the specified value key if this map contains a mapping for the key */
    void remove(int key) {
        int index = getIndex(key);
        int pos = getPos(key, index);
        if (pos >= 0) {
            map[index].erase(map[index].begin() + pos);
        }
    }
};
```



### 删除操作：

在找到元素的位置之后，我们需要从数组列表中删除元素。

假设我们要删除第 i 个元素，并且数组列表的大小为 n。

内置函数中使用的策略是把第 i 个元素后的所有元素向前移动一个位置。也就是说，你必须移动 n - i 次。因此，从数组列表中删除元素的时间复杂度将为 O(n)。

考虑 i 取不同值的情况。平均而言，我们将移动 ((n - 1) + (n - 2) + ... + 1 + 0) / n = (n - 1) / 2 次。

**希望有两种解决方案可以将时间复杂度从 O(n) 降低到 O(1)。**

1. 交换

我们可以使用一种巧妙的策略。首先，用存储桶中的最后一个元素交换要移除的元素。然后删除最后一个元素。通过这种方法，我们成功地在 O(1) 的时间复杂度中去除了元素。

2. 链表

实现此目标的另一种方法是使用链表而不是数组列表。通过这种方式，我们可以在不修改列表中的顺序的情况下删除元素。该策略时间复杂度为 O(1)。

以上两种方式不利于查询，如果在框里的元素有序存放，则认为更加方便查询；如果主要涉及的就是删除业务，那么交换或者链表可行，如果主要涉及的是查询业务，则使用有序的 序列式容器是更好的选择；



**复杂度分析**
如果总共有 M 个键，那么在使用哈希表时，可以很容易地达到 O(M) 的空间复杂度。

但是，你可能已经注意到哈希表的时间复杂度与设计有很强的关系。

我们中的大多数人可能已经在每个桶中使用数组来将值存储在同一个桶中，理想情况下，桶的大小足够小时，可以看作是一个常数。插入和搜索的时间复杂度都是 O(1)。

但在最坏的情况下，桶大小的最大值将为 N。插入时时间复杂度为 O(1)，搜索时为 O(N)



**内置哈希表的原理**
内置哈希表的典型设计是：

 键值可以是任何可哈希化的类型。并且属于可哈希类型的值将具有哈希码。此哈希码将用于映射函数以获取存储区索引。
 每个桶包含一个数组，用于在初始时将所有值存储在同一个桶中。
 如果在同一个桶中有太多的值，这些值将被保留在一个**高度平衡的二叉树搜索树**中。
插入和搜索的平均时间复杂度仍为 O(1)。最坏情况下插入和搜索的时间复杂度是 O(logN)，使用高度平衡的 BST。这是在插入和搜索之间的一种权衡。



### **哈希集基本操作**

```cpp
#include <unordered_set>                // 0. include the library

int main() {
    // 1. initialize a hash set
    unordered_set<int> hashset;   
    // 2. insert a new key
    hashset.insert(3);
    hashset.insert(2);
    hashset.insert(1);
    // 3. delete a key
    hashset.erase(2);
    // 4. check if the key is in the hash set
    if (hashset.count(2) <= 0) {
        cout << "Key 2 is not in the hash set." << endl;
    }
    // 5. get the size of the hash set
    cout << "The size of hash set is: " << hashset.size() << endl; 
    // 6. iterate the hash set
    for (auto it = hashset.begin(); it != hashset.end(); ++it) {
        cout << (*it) << " ";
    }
    cout << "are in the hash set." << endl;
    // 7. clear the hash set
    hashset.clear();
    // 8. check if the hash set is empty
    if (hashset.empty()) {
        cout << "hash set is empty now!" << endl;
    }
}
```

### 哈希映射基本操作

```cpp
#include <unordered_map>                // 0. include the library

int main() {
    // 1. initialize a hash map
    unordered_map<int, int> hashmap;
    // 2. insert a new (key, value) pair
    hashmap.insert(make_pair(0, 0));
    hashmap.insert(make_pair(2, 3));
    // 3. insert a new (key, value) pair or update the value of existed key
    hashmap[1] = 1;
    hashmap[1] = 2;
    // 4. get the value of a specific key
    cout << "The value of key 1 is: " << hashmap[1] << endl;
    // 5. delete a key
    hashmap.erase(2);
    // 6. check if a key is in the hash map
    if (hashmap.count(2) <= 0) {
        cout << "Key 2 is not in the hash map." << endl;
    }
    // 7. get the size of the hash map
    cout << "the size of hash map is: " << hashmap.size() << endl; 
    // 8. iterate the hash map
    for (auto it = hashmap.begin(); it != hashmap.end(); ++it) {
        cout << "(" << it->first << "," << it->second << ") ";
    }
    cout << "are in the hash map." << endl;
    // 9. clear the hash map
    hashmap.clear();
    // 10. check if the hash map is empty
    if (hashmap.empty()) {
        cout << "hash map is empty now!" << endl;
    }
}
```





#### [1. 两数之和](https://leetcode-cn.com/problems/two-sum/)

难度简单11698收藏分享切换为英文接收动态反馈

给定一个整数数组 `nums` 和一个整数目标值 `target`，请你在该数组中找出 **和为目标值** *`target`* 的那 **两个** 整数，并返回它们的数组下标。

你可以假设每种输入只会对应一个答案。但是，数组中同一个元素在答案里不能重复出现。

你可以按任意顺序返回答案。

 

**示例 1：**

```
输入：nums = [2,7,11,15], target = 9
输出：[0,1]
解释：因为 nums[0] + nums[1] == 9 ，返回 [0, 1] 。
```

**示例 2：**

```
输入：nums = [3,2,4], target = 6
输出：[1,2]
```

**示例 3：**

```
输入：nums = [3,3], target = 6
输出：[0,1]
```

 

**提示：**

- `2 <= nums.length <= 104`
- `-109 <= nums[i] <= 109`
- `-109 <= target <= 109`
- **只会存在一个有效答案**

**进阶：**你可以想出一个时间复杂度小于 `O(n2)` 的算法吗？



目标： 返回一个vector, 包含两个元素，和为目标值；

条件： 寻找和为目标值的两个节点的值；

方案： 直接使用哈希表，数组元素；

**考虑使用哈希表的一种情况：使用外部的元素和哈希表中的元素进行比较**

#### [594. 最长和谐子序列](https://leetcode-cn.com/problems/longest-harmonious-subsequence/)

难度简单180收藏分享切换为英文接收动态反馈

和谐数组是指一个数组里元素的最大值和最小值之间的差别 **正好是 `1`** 。

现在，给你一个整数数组 `nums` ，请你在所有可能的子序列中找到最长的和谐子序列的长度。

数组的子序列是一个由数组派生出来的序列，它可以通过删除一些元素或不删除元素、且不改变其余元素的顺序而得到。

 

**示例 1：**

```
输入：nums = [1,3,2,2,5,2,3,7]
输出：5
解释：最长的和谐子序列是 [3,2,2,2,3]
```

**示例 2：**

```
输入：nums = [1,2,3,4]
输出：2
```

**示例 3：**

```
输入：nums = [1,1,1,1]
输出：0
```

 

**提示：**

- `1 <= nums.length <= 2 * 104`
- `-109 <= nums[i] <= 109`

​			

目标：找到最长的和谐子序列长度，，，

条件：最大最小值正好差1；

​	可以通过删除或者不删除得到一个子序列；



思路1：使用hash 键值对，键表示数组中的元素，值表示这个元素出现的出现一次加1；

```cpp
class Solution {
public:
    int findLHS(vector<int>& nums) {
        //create hash map;
        std::unordered_map<int,int> hash_map(nums.size() + 1);
        for(auto c : nums)
        {
            if(hash_map.count(c))
            {
                hash_map[c]+=1;
            }
            else
            {
                hash_map.emplace(c, 1); 
            }
        }
        //find the max value;
        int max_length_sum = 0;
        for(auto it = hash_map.begin(); it != hash_map.end(); it++)
        {
            auto it_next = hash_map.find(it->first + 1);
            if (it_next != hash_map.end())
            {
                if(max_length_sum < it->second + it_next->second)
                {
                    max_length_sum = it->second + it_next->second;
                }
            }
        }
        return max_length_sum;
    }
};
```

**一定要注意的是，哈希表中的键不一定是按照常规顺序存放的，比如可能就不是按照1、2 、3、等等这种方式存放的；**

#### [128. 最长连续序列](https://leetcode-cn.com/problems/longest-consecutive-sequence/)

难度中等853收藏分享切换为英文接收动态反馈

给定一个未排序的整数数组 `nums` ，找出数字连续的最长序列（不要求序列元素在原数组中连续）的长度。

请你设计并实现时间复杂度为 `O(n)` 的算法解决此问题。

 

**示例 1：**

```
输入：nums = [100,4,200,1,3,2]
输出：4
解释：最长数字连续序列是 [1, 2, 3, 4]。它的长度为 4。
```

**示例 2：**

```
输入：nums = [0,3,7,2,5,8,4,6,0,1]
输出：9
```

 

**提示：**

- `0 <= nums.length <= 105`
- `-109 <= nums[i] <= 109`



解决方案：

1. 将数组构建hash_set,键为数组元素的值，第二次遍历整个键值，判断下一个是否连续；如果连续就加1；

   总体来说，两步，第一步构建hashset, 第二步，依据键值，判断连续性，并且返回最大连续值；

```cpp
class Solution {
public:
    int longestConsecutive(vector<int>& nums) {
        std::unordered_set<int> hash_set(nums.size());
        for(auto c : nums)
        {
            hash_set.insert(c);
        }
        int max_length = 0;
        for(auto it = hash_set.begin(); it != hash_set.end(); )
        {
            int lenght = 1;
            if(hash_set.find((*it) - 1)  != hash_set.end())
            {
                it++;
                continue;
            }
            auto temp_it = it;
            while(hash_set.find((*temp_it) + 1) != hash_set.end())
            {       
                temp_it = hash_set.find((*temp_it) + 1);
                lenght++;
            }
            if(lenght > max_length)
            {
                max_length = lenght;
            }
            it++;
        }
        return max_length;
    }
};
```



**这个题的核心思想就是从一个串的最小的值开始找，大于最小值的都直接跳过**

这种也适合其他类似找最长顺序子串的情况；























