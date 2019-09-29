## Question
Given a non-negative integer represented as non-empty a singly linked list of digits, plus one to the integer.<br>

You may assume the integer do not contain any leading zero, except the number 0 itself.<br>

The digits are stored such that the most significant digit is at the head of the list.</br>

**Example :**   
<pre>
Input: [1,2,3]
Output: [1,2,4]
</pre>

## Thinking
Try to explain with example 1->8->9->7->9->9.
Pointer i points to the first node before the tailing 9s, which is 7 in this case and pointer j points to the last node of the whole list, which is 9.

If the value of i exists, i is in the middle of the int with value < 9, if it doesn't exist, i is the dummy node, and the rest int is 9, so change dummy node to 1, and rest to 0.
The use of dummy node:

In the cases like 9->9->9, the answer should return dummy. (1->0->0->0)
In other cases like 8->9->9, the answer should return dummy.next. (None->9->0->0, dummy is the first 0).

## Coding
Time: O(n); </br>
Space: O(1)
```python
# Definition for singly-linked list.
# class ListNode:
#     def __init__(self, x):
#         self.val = x
#         self.next = None

class Solution:
    def plusOne(self, head: ListNode) -> ListNode:
        
        dummy = ListNode(None)
        dummy.next = head
        # i is first none 9 node
        i, j = dummy, dummy
        
        while j.next:
            j = j.next
            if j.val != 9:
                i = j
        
        if i.val:
            i.val += 1
        else:
            i.val = 1
            
        i = i.next
        while i:
            i.val = 0
            i = i.next
        
        if not dummy.val:
            return dummy.next
        else:
            return dummy
```

