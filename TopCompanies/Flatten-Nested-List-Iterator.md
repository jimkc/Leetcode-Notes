## Question
Given a nested list of integers, implement an iterator to flatten it.<br>

Each element is either an integer, or a list -- whose elements may also be integers or other lists.

**Example :**   
<pre>
Given the list [[1,1],2,[1,1]],

By calling next repeatedly until hasNext returns false, the order of elements returned by next should be: [1,1,2,1,1].


Given the list [1,[4,[6]]],

By calling next repeatedly until hasNext returns false, the order of elements returned by next should be: [1,4,6].
</pre>

## Thinking

## Coding
Time: O(n); <br>
Space: O(n)
```python3
# """
# This is the interface that allows for creating nested lists.
# You should not implement it, or speculate about its implementation
# """
#class NestedInteger(object):
#    def isInteger(self):
#        """
#        @return True if this NestedInteger holds a single integer, rather than a nested list.
#        :rtype bool
#        """
#
#    def getInteger(self):
#        """
#        @return the single integer that this NestedInteger holds, if it holds a single integer
#        Return None if this NestedInteger holds a nested list
#        :rtype int
#        """
#
#    def getList(self):
#        """
#        @return the nested list that this NestedInteger holds, if it holds a nested list
#        Return None if this NestedInteger holds a single integer
#        :rtype List[NestedInteger]
#        """

class NestedIterator(object):

    def __init__(self, nestedList):
        """
        Initialize your data structure here.
        :type nestedList: List[NestedInteger]
        """
        # Reversed it so that we can use pop 
        self.stack = nestedList[::-1]
        
    def next(self):
        """
        :rtype: int
        """
        return self.stack.pop().getInteger()
        

    def hasNext(self):
        """
        :rtype: bool
        """
        
        # While our last element is a list of NestedInteger we dont break down all elements to be Integer, we break down until the last element is an Integer
        while self.stack and self.stack[-1].getInteger() == None:
            last_element = self.stack.pop()
            for el in last_element.getList()[::-1]:
                self.stack.append(el)
        return bool(self.stack)
                
                    
                
                
                
               
        
    

# Your NestedIterator object will be instantiated and called as such:
# i, v = NestedIterator(nestedList), []
# while i.hasNext(): v.append(i.next())
```

