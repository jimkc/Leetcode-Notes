## Question
Given a singly linked list, group all odd nodes together followed by the even nodes. Please note here we are talking about the node number and not the value in the nodes.<br>
<br>
You should try to do it in place. The program should run in O(1) space complexity and O(nodes) time complexity.</br>

**Example :**   
<pre>
Input: 1->2->3->4->5->NULL
Output: 1->3->5->2->4->NULL

Input: 2->1->3->5->6->4->7->NULL
Output: 2->3->6->7->1->5->4->NULL
</pre>

Note:<br>
<br>
The relative order inside both the even and odd groups should remain as it was in the input.<br>
The first node is considered odd, the second node even and so on ...

## Thinking


## Coding
Time: O(n); n node </br>
Space: O(1) only 2 dummy node
```python
# Definition for singly-linked list.
# class ListNode:
#     def __init__(self, x):
#         self.val = x
#         self.next = None

class Solution:
    def oddEvenList(self, head: ListNode) -> ListNode:
        oddDummy = ListNode(None)
        evenDummy = ListNode(None)
        odd, even = oddDummy, evenDummy
        
        isOdd = True
        
        while head:
            if isOdd:
                odd.next = head
                odd = odd.next
                isOdd = False
            else:
                even.next = head
                even = even.next
                isOdd = True
            head = head.next
        
        even.next = None
        odd.next = evenDummy.next
        
        return oddDummy.next
                
            
```

