## Question
Given a string, sort it in decreasing order based on the frequency of characters.</br>

**Example :**   
<pre>
Input:
"tree"

Output:
"eert"

Explanation:
'e' appears twice while 'r' and 't' both appear once.
So 'e' must appear before both 'r' and 't'. Therefore "eetr" is also a valid answer.

Input:
"cccaaa"

Output:
"cccaaa"

Explanation:
Both 'c' and 'a' appear three times, so "aaaccc" is also a valid answer.
Note that "cacaca" is incorrect, as the same characters must be together.

Input:
"Aabb"

Output:
"bbAa"

Explanation:
"bbaA" is also a valid answer, but "Aabb" is incorrect.
Note that 'A' and 'a' are treated as two different characters.
</pre>

## Thinking


## Coding
Time: O(n); </br>
Space: O(n)
```python
class Solution:
    def frequencySort(self, s: str) -> str:
        ans = []
        count = collections.Counter(s)
        
        freq_dict = collections.defaultdict(list)
        for k,v in count.items():
            freq_dict[v].append(k)
        
        freq_sorted_keys = sorted(freq_dict.keys(), reverse=True)
        
        for freq in freq_sorted_keys:
            for c in freq_dict[freq]:
                for _ in range(freq):
                    ans.append(c)
        return "".join(ans)
```

