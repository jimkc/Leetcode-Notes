## Question
Implement the following operations of a stack using queues..<br>

push(x) -- Push element x onto stack.<br>
pop() -- Removes the element on top of the stack..<br>
top() -- Get the top element..<br>
empty() -- Return whether the stack is empty..<br>

**Example :**
<pre>
MyStack stack = new MyStack();

stack.push(1);
stack.push(2);  
stack.top();   // returns 2
stack.pop();   // returns 2
stack.empty(); // returns false
</pre>

Notes:<br>

You must use only standard operations of a queue -- which means only push to back, peek/pop from front, size, and is empty operations are valid.<br>
Depending on your language, queue may not be supported natively. You may simulate a queue by using a list or deque (double-ended queue), as long as you use only standard operations of a queue.<br>
You may assume that all operations are valid (for example, no pop or top operations will be called on an empty stack).<br>

## Thinking
Implement with queue means we should only use pop(0).

## Coding
Time: O(1) for push O(n) for pop;  </br>
Space: O().
```python3
class MyStack:

    def __init__(self):
        """
        Initialize your data structure here.
        """
        self.q1 = [] # Can only do pop(0)
        self.q2 = []
        self.size = 0

    def push(self, x):
        """
        Push element x onto stack.
        :type x: int
        :rtype: void
        """
        
        if not self.q1:
            self.q2.append(x)
        else:
            self.q1.append(x)
        self.size += 1
        

    def pop(self):
        """
        Removes the element on top of the stack and returns that element.
        :rtype: int
        """
        
        if len(self.q1) == 0:
            for i in range(self.size-1):
                self.q1.append(self.q2.pop(0))
            self.size -= 1
            return self.q2.pop(0)
        else:
            for i in range(self.size-1):
                self.q2.append(self.q1.pop(0))
            self.size -= 1
            return self.q1.pop(0)
        
        
    def top(self):
        """
        Get the top element.
        :rtype: int
        """
        pop_item = None
        if len(self.q1) == 0:
            for i in range(self.size-1):
                self.q1.append(self.q2.pop(0))
            pop_item = self.q2.pop(0)
            self.q1.append(pop_item)
        else:
            for i in range(self.size-1):
                self.q2.append(self.q1.pop(0))
                
            pop_item = self.q1.pop(0)
            self.q2.append(pop_item)
        return pop_item
        
        

    def empty(self):
        """
        Returns whether the stack is empty.
        :rtype: bool
        """
        return self.size == 0
        


# Your MyStack object will be instantiated and called as such:
# obj = MyStack()
# obj.push(x)
# param_2 = obj.pop()
# param_3 = obj.top()
# param_4 = obj.empty()
```

