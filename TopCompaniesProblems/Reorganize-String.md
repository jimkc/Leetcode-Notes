## Question
Given a string S, check if the letters can be rearranged so that two characters that are adjacent to each other are not the same.<br>

If possible, output any possible result.  If not possible, return the empty string.

**Example :**   
<pre>
Input: S = "aab"
Output: "aba"

Input: S = "aaab"
Output: ""
</pre>

## Thinking
Say most frequently occuring character (say c) has frequency max_freq. Then arrange c leaving a space between consecutive c's. The remaining characters should be more than the number of spaces for a valid arrangement. This means that max_freq + (max_freq-1) <= len(S). We can test this quickly using Counter class from collections module and using the mostcommon(i) method which returns a list i most frequent tuples.
Now we create an array called result to store the result. We add (freq*-1,char) tuples to a heap (this is how we simulate a max-heap in Python using a min-heap. We then pick the most frequent element and if that element is not the last element in the result, we add it to result, If it is the last element of the result, then we pick the second most frequent element and add that to the result, and also add back the most frequent element back to the heap. Note when we pop from the heap and utilize the character in the result, we need to add it back to the heap if the frequency is not -1 (-1 means that only one instance of that element was in the heap and we have now used it, so no need to add it back).
Time Complexity is O(N * lg(A)). N is the length of the string. A is the size of the alphabet. The size of the heap will be at-most A. But since we remove and add back elements such that at each iteration we only add one character to the result, there will be N * lg(A) calls. Since A is fixed, we can assume complexity to be O(N).
Space is also O(A).

## Coding
Time: O(); <br
Space: O()
```python3
class Solution:
    def reorganizeString(self, S):
        """
        :type S: str
        :rtype: str
        """
        cnt = collections.Counter(S)
        maxfreq = cnt.most_common(1)[0][1]
        heap = []
        
        # Means the empty space of maxfreq cant be filled 
        if maxfreq-1 > len(S)-maxfreq:
            return ''
        
        for k,v in cnt.items():
            heapq.heappush(heap,(-v,k)) # Use min heap to simulate max heap
        ans = []
        
        while heap:
            v1, k1 = heapq.heappop(heap) # Largest freq
            
            if not ans or k1!=ans[-1]:
                ans.append(k1)
                v1+=1
                if v1 < 0:
                    heapq.heappush(heap,(v1,k1))
            else:
                v2, k2 = heapq.heappop(heap) # Second largest freq
                ans.append(k2)
                v2+=1
                if v2 < 0:
                    heapq.heappush(heap,(v2,k2))
                heapq.heappush(heap,(v1,k1))
        return ''.join(ans)
```


Faster:
```python3
from collections import Counter
from heapq import heappush,heappop 

class Solution:
    def reorganizeString(self, S):
        """
        :type S: str
        :rtype: str
        """
        hs = Counter(S)
        hp = []
        for key in hs:
            heappush(hp, (-hs[key], key))
        
        
        if -hp[0][0] * 2 - 1 > len(S):
            return ""
        ans = []
        while len(hp) >= 2:
            (cnt1, c1) = heappop(hp)
            (cnt2, c2) = heappop(hp)
            
            ans.extend([c1, c2])
            
            if cnt1 + 1: 
                heappush(hp, (cnt1 + 1, c1))
            if cnt2 + 1:
                heappush(hp, (cnt2 + 1, c2))
        if hp:
            ans.extend(hp[0][1])
        return "".join(ans)
```
