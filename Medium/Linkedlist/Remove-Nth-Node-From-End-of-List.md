## Question
Given a linked list, remove the n-th node from the end of list and return its head. </br>

**Example :**   
<pre>
Given linked list: 1->2->3->4->5, and n = 2.

After removing the second node from the end, the linked list becomes 1->2->3->5.
</pre>

Note:<br>

Given n will always be valid.<br>

Follow up:<br>

Could you do this in one pass?

## Thinking
用兩個指標來製造gap，這樣就可以one pass做完

## Coding
Time: O(n);<br>
Space: O(1)
```python3
# Definition for singly-linked list.
# class ListNode:
#     def __init__(self, x):
#         self.val = x
#         self.next = None

class Solution:
    def removeNthFromEnd(self, head, n):
        """
        :type head: ListNode
        :type n: int
        :rtype: ListNode
        """    
        dummy = ListNode(0)
        dummy.next = head
        first = dummy # first pointer
        second = dummy # second pointer
        
        # first pointer go to the n+1 th node
        for i in range(n+1):
            first = first.next
        
        # second starts from the head, so when first reaches end, second will be the previous node of the delete node
        while first:
            first = first.next
            second = second.next
        second.next = second.next.next
        return dummy.next
        
```

