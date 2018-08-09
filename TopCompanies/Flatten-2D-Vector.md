## Question
Implement an iterator to flatten a 2d vector.

**Example :**   
<pre>
Input: 2d vector =
[
  [1,2],
  [3],
  [4,5,6]
]
Output: [1,2,3,4,5,6]
Explanation: By calling next repeatedly until hasNext returns false, 
             the order of elements returned by next should be: [1,2,3,4,5,6].
</pre>

Follow up:<br>
As an added challenge, try to code it using only iterators in C++ or iterators in Java.

## Thinking


## Coding
Time: O(); <br>
Space: O()
```python3
class Vector2D(object):

    def __init__(self, vec2d):
        """
        Initialize your data structure here.
        :type vec2d: List[List[int]]
        """
        self.vecs = vec2d
        self.idx1 = 0
        self.idx2 = -1
    def next(self):
        """
        :rtype: int
        """
        
        return self.vecs[self.idx1][self.idx2]
        

    def hasNext(self):
        """
        :rtype: bool
        """
        if self.idx1 > len(self.vecs)-1:
            return False
        elif self.idx1 == len(self.vecs)-1 and self.idx2 == len(self.vecs[-1])-1:
            return False
        else:
            if self.idx2 == len(self.vecs[self.idx1])-1:
                self.idx1 += 1
                self.idx2 = 0

                while not self.vecs[self.idx1]:
                    self.idx1 += 1
                    
                    if self.idx1 > len(self.vecs)-1:
                        return False
                
            else:
                self.idx2 += 1

        return True
        
        
# Your Vector2D object will be instantiated and called as such:
# i, v = Vector2D(vec2d), []
# while i.hasNext(): v.append(i.next())
```

