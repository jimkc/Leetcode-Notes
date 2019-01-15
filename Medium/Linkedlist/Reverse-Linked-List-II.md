## Question
Reverse a linked list from position m to n. Do it in one-pass. </br>

Note: 1 ≤ m ≤ n ≤ length of list.

**Example :**   
<pre>
Input: 1->2->3->4->5->NULL, m = 2, n = 4
Output: 1->4->3->2->5->NULL
</pre>

## Thinking
1. reverse the selected part. <br>
2. check what our head is now. <br>
記得檢查是否有倒轉整個list,以確保回傳的head是對的

## Coding
Time: O(n);One pass <br>
Space: O(1)
```python3
class Solution:
    def reverseBetween(self, head, m, n):
        """
        :type head: ListNode
        :type m: int
        :type n: int
        :rtype: ListNode
        """
        
        prevnode = None # node before current node
        before_revhead = None # node before the first node of reversed part
        currentnode = head
        
        # move currentnode to the first node of the reversed part
        for i in range(1,m):
            before_revhead = currentnode
            currentnode = currentnode.next
        
        start = currentnode # start of the reversed part
        
        # reverse
        for i in range(m,n+1):
            nextnode = currentnode.next
            currentnode.next = prevnode
            prevnode = currentnode
            currentnode = nextnode
        
        # if the before_revhead is not None, then we have to point the node before the start of the rerversed list back to the head of the reversed list
        if before_revhead is not None:
            before_revhead.next = prevnode
        
        # the start is now the last node of the reversed part, so we need to point start to the current node, which is next node of the last node of the reversed part before reversing
        start.next = currentnode

        if before_revhead is not None:
            return head
        else:
            return prevnode
```

