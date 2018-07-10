## Question
Given a singly linked list, determine if it is a palindrome.</br>

**Example 1:**
<pre>
Input: 1->2
Output: false
</pre>

**Example 2:**
<pre>
Input: 1->2->2->1
Output: true
</pre>

Follow up:
Could you do it in O(n) time and O(1) space?

## Thinking
Find the middle point of the linked list, then reverse the second half and compare if the first half is 
equal to the first half.

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
    def isPalindrome(self, head):
        """
        :type head: ListNode
        :rtype: bool
        """
        if head == None or head.next == None:
            return True
        
        
        fast = head
        slow = head
        # Find the middle point of the list (slow is in the middle)
        while fast.next and fast.next.next:
            slow = slow.next
            fast = fast.next.next
            
        # Reverse the second half of the list
        prev = None
        slow = slow.next
        while slow:
            nxt = slow.next
            slow.next = prev
            prev = slow
            slow = nxt
        
        # Compare head and the reversed second half  
        while prev:
            if head.val == prev.val:
                head = head.next
                prev = prev.next
            else:
                return False
        return True
```

