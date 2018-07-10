## Question
Remove all elements from a linked list of integers that have value val.

**Example :**
<pre>
Input:  1->2->6->3->4->5->6, val = 6
Output: 1->2->3->4->5
</pre>
## Thinking
Use three pointers prev,prehead and curr.
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
    def removeElements(self, head, val):
        """
        :type head: ListNode
        :type val: int
        :rtype: ListNode
        """
        prehead = ListNode(0)
        prehead.next = head
        prev = prehead 
        curr = head
        
        
        while curr != None:
            nxt = curr.next
            if curr.val == val:
                prev.next = nxt
                curr = nxt
            else:
                prev = curr
                curr = nxt
        return prehead.next
                
```

