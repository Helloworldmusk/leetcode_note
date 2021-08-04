#### [232. 用栈实现队列](https://leetcode-cn.com/problems/implement-queue-using-stacks/)

难度简单442收藏分享切换为英文接收动态反馈

请你仅使用两个栈实现先入先出队列。队列应当支持一般队列支持的所有操作（`push`、`pop`、`peek`、`empty`）：

实现 `MyQueue` 类：

- `void push(int x)` 将元素 x 推到队列的末尾
- `int pop()` 从队列的开头移除并返回元素
- `int peek()` 返回队列开头的元素
- `boolean empty()` 如果队列为空，返回 `true` ；否则，返回 `false`

 

**说明：**

- 你只能使用标准的栈操作 —— 也就是只有 `push to top`, `peek/pop from top`, `size`, 和 `is empty` 操作是合法的。
- 你所使用的语言也许不支持栈。你可以使用 list 或者 deque（双端队列）来模拟一个栈，只要是标准的栈操作即可。

 

**进阶：**

- 你能否实现每个操作均摊时间复杂度为 `O(1)` 的队列？换句话说，执行 `n` 个操作的总时间复杂度为 `O(n)` ，即使其中一个操作可能花费较长时间。

 

**示例：**

```
输入：
["MyQueue", "push", "push", "peek", "pop", "empty"]
[[], [1], [2], [], [], []]
输出：
[null, null, null, 1, 1, false]

解释：
MyQueue myQueue = new MyQueue();
myQueue.push(1); // queue is: [1]
myQueue.push(2); // queue is: [1, 2] (leftmost is front of the queue)
myQueue.peek(); // return 1
myQueue.pop(); // return 1, queue is [2]
myQueue.empty(); // return false
```



 

**提示：**

- `1 <= x <= 9`
- 最多调用 `100` 次 `push`、`pop`、`peek` 和 `empty`
- 假设所有操作都是有效的 （例如，一个空的队列不会调用 `pop` 或者 `peek` 操作）

target 使用栈来实现队列，即先进先出；要求操作总时间复杂度为O(N)

条件： 两个栈；

个人思路：入栈一定都是在 左栈，出栈都是在右栈，入栈前，要把右栈的所有元素都放到左栈，出栈是，要把左栈的所有元素放到右栈； (这样复杂度比较高，正确的应该是分段处理)

正确思路： 只有当右栈为空的时候，才把左栈的按照倒序放到右栈，这算是分段进行前后调换，但是总体上也是进行了前后调换；

```cpp
#include <stack>
class MyQueue {
public:
    /** Initialize your data structure here. */
    MyQueue() {

    }
    
    /** Push element x to the back of queue. */
    void push(int x) {
        left_stack.push(x);
    }
    
    /** Removes the element from in front of queue and returns that element. */
    int pop() {
        if(right_stack.empty())
        {
            while(!left_stack.empty())
            {
                right_stack.push(left_stack.top());
                left_stack.pop();
            }
        }
        int result = right_stack.top();
        right_stack.pop();
        return result;
    }

    
    /** Get the front element. */
    int peek() {
        if(right_stack.empty())
        {
            while(!left_stack.empty())
            {
                right_stack.push(left_stack.top());
                left_stack.pop();
            }
        }
        int result = right_stack.top();
        return result;
    }
    
    /** Returns whether the queue is empty. */
    bool empty() {
        if(left_stack.empty() && right_stack.empty())
        {
            return true;
        }
        return false;
    }

private:
    std::stack<int> left_stack;
    std::stack<int> right_stack;
};

/**
 * Your MyQueue object will be instantiated and called as such:
 * MyQueue* obj = new MyQueue();
 * obj->push(x);
 * int param_2 = obj->pop();
 * int param_3 = obj->peek();
 * bool param_4 = obj->empty();
 */
```



#### [225. 用队列实现栈](https://leetcode-cn.com/problems/implement-stack-using-queues/)

难度简单350收藏分享切换为英文接收动态反馈

请你仅使用两个队列实现一个后入先出（LIFO）的栈，并支持普通栈的全部四种操作（`push`、`top`、`pop` 和 `empty`）。

实现 `MyStack` 类：

- `void push(int x)` 将元素 x 压入栈顶。
- `int pop()` 移除并返回栈顶元素。
- `int top()` 返回栈顶元素。
- `boolean empty()` 如果栈是空的，返回 `true` ；否则，返回 `false` 。

 

**注意：**

- 你只能使用队列的基本操作 —— 也就是 `push to back`、`peek/pop from front`、`size` 和 `is empty` 这些操作。
- 你所使用的语言也许不支持队列。 你可以使用 list （列表）或者 deque（双端队列）来模拟一个队列 , 只要是标准的队列操作即可。

 

**示例：**

```
输入：
["MyStack", "push", "push", "top", "pop", "empty"]
[[], [1], [2], [], [], []]
输出：
[null, null, null, 2, 2, false]

解释：
MyStack myStack = new MyStack();
myStack.push(1);
myStack.push(2);
myStack.top(); // 返回 2
myStack.pop(); // 返回 2
myStack.empty(); // 返回 False
```

 

**提示：**

- `1 <= x <= 9`
- 最多调用`100` 次 `push`、`pop`、`top` 和 `empty`
- 每次调用 `pop` 和 `top` 都保证栈不为空

**思路：分成主队列和辅助队列，主队列插入一个值之前，先把主队列的内容转移到 辅助队列，这时主队列为空，然后把辅助队列内容在转移到主队列，即实现了新插入的在出口，早插入的在最后;**

 : 

A : 

 : A

B : A

AB:

​	: AB

C : A B

ABC :  

关键点是，插入后，要把右栈的转移到左栈；

```
//个人+参考解法
#include <queue>
class MyStack {
public:
    /** Initialize your data structure here. */
    MyStack() {

    }
    
    /** Push element x onto stack. */
    void push(int x) {
        while(!left_queue.empty())
        {
            right_queue.push(left_queue.front());
            left_queue.pop();
        }
        left_queue.push(x);
        while(!right_queue.empty())
        {
            left_queue.push(right_queue.front());
            right_queue.pop();
        }
    }
    
    /** Removes the element on top of the stack and returns that element. */
    int pop() {
        int result = left_queue.front();
        left_queue.pop();
        return result;
    }
    
    /** Get the top element. */
    int top() {
        int result = left_queue.front();
        return result;
    }
    

    /** Returns whether the stack is empty. */
    bool empty() {
        if(left_queue.empty() && right_queue.empty() )
        {
            return true;
        }
        return false;
    }
private:
    std::queue<int> left_queue;
    std::queue<int> right_queue;
};

```



设计一个支持 push ，pop ，top 操作，并能在常数时间内检索到最小元素的栈。

push(x) —— 将元素 x 推入栈中。
pop() —— 删除栈顶的元素。
top() —— 获取栈顶元素。
getMin() —— 检索栈中的最小元素。


示例:

输入：
["MinStack","push","push","push","getMin","pop","top","getMin"]
[[],[-2],[0],[-3],[],[],[],[]]

输出：
[null,null,null,null,-3,null,0,-2]

解释：
MinStack minStack = new MinStack();
minStack.push(-2);
minStack.push(0);
minStack.push(-3);
minStack.getMin();   --> 返回 -3.
minStack.pop();
minStack.top();      --> 返回 0.
minStack.getMin();   --> 返回 -2.


提示：

pop、top 和 getMin 操作总是在 非空栈 上调用。

设计一个栈： 常数时间内可以获取到最小值；

```cpp
//自己答案 + 参考：
//    使用双栈实现；


class MinStack {
public:
    /** initialize your data structure here. */
    MinStack() {


    }
    
    void push(int val) {
        main_stack.push(val);
        if(min_stack.empty())
        {
            min_stack.push(val);
        }
        else if (val <= min_stack.top())
        {
            min_stack.push(val);
        }
    }
    
    void pop() {
        if(!min_stack.empty() && (min_stack.top() == main_stack.top() ) )
        {
            min_stack.pop();
        } 
        main_stack.pop();
    }
    
    int top() {
        return main_stack.top();
    }
    
    int getMin() {
        return min_stack.top();
    }
private:
    std::stack<int> main_stack;
    std::stack<int> min_stack;
};		

//解法2： 使用单栈实现；
//思路： 当有更小的值来的时候，我们只需要把之前的最小值入栈，当前更小的值再入栈即可。当这个最小值要出栈的时候，下一个值便是之前的最小值
#include <limits.h>
class MinStack {
public:
    /** initialize your data structure here. */
    MinStack() {

    }
    
    void push(int val) {
        if(val <= min) 
        { 
            stack.push(min);
            min = val; 
        }
        stack.push(val);
    }
    
    void pop() {
        if (!stack.empty())
        {
            if (min == stack.top())
            {
                stack.pop();
                min = stack.top();
                stack.pop();
            }
            else
            {
                stack.pop();
            }
        }
    }
    
    int top() {
        return stack.top();
    }
    
    int getMin() {
        return min;
    }
private:
    int min = INT_MAX ;
    std::stack<int> stack;
};

```

参考： https://leetcode-cn.com/problems/min-stack/solution/xiang-xi-tong-su-de-si-lu-fen-xi-duo-jie-fa-by-38/

#### [20. 有效的括号](https://leetcode-cn.com/problems/valid-parentheses/)

难度简单2521收藏分享切换为英文接收动态反馈

给定一个只包括 `'('`，`')'`，`'{'`，`'}'`，`'['`，`']'` 的字符串 `s` ，判断字符串是否有效。

有效字符串需满足：

1. 左括号必须用相同类型的右括号闭合。
2. 左括号必须以正确的顺序闭合。

 

**示例 1：**

```
输入：s = "()"
输出：true
```

**示例 2：**

```
输入：s = "()[]{}"
输出：true
```

**示例 3：**

```
输入：s = "(]"
输出：false
```

**示例 4：**

```
输入：s = "([)]"
输出：false
```

**示例 5：**

```
输入：s = "{[]}"
输出：true
```

 

**提示：**

- `1 <= s.length <= 104`
- `s` 仅由括号 `'()[]{}'` 组成



思路： 使用栈，只要是左括号，就入栈，如果是右括号，就出栈，如果出栈前 栈顶元素的括号和 当前括号不是一堆，就返回错误；

```cpp
class Solution {
public:
    bool isValid(string s) {
        for( auto c : s)
        {
            switch(c)
            {
                case '(': 
                {
                    stack.push(c);
                    break;
                }
                case '{':
                {
                    stack.push(c);
                    break;
                }
                case '[':
                {
                    stack.push(c);
                    break;
                }
                case ')':
                {
                    if(!stack.empty() && stack.top() == '(')
                    {
                        stack.pop();
                    }
                    else
                    {
                        return false;
                    }
                    break;
                }
                case '}':
                {
                    if(!stack.empty() && stack.top() == '{')
                    {
                        stack.pop();
                    }
                    else
                    {
                        return false;
                    }
                    break;
                }
                case ']':
                {
                    if(!stack.empty() && stack.top() == '[')
                    {
                        stack.pop();
                    }
                    else
                    {
                        return false;
                    }
                    break;
                }
                default: {}
            } //switch(c):
        } //for( auto c : s)
        if(stack.empty())
        {
            return true;
        }
        return false;
    } //bool isValid(string s)
private:
    std::stack<int> stack;
};
```





#### [739. 每日温度](https://leetcode-cn.com/problems/daily-temperatures/)

难度中等827收藏分享切换为英文接收动态反馈

请根据每日 `气温` 列表 `temperatures` ，请计算在每一天需要等几天才会有更高的温度。如果气温在这之后都不会升高，请在该位置用 `0` 来代替。

**示例 1:**

```
输入: temperatures = [73,74,75,71,69,72,76,73]
输出: [1,1,4,2,1,1,0,0]
```

**示例 2:**

```
输入: temperatures = [30,40,50,60]
输出: [1,1,1,0]
```

**示例 3:**

```
输入: temperatures = [30,60,90]
输出: [1,1,0]
```

 

**提示：**

- `1 <= temperatures.length <= 105`
- `30 <= temperatures[i] <= 100`

目的： 找到vector 中， 比当前值大的元素的索引和当前值所在的索引的差值；没有比当前元素大的，认为距离为0；

方案1： 

依次遍历每个元素，在后面找第一个比当前值大的元素，返回二者的差值，如果到达末尾，则返回0； 时间复杂度O(N2), 空间复杂度O(1);

方案2：

参考答案：

​	依次将访问的元素的下标入栈，如果一个元素入栈前的值大于栈顶元素，则将栈顶元素对应的结果位置设置为 查询元素的差值 ，迭代对比栈顶元素，并且将查询元素入栈；

```cpp
//个人参考思想自己写的解法；
#include <iostream>
class Solution {
public:
    vector<int> dailyTemperatures(vector<int>& temperatures) {
        vector<int> result;
        std::stack<int> stack;
        result.resize(temperatures.size(),0);
        for(int i = 0; i < temperatures.size(); i++)
        {
            while( !stack.empty() && temperatures[i] > temperatures[stack.top()] )
            {
                result[stack.top()] = i - stack.top();
                stack.pop();
            }
            stack.push(i);
        }
        return result;
    }
};
```



#### [503. 下一个更大元素 II](https://leetcode-cn.com/problems/next-greater-element-ii/)

难度中等458收藏分享切换为英文接收动态反馈

给定一个循环数组（最后一个元素的下一个元素是数组的第一个元素），输出每个元素的下一个更大元素。数字 x 的下一个更大的元素是按数组遍历顺序，这个数字之后的第一个比它更大的数，这意味着你应该循环地搜索它的下一个更大的数。如果不存在，则输出 -1。

**示例 1:**

```
输入: [1,2,1]
输出: [2,-1,2]
解释: 第一个 1 的下一个更大的数是 2；
数字 2 找不到下一个更大的数； 
第二个 1 的下一个最大的数需要循环搜索，结果也是 2。
```

**注意:** 输入数组的长度不会超过 10000。

思路：

​	依次遍历数组中的元素，如果当前元素的值大于 栈顶 id对应元素的值，则将栈顶元素对应的结果索引设置为当前遍历值，否则将元素入栈； 如果 查询id == 当前id的时候， 退出；设置最多遍历两边，防止单值的vector出现循环的现象；

```cpp
//个人写法；
#include <stack>
class Solution {
public:
    vector<int> nextGreaterElements(vector<int>& nums) {
        vector<int> result;
        result.resize(nums.size(), -1);
        std::stack<int> stack;
        int cnt = 0;

        for(int i =0; i < nums.size() && cnt < 2 * nums.size(); cnt++, i = (++i)%nums.size())
        {
            while(!stack.empty() && nums[i] > nums[stack.top()] )
            {
                result[stack.top()] = nums[i];
                stack.pop();
            }
            if(!stack.empty() && i == stack.top()) //need review; for( 3,3,3,3,3,)这种情况；
            {
                return result;
            }
            else 
            {
                stack.push(i);
            }
        }
        return result;
    }
};
```

​    **if(**!stack.empty() && i == stack.top()) //need review;**
​        **{**
​            **return result;**
​        }**









