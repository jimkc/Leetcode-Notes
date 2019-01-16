## Question
Given a linked list, return the node where the cycle begins. If there is no cycle, return null.<br>

To represent a cycle in the given linked list, we use an integer pos which represents the position (0-indexed) in the linked list where tail connects to. If pos is -1, then there is no cycle in the linked list.<br>

Note: Do not modify the linked list.</br>

**Example :**   
<pre>
Input: head = [3,2,0,-4], pos = 1 (picture has -4 connected to 2)
Output: tail connects to node index 1
Explanation: There is a cycle in the linked list, where tail connects to the second node.

Input: head = [1,2], pos = 0 (2 connected to 1)
Output: tail connects to node index 0
Explanation: There is a cycle in the linked list, where tail connects to the first node.

Input: head = [1], pos = -1
Output: no cycle
Explanation: There is no cycle in the linked list.
</pre>

## Thinking
這個問題可以拆成兩部分來看。
1. 第一部分，先檢查有沒有cycle的存在，利用兩個pointer，一個快一個慢，若他們會相遇就代表有cycle存在，並令相遇的點稱為X。<br>
2. 第二部分是找出entry point (定義為E)，這部分我們用數學是證明。<br>
   因為第一部分的快和慢pointer，所以我們可以把他們走的距離寫成一個等式。令起始點叫做H，走一圈cycle的距離叫做C<br>
   -> 2(HE + EX) = HE + nC + EX (這邊的n是不知道走了幾圈)<br>
   -> HE  = nC -EX<br>
   從上面等式我們可以知道當pointer從H走到E時，另一個從X出發的pointer（速度一樣）會從X走n圈又少EX的距離，也就會回到E，我們的答案。<br>
   在知道X之後，只要讓一個點從H出發，一個點從X出發，等他們相遇的時候就會是entry point了<br>


## Coding
Time: O(n); <br>
Space: O(1)
```python3
# Definition for singly-linked list.
# class ListNode(object):
#     def __init__(self, x):
#         self.val = x
#         self.next = None

class Solution(object):
    def detectCycle(self, head):
        """
        :type head: ListNode
        :rtype: ListNode
        """
        dummy = ListNode(0)
        dummy.next = head
        fast, slow = dummy, dummy
        slow2 = dummy
        
        # part 1 checks if there exists a cycle, and find the meet up point
        while fast and fast.next:
            fast = fast.next.next
            slow = slow.next
            
            if fast == slow:
                break
                
        # fast may stop in the last node of a non-cycle list, so we need to check fast.next too
        if fast == None or fast.next == None:
            return None
        
        # part 2, the distance of head to entry point, will be the same as the distance from meet up point to the entry point
        while slow != slow2:
            slow = slow.next
            slow2 = slow2.next
            
        return slow
        
                
```

