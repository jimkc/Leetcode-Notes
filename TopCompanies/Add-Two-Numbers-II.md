## Question
You are given two non-empty linked lists representing two non-negative integers. The most significant digit comes first and each of their nodes contain a single digit. Add the two numbers and return it as a linked list.<br>

You may assume the two numbers do not contain any leading zero, except the number 0 itself.<br>

Follow up:<br>
What if you cannot modify the input lists? In other words, reversing the lists is not allowed.

**Example :**   
<pre>
Input: (7 -> 2 -> 4 -> 3) + (5 -> 6 -> 4)
Output: 7 -> 8 -> 0 -> 7
</pre>

## Thinking
It seems to me that by pairing the neighbor numbers in the sorted list will result the maximum value.

## Coding
Time: O(n);<br>
Space: O(n)
```python3
# Definition for singly-linked list.
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
        n1 = []
        n2 = []
        
        while l1:
            n1.append(str(l1.val))
            l1 = l1.next
        while l2:
            n2.append(str(l2.val))
            l2 = l2.next 
        
        s = list(  str( int(''.join(n1)) + int(''.join(n2)) ) ) # Sum of two nums
        
        head = ListNode(int(s[0]))
        node = head
        for n in s[1:]:
            node.next = ListNode(int(n))
            node = node.next
        return head
```

Method without type transformation:
```python3
class Solution:
    def addTwoNumbers(self, l1, l2):
        """
        :type l1: ListNode
        :type l2: ListNode
        :rtype: ListNode
        """
        p1 = l1
        x = 0
        while p1:
            x = x * 10 + p1.val
            p1 = p1.next
        p2 = l2
        y = 0
        while p2:
            y = y * 10 + p2.val
            p2 = p2.next
        r = x+y
        res = None
        if r == 0 and l1 and l2:
            return ListNode(0)

        while r:
            reminder = r % 10
            r = r//10
            if res:
                p = ListNode(reminder)
                p.next = res
                res = p
            else:
                res = ListNode(reminder)
        return res
```

