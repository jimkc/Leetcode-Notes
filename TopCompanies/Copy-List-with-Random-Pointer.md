## Question
A linked list is given such that each node contains an additional random pointer which could point to any node in the list or null.<br>

Return a deep copy of the list.

## Thinking


## Coding
Time: O(n);<br>
Space: O(n)
```python
# Definition for singly-linked list with a random pointer.
# class RandomListNode(object):
#     def __init__(self, x):
#         self.label = x
#         self.next = None
#         self.random = None

class Solution:
# @param head, a RandomListNode
# @return a RandomListNode
    def copyRandomList(self, head):
        dic = dict()
        m = n = head
        while m:
            dic[m] = RandomListNode(m.label) # Original node as key, create new node as value
            m = m.next
        while n:
            dic[n].next = dic.get(n.next) # Get returns None if not exists
            dic[n].random = dic.get(n.random)
            n = n.next
        return dic.get(head)
```
