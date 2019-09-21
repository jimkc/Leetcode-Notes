## Question
Given a linked list and a value x, partition it such that all nodes less than x come before nodes greater than or equal to x.<br>

You should preserve the original relative order of the nodes in each of the two partitions.

**Example :**   
<pre>
Input: head = 1->4->3->2->5->2, x = 3
Output: 1->2->2->4->3->5
</pre>

## Thinking
和sort list 那題的quicksort有點像，用兩個額外的list，一個放比x大或等於，一個放比x小的。最後在合起來。

## Coding
Time: O(n);</br>
Space: O(n)
```python
class Solution(object):
    def partition(self, head, x):
        """
        :type head: ListNode
        :type x: int
        :rtype: ListNode
        """

        # before and after are the two pointers used to create two list
        # before_head and after_head are used to save the heads of the two lists.
        # All of these are initialized with the dummy nodes created.
        before = before_head = ListNode(0)
        after = after_head = ListNode(0)

        while head:
            # If the original list node is lesser than the given x,
            # assign it to the before list.
            if head.val < x:
                before.next = head
                before = before.next
            else:
                # If the original list node is greater or equal to the given x,
                # assign it to the after list.
                after.next = head
                after = after.next

            # move ahead in the original list
            head = head.next

        # Last node of "after" list would also be ending node of the reformed list
        after.next = None
        # Once all the nodes are correctly assigned to the two lists,
        # combine them to form a single list which would be returned.
        before.next = after_head.next

        return before_head.next
```

