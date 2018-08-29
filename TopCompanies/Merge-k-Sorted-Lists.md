## Question
Merge k sorted linked lists and return it as one sorted list. Analyze and describe its complexity.

**Example :**   
<pre>
Input:
[
  1->4->5,
  1->3->4,
  2->6
]
Output: 1->1->2->3->4->4->5->6
</pre>

## Thinking
This approach walks alongside the one above but is improved a lot. We don't need to traverse most nodes many times repeatedly<br>

Pair up \text{k}k lists and merge each pair.<br>

After the first pairing, \text{k}k lists are merged into k/2k/2 lists with average 2N/k2N/k length, then k/4k/4, k/8k/8 and so on.<br>

Repeat this procedure until we get the final sorted linked list.<br>

Thus, we'll traverse almost NN nodes per pairing and merging, and repeat this procedure about \log_{2}{k}log
​2​​k times.

## Coding
Time: O(n);<br> 
Space: O(1)
```python3
# Definition for singly-linked list.
# class ListNode:
#     def __init__(self, x):
#         self.val = x
#         self.next = None

class Solution:
    def mergeKLists(self, lists):
        """
        :type lists: List[ListNode]
        :rtype: ListNode
        """
        amount = len(lists)
        interval = 1
        while interval < amount:
            for i in range(0, amount - interval, interval * 2):
                lists[i] = self.merge2Lists(lists[i], lists[i + interval])
            interval *= 2
        return lists[0] if amount > 0 else lists
                
        
    
    def merge2Lists(self, l1, l2):
        head = point = ListNode(0)
        
        while l1 and l2:
            if l1.val <= l2.val:
                point.next = l1
                l1 = l1.next
            else:
                point.next = l2
                l2 = l2.next
            point = point.next
        
        if l1:
            point.next = l1
        else:
            point.next = l2
        
        return head.next
        
```

Method 2 with queue(heap):<br>
Time: O(n);<br>
Space: O(n); 
```python3
from Queue import PriorityQueue

class Solution(object):
    def mergeKLists(self, lists):
        """
        :type lists: List[ListNode]
        :rtype: ListNode
        """
        head = point = ListNode(0)
        q = PriorityQueue()
        for l in lists:
            if l:
                q.put((l.val, l))
        while not q.empty():
            val, node = q.get()
            point.next = ListNode(val)
            point = point.next
            node = node.next
            if node:
                q.put((node.val, node))
        return head.next
```

