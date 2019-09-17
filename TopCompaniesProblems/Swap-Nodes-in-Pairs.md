## Question
Given a linked list, swap every two adjacent nodes and return its head.

**Example :**   
<pre>
Given 1->2->3->4, you should return the list as 2->1->4->3.
</pre>

Note:<br>

Your algorithm should use only constant extra space.<br>
You may not modify the values in the list's nodes, only nodes itself may be changed.

## Thinking

## Coding
Time: O(n); <br>
Space: O(1)
```python3
# Definition for singly-linked list.
# class ListNode:
#     def __init__(self, x):
#         self.val = x
#         self.next = None

class Solution:
    def swapPairs(self, head):        
        """
        :type head: ListNode
        :rtype: ListNode
        """
        dummy = p = ListNode(0) # Dummy is the node before the head of every pair
        dummy.next = head
        while head and head.next:
            tmp = head.next # tmp is the second of each pair
            head.next = tmp.next
            tmp.next = head
            p.next = tmp
            head = head.next
            p = tmp.next
        return dummy.next
```

