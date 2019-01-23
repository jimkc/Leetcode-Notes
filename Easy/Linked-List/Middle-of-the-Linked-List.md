## Question
Given a non-empty, singly linked list with head node head, return a middle node of linked list.<br>

If there are two middle nodes, return the second middle node.

**Example :**   
<pre>
Input: [1,2,3,4,5]
Output: Node 3 from this list (Serialization: [3,4,5])
The returned node has value 3.  (The judge's serialization of this node is [3,4,5]).
Note that we returned a ListNode object ans, such that:
ans.val = 3, ans.next.val = 4, ans.next.next.val = 5, and ans.next.next.next = NULL.

Input: [1,2,3,4,5,6]
Output: Node 4 from this list (Serialization: [4,5,6])
Since the list has two middle nodes with values 3 and 4, we return the second one.
</pre>

## Thinking
用兩個指標，fast和slow，這樣只要one pass

## Coding
Time: O(n); one pass </br>
Space: O(1)
```python3
# Definition for singly-linked list.
# class ListNode(object):
#     def __init__(self, x):
#         self.val = x
#         self.next = None

class Solution(object):
    def middleNode(self, head):
        """
        :type head: ListNode
        :rtype: ListNode
        """
        if not head:
            return head
        dummy = ListNode(0)
        dummy.next = head
        slow = dummy
        fast = dummy

        
        while fast and fast.next:
            slow = slow.next
            fast = fast.next.next
            
        if fast == None:
            return slow
        else:
            return slow.next
```

