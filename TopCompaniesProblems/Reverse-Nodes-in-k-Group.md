## Question
Given a linked list, reverse the nodes of a linked list k at a time and return its modified list.<br>

k is a positive integer and is less than or equal to the length of the linked list. If the number of nodes is not a multiple of k then left-out nodes in the end should remain as it is.

**Example :**   
<pre>
Given this linked list: 1->2->3->4->5

For k = 2, you should return: 2->1->4->3->5

For k = 3, you should return: 3->2->1->4->5
</pre>

Note:<br>

Only constant extra memory is allowed.<br>
You may not alter the values in the list's nodes, only nodes itself may be changed.

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
    def reverseKGroup(self, head, k):
        dummy = jump = ListNode(0) # jump is the temp head for every single range that we want to reverse
        dummy.next = l = r = head

        while True:
            count = 0
            while r and count < k:   # use r to locate the range (r is not the last ele in range and add 1)
                r = r.next
                count += 1
            if count == k:  # if size k satisfied, reverse the inner linked list
                pre, cur = r, l
                for _ in range(k):
                    cur.next, cur, pre = pre, cur.next, cur  # standard reversing
                jump.next, jump, l = pre, l, r  # connect two k-groups
            else:
                return dummy.next
```

