## Question
Given two lists Aand B, and B is an anagram of A. B is an anagram of A means B is made by randomizing the order of the elements in A.<br>

We want to find an index mapping P, from A to B. A mapping P[i] = j means the ith element in A appears in B at index j.<br>

These lists A and B may contain duplicates. If there are multiple answers, output any of them.


**Example :**   
<pre>
A = [12, 28, 46, 32, 50]
B = [50, 12, 32, 46, 28]

We should return
[1, 4, 3, 2, 0]
</pre>

## Thinking


## Coding
Time: O(n); </br>
Space: O(n)
```python
class Solution:
    def anagramMappings(self, A: List[int], B: List[int]) -> List[int]:
        
        index_match = {}
        ans = []
        
        for i, e in enumerate(B):
            if e in index_match.keys():
                index_match[e].append(i)
            else:
                index_match[e] = [i]

        for i, e in enumerate(A):
            ans.append(index_match[e].pop())
            
        
        return ans
            
```

