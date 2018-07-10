## Question
Given a string s and a non-empty string p, find all the start indices of p's anagrams in s.<br>

Strings consists of lowercase English letters only and the length of both strings s and p will not be larger than 20,100.<br>

The order of output does not matter.<br>

**Example :**
<pre>
Input:
s: "cbaebabacd" p: "abc"

Output:
[0, 6]

Explanation:
The substring with start index = 0 is "cba", which is an anagram of "abc".
The substring with start index = 6 is "bac", which is an anagram of "abc".



Input:
s: "abab" p: "ab"

Output:
[0, 1, 2]

Explanation:
The substring with start index = 0 is "ab", which is an anagram of "ab".
The substring with start index = 1 is "ba", which is an anagram of "ab".
The substring with start index = 2 is "ab", which is an anagram of "ab".
</pre>


## Thinking
The reason we use array instead of dic is because we have to check if the char is inside
the dic and the appear times at the same time, and the phash may not have element to compare
with an element that appears 0 times in the shash.

## Coding
Time: O(p+s); </br>
Space: O(s).
```python3
class Solution:
    def findAnagrams(self, s, p):
        """
        :type s: str
        :type p: str
        :rtype: List[int]
        """
        ans = []
        ls, lp = len(s), len(p)
        if ls < lp: return ans
        phash, shash = [0]*123, [0]*123
        
        for c in p:
            phash[ord(c)] += 1
        for c in s[:lp]:
            shash[ord(c)] += 1
        if shash == phash:
            ans.append(0)
        for i in range(1, ls-(lp-1)):
            shash[ord(s[i-1])] -= 1
            shash[ord(s[i+(lp-1)])] += 1
            if shash == phash:
                ans.append(i)
                
        return ans
```

