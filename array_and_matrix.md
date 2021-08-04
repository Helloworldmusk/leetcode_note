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

//官方题解：
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

