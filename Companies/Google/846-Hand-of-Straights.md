## Question
Alice has a hand of cards, given as an array of integers.<br>

Now she wants to rearrange the cards into groups so that each group is size W, and consists of W consecutive cards.<br>

Return true if and only if she can.</br>

**Example :**   
<pre>
Input: hand = [1,2,3,6,2,3,4,7,8], W = 3
Output: true
Explanation: Alice's hand can be rearranged as [1,2,3],[2,3,4],[6,7,8].

Input: hand = [1,2,3,4,5], W = 4
Output: false
Explanation: Alice's hand can't be rearranged into groups of 4.
</pre>

Note:<br>

1 <= hand.length <= 10000<br>
0 <= hand[i] <= 10^9<br>
1 <= W <= hand.length

## Thinking


## Coding
Time: O(nlogn+n); actually is 2n as every element is traversed twice</br>
Space: O(n)
```python
class Solution:
    def isNStraightHand(self, hand: List[int], W: int) -> bool:
        
        if len(hand)% W != 0:
            return False
        
        seen = collections.defaultdict(int)
        
        for n in hand:
            seen[n]+=1
        
        sorted_nums = sorted(hand)
        
        for n in sorted_nums:
            num = n
            # if the number is not all used as others consecutive array, 
            # start counting a new consecutive array
            if seen[num] > 0:
                seen[num] -= 1
                # check the consecutive next W-1 nnumbers
                for _ in range(W-1):
                    if seen[num+1] > 0:
                        seen[num+1] -= 1
                        num+=1
                    else:
                        return False
            
        return True
```

