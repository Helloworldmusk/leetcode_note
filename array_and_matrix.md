#### [283. 移动零](https://leetcode-cn.com/problems/move-zeroes/)

难度简单1160

给定一个数组 `nums`，编写一个函数将所有 `0` 移动到数组的末尾，同时保持非零元素的相对顺序。

**示例:**

```
输入: [0,1,0,3,12]
输出: [1,3,12,0,0]
```

**说明**:

1. 必须在原数组上操作，不能拷贝额外的数组。
2. 尽量减少操作次数。

target : 只需修改，不需要返回；

条件： 所有的0 都在数组的末尾；

思考：双指针不一定是双向指针，也可能是同向指针；

```cpp
class Solution {
public:
    void moveZeroes(vector<int>& nums) {
        int left_index = 0;
        int right_index = 0;
        while(right_index < nums.size())
        {
            while(left_index < nums.size() &&  nums[left_index] != 0)
            {
                left_index++;
            }
            if(left_index > right_index)
            {
                right_index = left_index;
            }
            while(right_index < nums.size() && nums[right_index] == 0)
            { 
                right_index++;
            }
            if(right_index < nums.size())
            {
                nums[left_index] = nums[right_index];
                nums[right_index] = 0;
            }
        }
    }
};

//官方题解：写的很简单，但是不直观，不推荐；
class Solution {
public:
    void moveZeroes(vector<int>& nums) {
        int n = nums.size(), left = 0, right = 0;
        while (right < n) {
            if (nums[right]) {
                swap(nums[left], nums[right]);
                left++;
            }
            right++;
        }
    }
};

```

双指针存在的意义在于维护三个序列；双指针可以把整个数组分成三份，第一份是已经符合要求的序列，第二份是0序列，第三份是还需要检索的序列；

对于一个要求把元素分成两组的这种情况，考虑使用双指针；



bugfree, quick , 注重大思路的正确性以及高效性；

#### [566. 重塑矩阵](https://leetcode-cn.com/problems/reshape-the-matrix/)

难度简单224

在 MATLAB 中，有一个非常有用的函数 `reshape` ，它可以将一个 `m x n` 矩阵重塑为另一个大小不同（`r x c`）的新矩阵，但保留其原始数据。

给你一个由二维数组 `mat` 表示的 `m x n` 矩阵，以及两个正整数 `r` 和 `c` ，分别表示想要的重构的矩阵的行数和列数。

重构后的矩阵需要将原始矩阵的所有元素以相同的 **行遍历顺序** 填充。

如果具有给定参数的 `reshape` 操作是可行且合理的，则输出新的重塑矩阵；否则，输出原始矩阵。

 

**示例 1：**

![img](typora_image/reshape1-grid.jpg)

```
输入：mat = [[1,2],[3,4]], r = 1, c = 4
输出：[[1,2,3,4]]
```

**示例 2：**

![img](typora_image/reshape2-grid.jpg)

```
输入：mat = [[1,2],[3,4]], r = 2, c = 4
输出：[[1,2],[3,4]]
```

 

**提示：**

- `m == mat.length`
- `n == mat[i].length`
- `1 <= m, n <= 100`
- `-1000 <= mat[i][j] <= 1000`
- `1 <= r, c <= 300`

方案1： 首先检查输入和要求输出之间的大小是否相等；

​				然后相等的情况下，依次读取原始矩阵的元素，存入新的二维数组中；

​				换行的问题： 只要添加次数超过c，把这个vector加入到二维vector中；

​										新建一个vector;

应该是没有第二方案？

```cpp
//个人解法：
//bugfree, quick;
class Solution {
public:
    vector<vector<int>> matrixReshape(vector<vector<int>>& mat, int r, int c) {
        if(mat.size() == 0)
        {
            return mat;    
        }
        if(mat.size() * mat[0].size() != r * c) 
        {
            return mat;
        }
        vector<vector<int>> result;
        int row { 0 };
        int col { 0 };
        for(int i = 0; i < r; i++)
        {
            vector<int> new_vector;
            for(int j = 0; j < c; j++)
            {
                if(col == mat[row].size())
                {
                    col = 0;
                    row++;
                }
                new_vector.push_back(mat[row][col]);
                col++;
            }
            result.push_back(new_vector);
        }
        return result;
    }
};
```

官方思路：

根据已知大小，可以直接构建目标矩阵，而且只要两个矩阵的大小能够确定，那么两个矩阵之间的映射关系就是一定的，**可以根据这个映射关系直接赋值；(这是自己没有想到的，要联想两个矩阵之间的关系，要联系 已知 和未知之间的关系)**

```cpp
对于 x \in [0, mn)x∈[0,mn)，第 xx 个元素在 \textit{nums}nums 中对应的下标为 (x ~/~ n, x~\%~ n)(x / n,x % n)，而在新的重塑矩阵中对应的下标为 (x ~/~ c, x~\%~ c)(x / c,x % c)。我们直接进行赋值即可。

class Solution {
public:
    vector<vector<int>> matrixReshape(vector<vector<int>>& nums, int r, int c) {
        int m = nums.size();
        int n = nums[0].size();
        if (m * n != r * c) {
            return nums;
        }

        vector<vector<int>> ans(r, vector<int>(c));
        for (int x = 0; x < m * n; ++x) {
            ans[x / c][x % c] = nums[x / n][x % n];
        }
        return ans;
    }
};

```



#### [485. 最大连续 1 的个数](https://leetcode-cn.com/problems/max-consecutive-ones/)

难度简单255(很简单，直接过)

给定一个二进制数组， 计算其中最大连续 1 的个数。

 

**示例：**

```
输入：[1,1,0,1,1,1]
输出：3
解释：开头的两位和最后的三位都是连续 1 ，所以最大连续 1 的个数是 3.
```

 

**提示：**

- 输入的数组只包含 `0` 和 `1` 。
- 输入数组的长度是正整数，且不超过 10,000。



target: 输出最大连续1 的个数；

条件： 二进制数组，只包含0和1；要求连续1的最大个数；

思路，依次遍历每个元素，值是1，累加局部变量，元素是0，对比局部遍历和全局最大值，局部变量清零，结束时，也对比局部变量和最大值；

```cpp
//个人解法：
class Solution {
public:
    int findMaxConsecutiveOnes(vector<int>& nums) {
        int max { 0 };
        int local_cnt { 0 };
        for( auto x : nums)
        {
            if(x == 0)
            {
                if (local_cnt > max) { max = local_cnt; }
                local_cnt = 0;
            }
            else
            {
                local_cnt++;
            }
        }
        if(local_cnt > max) {max = local_cnt; }
        return max;
    }
};
```





#### [240. 搜索二维矩阵 II](https://leetcode-cn.com/problems/search-a-2d-matrix-ii/)

难度中等681

编写一个高效的算法来搜索 `*m* x *n*` 矩阵 `matrix` 中的一个目标值 `target` 。该矩阵具有以下特性：

- 每行的元素从左到右升序排列。
- 每列的元素从上到下升序排列。

 

**示例 1：**

![img](typora_image/searchgrid2.jpg)

```
输入：matrix = [[1,4,7,11,15],[2,5,8,12,19],[3,6,9,16,22],[10,13,14,17,24],[18,21,23,26,30]], target = 5
输出：true
```

**示例 2：**

![img](typora_image/searchgrid.jpg)

```
输入：matrix = [[1,4,7,11,15],[2,5,8,12,19],[3,6,9,16,22],[10,13,14,17,24],[18,21,23,26,30]], target = 20
输出：false
```

 

**提示：**

- `m == matrix.length`
- `n == matrix[i].length`
- `1 <= n, m <= 300`
- `-109 <= matix[i][j] <= 109`
- 每行的所有元素从左到右升序排列
- 每列的所有元素从上到下升序排列
- `-109 <= target <= 109`



返回值要求： 返回布尔值，是否含有目标值；

条件： 从左向右递增；

​			从上到下递增；

思路1：找到矩阵的规律：从左到右递增，从上到下递增，所以，第一列 + 最后一行的方式，左右数组元素都是递增的；找到数组的特征是本题的关键；

```cpp
//参考官方解法；
class Solution {
public:
    bool searchMatrix(vector<vector<int>>& matrix, int target) {
        if(matrix.size() == 0)
        {
            return false;
        }
        int s_row = matrix.size()-1;
        int s_col = 0;
        while((s_row >= 0) && (s_row < matrix.size()) && (s_col >= 0) && (s_col < matrix[0].size()))
        {
            if(matrix[s_row][s_col] > target)
            {
                s_row--;
            }
            else if (matrix[s_row][s_col] < target)
            {
                s_col++;
            }
            else
            {
                return true;
            }
        }
        return false;
    }
};
```



#### [378. 有序矩阵中第 K 小的元素](https://leetcode-cn.com/problems/kth-smallest-element-in-a-sorted-matrix/)

难度中等635

给你一个 `n x n` 矩阵 `matrix` ，其中每行和每列元素均按升序排序，找到矩阵中第 `k` 小的元素。
请注意，它是 **排序后** 的第 `k` 小元素，而不是第 `k` 个 **不同** 的元素。

 

**示例 1：**

```
输入：matrix = [[1,5,9],[10,11,13],[12,13,15]], k = 8
输出：13
解释：矩阵中的元素为 [1,5,9,10,11,12,13,13,15]，第 8 小元素是 13
```

**示例 2：**

```
输入：matrix = [[-5]], k = 1
输出：-5
```

 

**提示：**

- `n == matrix.length`
- `n == matrix[i].length`
- `1 <= n <= 300`
- `-109 <= matrix[i][j] <= 109`
- 题目数据 **保证** `matrix` 中的所有行和列都按 **非递减顺序** 排列
- `1 <= k <= n2`



根据官方解法总结自己的思路：

首先取左上角和右下角分别为min 和 max， 取二者的中值min = (max + min) / 2;，检测比中值小的个数cnt， 如果比中值小的个数小于k， 则说明目标值在 mid 和 max 之间，取min = left + 1；  如果 比中值小的个数cnt 大于等于k，且没有发现 目标值，则说明目标值 在min 和 mid 之间，如果发现目标值，直接返回，去max = mid-1; 如果比中值小的个数等于cnt, 则检测

**有难度，暂时搁置；**





#### [645. 错误的集合](https://leetcode-cn.com/problems/set-mismatch/)

难度简单223

集合 `s` 包含从 `1` 到 `n` 的整数。不幸的是，因为数据错误，导致集合里面某一个数字复制了成了集合里面的另外一个数字的值，导致集合 **丢失了一个数字** 并且 **有一个数字重复** 。

给定一个数组 `nums` 代表了集合 `S` 发生错误后的结果。

请你找出重复出现的整数，再找到丢失的整数，将它们以数组的形式返回。

 

**示例 1：**

```
输入：nums = [1,2,2,4]
输出：[2,3]
```

**示例 2：**

```
输入：nums = [1,1]
输出：[1,2]
```

 

**提示：**

- `2 <= nums.length <= 104`
- `1 <= nums[i] <= 104`

思路，使用一个vector, 对应存储相应的元素，如果这个元素存在了，则记录在结果中，最后遍历vector, 检索出没有被赋值的索引，索引值加1，就是丢失的值；

```cpp
//个人解法
class Solution {
public:
    vector<int> findErrorNums(vector<int>& nums) {
        vector<int> result;
        vector<int> freq(nums.size(), 0);
        for(auto e : nums)
        {
            if(freq[e-1] != 0)
            {
                result.push_back(e);
            }
            else
            {
                freq[e-1] = e;
            }
        }
        for(int i = 0; i < freq.size(); i++)
        {
            if(freq[i] == 0)
            {
                result.push_back(i+1);
                break;
            }
        }
        return result;
    }
};
```





#### [287. 寻找重复数](https://leetcode-cn.com/problems/find-the-duplicate-number/)

难度中等1322

给定一个包含 `n + 1` 个整数的数组 `nums` ，其数字都在 `1` 到 `n` 之间（包括 `1` 和 `n`），可知至少存在一个重复的整数。

假设 `nums` 只有 **一个重复的整数** ，找出 **这个重复的数** 。

你设计的解决方案必须不修改数组 `nums` 且只用常量级 `O(1)` 的额外空间。

 

**示例 1：**

```
输入：nums = [1,3,4,2,2]
输出：2
```

**示例 2：**

```
输入：nums = [3,1,3,4,2]
输出：3
```

**示例 3：**

```
输入：nums = [1,1]
输出：1
```

**示例 4：**

```
输入：nums = [1,1,2]
输出：1
```

 

**提示：**

- `1 <= n <= 105`
- `nums.length == n + 1`
- `1 <= nums[i] <= n`
- `nums` 中 **只有一个整数** 出现 **两次或多次** ，其余整数均只出现 **一次**

 

**进阶：**

- 如何证明 `nums` 中至少存在一个重复的数字?
- 你可以设计一个线性级时间复杂度 `O(n)` 的解决方案吗？

target : 返回重复的数字；

limitation : 只有一个数字重复, 说明其他数字都存在；NONONO,

​				    只能用O(1)的额外空间；时间复杂度至少为O(N);

​					数字都在1-n之间，数组大小为n+1;

​					不能修改数组nums;

思路： sum all element - sum index all; NONO, 题意不是这个，题意是说只有一个重复的整数，但是而已重复很多次；

思路2： 使用冒泡或者插入排序，排序后进行统计，但是时间复杂度比较高；首先不能修改数组元素，就把这一条给排除了；

思路3：既然是找重复，那么对于每个拿过来的数字，都要知道他过去出现的频次，这样，空间复杂度如何才能等于1呢？不可能，所以是需要基于二分分配查找的方法；

还有一种双指针解法，待学习：

```cpp
public int findDuplicate(int[] nums) {
    int slow = nums[0], fast = nums[nums[0]];
    while (slow != fast) {
        slow = nums[slow];
        fast = nums[nums[fast]];
    }
    fast = 0;
    while (slow != fast) {
        slow = nums[slow];
        fast = nums[fast];
    }
    return slow;
}
//参考： https://leetcode-cn.com/problems/find-the-duplicate-number/solution/qian-duan-ling-hun-hua-shi-tu-jie-kuai-man-zhi-z-3/
```



官方思路：

已经知道最大和最小范围，那么可以寻找一个中间值，如果没有重复值的话，那么小于等于i的元素个数应该为i个，但是如果有重复值的话，那么小于等于i的个数就大于i个；然后缩小范围；直到左边指针的值等于右边指针的值；

伪代码：

left = 1; 

right = n;

if(left  not eq right )

​	calculate mid;

​	counter if element less than or eq mid;

​    if counter > mid;

​		right = mid;

​    else 

​	    left = mid + 1;

return left value;

难点在于区分left 和 right 到底是值还是索引；二分查找不仅可以通过索引来分排好序的数组，而且可以通过值把元素分成两个集合，这里就是通过值把元素分成两个集合，一个集合包含重复值，另一个集合不包含重复值，到最后，有一个集合里面只包含重复值，而这时，左右都包含在里面；

```cpp
//个人依据官方思路的解法；
//bugfree; quick;
class Solution {
public:
    int findDuplicate(vector<int>& nums) {
        int left { 1 };
        int right = nums.size();
        int mid { (left + right) / 2};
        while((left != right))
        {
            mid = (left + right) / 2;
            if(counter_less_mid(nums,mid) > mid)
            {
                right = mid;
            }
            else
            {
                left = mid + 1;
            }
        }
        return left;

    }
    int counter_less_mid(vector<int>& nums, int mid)
    {
        int result { 0 };
        for(auto c : nums)
        {
            if ( c <= mid)
            {
                result++;
            }
        }
        return result;
    }
};
```



#### [667. 优美的排列 II](https://leetcode-cn.com/problems/beautiful-arrangement-ii/)

难度中等91

给你两个整数 `n` 和 `k` ，请你构造一个答案列表 `answer` ，该列表应当包含从 `1` 到 `n` 的 `n` 个不同正整数，并同时满足下述条件：

- 假设该列表是 `answer = [a1, a2, a3, ... , an]` ，那么列表 `[|a1 - a2|, |a2 - a3|, |a3 - a4|, ... , |an-1 - an|]` 中应该有且仅有 `k` 个不同整数。

返回列表 `answer` 。如果存在多种答案，只需返回其中 **任意一种** 。

 

**示例 1：**

```
输入：n = 3, k = 1
输出：[1, 2, 3]
解释：[1, 2, 3] 包含 3 个范围在 1-3 的不同整数，并且 [1, 1] 中有且仅有 1 个不同整数：1
```

**示例 2：**

```
输入：n = 3, k = 2
输出：[1, 3, 2]
解释：[1, 3, 2] 包含 3 个范围在 1-3 的不同整数，并且 [2, 1] 中有且仅有 2 个不同整数：1 和 2
```

 

**提示：**

- `1 <= k < n <= 104`

 

目标： 返回一个列表

条件： 相邻两个数差值的绝对值 的种类应该有K个；

​			包含了1 - n所有的整数，且不重复；



```cpp
//评论区一个新奇的想法：
若n=8初始状态
1 2 3 4 5 6 7 8
k=1~~~~~~~~         | 1 2 3 4 5 6 7 8 (不翻转，直接返回)
k=2~~~~~~~~         1 | 8 7 6 5 4 3 2
k=3~~~~~~~~         1 8 | 2 3 4 5 6 7
k=4~~~~~~~~         1 8 2 | 7 6 5 4 3
总结规律：当k>1时,需要翻转的次数为k-1次，每次翻转保留前m(m = 1,2,3...k-1)个数，每次翻转都在原数组进行。
思路是： 第一个和数和最后一个树之间的差值即为二者之间数据的个数，如果需要多一种，就把这两个放到前面；
class Solution {
    public int[] constructArray(int n, int k) {
        int[] res = new int[n];
        for(int i = 0; i < n; i++) res[i] = i + 1;  //产生1~n个数
        if(k == 1) return res;  //k==1直接返回
        //k != 1就要翻转k - 1次，每次翻转保留前m个数
        for(int m = 1; m < k; m++)
            reverse(res, m, n - 1); 
        return res;
    }
    //翻转数组[i,j]之间的数
    void reverse(int[] res, int i, int j){
        while(i < j){
            int t = res[i];
            res[i] = res[j];
            res[j] = t;
            i++;
            j--;
        }
    }

```

自己写一下伪代码：

init list;

while(i < k)

 swap i+1 and last;

 i++;



另一种官方解答：

思想： 首先按照从小到大的顺序排列，n - k 个数，剩下的k 个数，交叉放入最小值，最大值，最小值，最大值，每次缩小集合的范围即可；

伪代码：

loop:

​     1 -> n-k  : 1,2,3,4,,,, n-k;

n-k + 1 -> n:

left = n-k + 1;

right = n;

while(right > left)

right,left -> result;  //这里需要是先大后小，这样前一项和right的差值就会比较大，保证前面有一个有效的差值，因为后面总是包含一个差值为1的序列；

left ++; 

right--;

```cpp
class Solution {
public:
    vector<int> constructArray(int n, int k) {
        vector<int> result;
        for(int i = 1; i <= n -k; i++)
        {
            result.push_back(i);
        }
        int left {n-k + 1};
        int right { n };
        while(right >= left)
        {
            result.push_back(right);
            result.push_back(left);
            left++;
            right--;
        }
        left--;
        right++;
        if(right == left)
        {
            result.pop_back();
        }
        return result;
    }
};
```

​    



















