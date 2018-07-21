## Question
You are given two non-empty linked lists representing two non-negative integers. The digits are stored in reverse order and each of their nodes contain a single digit. Add the two numbers and return it as a linked list.<br>

You may assume the two numbers do not contain any leading zero, except the number 0 itself.

**Example :**   
<pre>
Input: (2 -> 4 -> 3) + (5 -> 6 -> 4)
Output: 7 -> 0 -> 8
Explanation: 342 + 465 = 807.
</pre>

## Thinking

## Coding
Time: O(n); <br>
Space: O(1)
```python3
# class ListNode:
#     def __init__(self, x):
#         self.val = x
#         self.next = None

class Solution:
    def addTwoNumbers(self, l1, l2):
        """
        :type l1: ListNode
        :type l2: ListNode
        :rtype: ListNode
        """
        head = l1
        carry = 0
        while l1 and l2:
            n = l1.val + l2.val + carry
            val, carry = n % 10, n // 10
            l1.val = val
            prev, l1, l2 =l1, l1.next, l2.next # Need prev after we finish the loop
            
        l = l1 or l2 
        prev.next = l # to link the previous added list to the rest of the uncounted list (l1 or l2)
        
        while l:
            n = l.val + carry
            val, carry = n % 10, n // 10
            l.val = val
            prev, l = l, l.next
            
        # In case there is carry after last digit
        if carry: 
            prev.next = ListNode(carry)
        
        return head
```

