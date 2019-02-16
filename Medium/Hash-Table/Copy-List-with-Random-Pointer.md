## Question
A linked list is given such that each node contains an additional random pointer which could point to any node in the list or null.<br>

Return a deep copy of the list.

## Thinking
1. Since we have to create the deep copy of the linked list, we cant simply create node while traversing, because in that case some nodes pointed from random may not be created yet.<br>
2. We create a dictionay with old list nodes as key and new list nodes as value.<br>
3. Traverse first time to create nodes with values, then create second time to connect nodes.<br>
4. Hash table help us to know the order and connected later.

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
        # do deep copy of nodes and put it in dictionary
        while m:
            dic[m] = RandomListNode(m.label) # Original node as key, create new node as value
            m = m.next
        # connect the created nodes
        while n:
            dic[n].next = dic.get(n.next) # Get returns None if not exists
            dic[n].random = dic.get(n.random)
            n = n.next
        return dic.get(head)
```
