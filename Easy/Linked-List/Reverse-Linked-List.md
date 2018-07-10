## Question
Reverse a singly linked list.</br>

**Example :**
<pre>
Input: 1->2->3->4->5->NULL
Output: 5->4->3->2->1->NULL
</pre>

## Thinking
Use three variables to store the status of current, previous and next.</br>

## Coding
Time: O(n); Go through the list once. </br>
Space: O(1) 
```python3
# Definition for singly-linked list.
# class ListNode:
#     def __init__(self, x):
#         self.val = x
#         self.next = None

class Solution:
    def reverseList(self, head):
        """
        :type head: ListNode
        :rtype: ListNode
        """
        
        curr = head
        prev = None
        
        while curr != None:
            nxt = curr.next
            curr.next = prev
            prev = curr
            curr = nxt
        return prev
```

