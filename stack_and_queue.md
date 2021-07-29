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

思路：分成主队列和辅助队列，主队列插入一个值之前，先把主队列的内容转移到 辅助队列，这时主队列为空，然后把辅助队列内容在转移到主队列，即实现了新插入的在出口，早插入的在最后;

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

