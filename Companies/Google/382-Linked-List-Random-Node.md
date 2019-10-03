## Question
Given a singly linked list, return a random node's value from the linked list. Each node must have the same probability of being chosen.<br>

Follow up:<br>
What if the linked list is extremely large and its length is unknown to you? Could you solve this efficiently without using extra space?</br>

**Example :**   
<pre>
// Init a singly linked list [1,2,3].
ListNode head = new ListNode(1);
head.next = new ListNode(2);
head.next.next = new ListNode(3);
Solution solution = new Solution(head);

// getRandom() should return either 1, 2, or 3 randomly. Each element should have equal probability of returning.
solution.getRandom();
</pre>

## Thinking
The probability of index i is 1/i x i/i+1 x i+1/i+2 ....... n-2/n-1 x n-1/n = 1/n, only the first 1/i is the prob to change result value to i, and the rest prob is the prob to not change the result value.

## Coding
Time: O(n) n nodes;</br>
Space: O(1)
```python
# Definition for singly-linked list.
# class ListNode:
#     def __init__(self, x):
#         self.val = x
#         self.next = None

class Solution:

    def __init__(self, head: ListNode):
        """
        @param head The linked list's head.
        Note that the head is guaranteed to be not null, so it contains at least one node.
        """
        self.node = head
        

    def getRandom(self) -> int:
        """
        Returns a random node's value.
        """
        n = self.node
        result = n.val
        
        index = 0
        while n:
                if random.randint(0, index) == index: # 1/index
                    result = n.val
                n = n.next
                index += 1
        return result
        


# Your Solution object will be instantiated and called as such:
# obj = Solution(head)
# param_1 = obj.getRandom()
```

