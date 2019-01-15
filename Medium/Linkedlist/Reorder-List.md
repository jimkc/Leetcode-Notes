## Question
Given a singly linked list L: L0→L1→…→Ln-1→Ln,<br>
reorder it to: L0→Ln→L1→Ln-1→L2→Ln-2→…<br>

You may not modify the values in the list's nodes, only nodes itself may be changed. </br>

**Example :**   
<pre>
Given 1->2->3->4, reorder it to 1->4->2->3.

Given 1->2->3->4->5, reorder it to 1->5->2->4->3.
</pre>

## Thinking
3 steps to do this.<br>
1. Find the head of the splited list, one in head one in middle.<br>
2. Reverse the second half list, change the pointers inplace<br>
3. Merge the two list inplace.


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

    # @param head, a ListNode
    # @return nothing
    def reorderList(self, head):

        if not head or not head.next:
            return

        a, b = _splitList(head)
        b = _reverseList(b)
        head = _mergeLists(a, b)

# Splits in place a list in two halves, the first half is >= in size than the second.
# @return A tuple containing the heads of the two halves
def _splitList(head):
    fast = head
    slow = head
    while fast and fast.next:
        slow = slow.next
        fast = fast.next
        fast = fast.next

    middle = slow.next
    slow.next = None

    return head, middle

# Reverses in place a list.
# @return Returns the head of the new reversed list
def _reverseList(head):

    last = None
    currentNode = head

    while currentNode:
        nextNode = currentNode.next
        currentNode.next = last
        last = currentNode
        currentNode = nextNode

    return last

# Merges in place two lists
# @return The newly merged list.
def _mergeLists(a, b):

    tail = a
    head = a

    # whil a is a.next, tail is still a
    # a is kind of checking do we still have next node to point to 
    a = a.next
    while b:
        tail.next = b 
        tail = tail.next # tail moves to another list
        b = b.next 
        # now tail is in b, so a is the node of another list, and it is the next node we are going to add
        # if exist, we switch a,b, so the next round we can point tail's next to b, which is another list
        if a:
            a, b = b, a
            
    return head



```

