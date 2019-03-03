## Question
You are given two non-empty linked lists representing two non-negative integers. The digits are stored in reverse order and each of their nodes contain a single digit. Add the two numbers and return it as a linked list.<br>

You may assume the two numbers do not contain any leading zero, except the number 0 itself.

**Example :**   
<pre>
Input: (2 -> 4 -> 3) + (5 -> 6 -> 4)
Output: 7 -> 0 -> 8
Explanation: 342 + 465 = 807.
</pre>

## Thinking

## Coding
Time: O(n); <br>
Space: O(1)<br>

python version:
```python
# class ListNode:
#     def __init__(self, x):
#         self.val = x
#         self.next = None

class Solution:
    def addTwoNumbers(self, l1, l2):
        """
        :type l1: ListNode
        :type l2: ListNode
        :rtype: ListNode
        """
        prev = None
        head = l1
        carry = 0
        while l1 and l2:
            n = l1.val + l2.val + carry
            val, carry = n % 10, n // 10
            l1.val = val
            prev, l1, l2 =l1, l1.next, l2.next # Need prev after we finish the loop
            
        l = l1 or l2 
        prev.next = l # to link the previous added list to the rest of the uncounted list (l1 or l2)
        
        while l:
            n = l.val + carry
            val, carry = n % 10, n // 10
            l.val = val
            prev, l = l, l.next
            
        # In case there is carry after last digit
        if carry: 
            prev.next = ListNode(carry)
        
        return head
```

java version:
```java
/**
 * Definition for singly-linked list.
 * public class ListNode {
 *     int val;
 *     ListNode next;
 *     ListNode(int x) { val = x; }
 * }
 */
import java.util.*;
class Solution {
    public ListNode addTwoNumbers(ListNode l1, ListNode l2) {
        
        ListNode dummy = new ListNode(0);
        dummy.next = l1;
        ListNode prev = dummy;
        int carry = 0;
        
        while(l1!=null && l2!=null){
            
            int sum = l1.val+l2.val+carry;
            int value = sum%10;
            carry = (int)sum/10;

            l1.val = value;
            prev = l1;
            l1 = l1.next;
            l2 = l2.next;
            
        }
        
        ListNode l = null;
        
        if(l1!=null){
            l = l1;
        }
        else{
            l = l2;
        }
        
        prev.next = l;
        //System.out.println(carry);
        while(l!=null){
            int sum = l.val+carry;
            int value = sum%10;
            carry = (int)sum/10;
            
            l.val = value;
            prev = l;
            l = l.next;
        }
        
        if (carry!=0){
            ListNode node = new ListNode(carry);
            prev.next = node;
        }
        
        
        return dummy.next;
    }
}
```



