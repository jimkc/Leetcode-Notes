## Question
Design a data structure that supports all following operations in average O(1) time.<br>

insert(val): Inserts an item val to the set if not already present.<br>
remove(val): Removes an item val from the set if present.<br>
getRandom: Returns a random element from current set of elements. Each element must have the same probability of being returned.

**Example :**   
<pre>
// Init an empty set.
RandomizedSet randomSet = new RandomizedSet();

// Inserts 1 to the set. Returns true as 1 was inserted successfully.
randomSet.insert(1);

// Returns false as 2 does not exist in the set.
randomSet.remove(2);

// Inserts 2 to the set, returns true. Set now contains [1,2].
randomSet.insert(2);

// getRandom should return either 1 or 2 randomly.
randomSet.getRandom();

// Removes 1 from the set, returns true. Set now contains [2].
randomSet.remove(1);

// 2 was already in the set, so return false.
randomSet.insert(2);

// Since 2 is the only number in the set, getRandom always return 2.
randomSet.getRandom();
</pre>

## Thinking


## Coding
Time: O(1);<br>
Space: O(n)
```python3
import random
class RandomizedSet:

    def __init__(self):
        """
        Initialize your data structure here.
        """
        self.s = {}
        self.l = []

    def insert(self, val):
        """
        Inserts a value to the set. Returns true if the set did not already contain the specified element.
        :type val: int
        :rtype: bool
        """
        if val not in self.s:
            self.l.append(val)
            self.s[val] = len(self.l)-1
            return True
        else:
            return False
        

    def remove(self, val):
        """
        Removes a value from the set. Returns true if the set contained the specified element.
        :type val: int
        :rtype: bool
        """
        
        if val in self.s:
            idx = self.s[val]
            self.l[idx], self.l[-1] = self.l[-1], self.l[idx] # Swap to do pop in O(1)
            self.s[self.l[idx]] = idx
            self.l.pop() # Do pop after all actions are done
            self.s.pop(val)
            return True
        else:
            return False

    def getRandom(self):
        """
        Get a random element from the set.
        :rtype: int
        """
        
        ran_idx = random.randint(0, len(self.l)-1)
        return self.l[ran_idx]

# Your RandomizedSet object will be instantiated and called as such:
# obj = RandomizedSet()
# param_1 = obj.insert(val)
# param_2 = obj.remove(val)
# param_3 = obj.getRandom()
```
