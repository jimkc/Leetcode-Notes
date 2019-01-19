## Question
Sort a linked list in O(n log n) time using constant space complexity. </br>

**Example :**   
<pre>
Input: 4->2->1->3
Output: 1->2->3->4

Input: -1->5->3->4->0
Output: -1->0->3->4->5
</pre>

## Thinking
 原理跟quicksosrt一樣，但要注意的是我們需要用三個linked list做partition，一個是跟pivot一樣大，一個比它大，一個比它小，然後再把比它大跟小的丟到quicksort recursive。

## Coding
Time: O(nlogn); <br>
array once. </br>
Space: O(1)
```python3
# Definition for singly-linked list.
# class ListNode(object):
#     def __init__(self, x):
#         self.val = x
#         self.next = None

class Solution(object):
    def sortList(self, head):
        """
        :type head: ListNode
        :rtype: ListNode
        """
        
        
        def quicksort(head):
            if not head:
                return head
            
            # left right head is used as pointers moving alone the three lists
            dummyhead = head
            dummyright = right = ListNode(0)
            dummyleft = left = ListNode(0)
            curr = head.next
            
            # divide nodes to three lists, =, > or <
            while curr:
                if curr.val == head.val:
                    head.next = curr
                    head = head.next
                elif curr.val > head.val:
                    right.next = curr
                    right = right.next
                else:
                    left.next = curr
                    left = left.next
                curr = curr.next
            
            # to make sure the three lists are not linked
            # these three are tails of the three lists
            head.next = None
            right.next = None
            left.next = None
            
            dummyleft.next = quicksort(dummyleft.next)
            dummyright.next = quicksort(dummyright.next)
            
            # find the tail of the left half
            left = dummyleft
            while left.next:
                left = left.next
            
            # concatenate the 3 lists 
            left.next, head.next = dummyhead, dummyright.next
            
            
            return dummyleft.next
        
        return quicksort(head)    
            
            
            
            
            
            
                
```

