## Question
Write a program to find the node at which the intersection of two singly linked lists begins.

**Example :**
<pre>
A:          a1 → a2
                   ↘
                     c1 → c2 → c3
                   ↗            
B:     b1 → b2 → b3
</pre>
begin to intersect at node c1.</br>


Notes:

If the two linked lists have no intersection at all, return null.</br>
The linked lists must retain their original structure after the function returns.</br>
You may assume there are no cycles anywhere in the entire linked structure.</br>
Your code should preferably run in O(n) time and use only O(1) memory.</br>

## Thinking
**1.** Maintain two pointers pA and pB initialized at the head of A and B, respectively. Then let them both traverse through the lists, one node at a time.</br>
**2.** When pA reaches the end of a list, then redirect it to the head of B (yes, B, that's right.); similarly when pB reaches the end of a list, redirect it the head of A.</br>
**3.** If at any point pA meets pB, then pA/pB is the intersection node.</br>
**4.** To see why the above trick would work, consider the following two lists: A = {1,3,5,7,9,11} and B = {2,4,9,11}, which are intersected at node '9'. Since B.length (=4) < A.length (=6), pB would reach the end of the merged list first, because pB traverses exactly 2 nodes less than pA does. By redirecting pB to head A, and pA to head B, we now ask pB to travel exactly 2 more nodes than pA would. So in the second iteration, they are guaranteed to reach the intersection node at the same time.</br>
**5.** If two lists have intersection, then their last nodes must be the same one. So when pA/pB reaches the end of a list, record the last element of A/B respectively. If the two last elements are not the same one, then the two lists have no intersections.</br>
## Coding
Time: O(m+n); Go through the both list once. </br>
Space: O(1) 
```python3
# Definition for singly-linked list.
# class ListNode(object):
#     def __init__(self, x):
#         self.val = x
#         self.next = None

class Solution(object):
    def getIntersectionNode(self, headA, headB):
        """
        :type head1, head1: ListNode
        :rtype: ListNode
        """
        if not headA or not headB:
            return None
        
        preheadA = headA
        preheadB = headB
        
        while headA or headB:
            if headA == None:
                headA = preheadB
            if headB == None:
                headB = preheadA
            if headA.val == headB.val:
                return headA
            else:
                headA = headA.next
                headB = headB.next
        return None
            
                
```

