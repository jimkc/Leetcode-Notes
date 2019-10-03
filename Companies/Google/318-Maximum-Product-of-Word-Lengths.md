## Question
Given a string array words, find the maximum value of length(word[i]) * length(word[j]) where the two words do not share common letters. You may assume that each word will contain only lower case letters. If no such two words exist, return 0.</br>

**Example :**   
<pre>
Input: ["abcw","baz","foo","bar","xtfn","abcdef"]
Output: 16 
Explanation: The two words can be "abcw", "xtfn".

Input: ["a","ab","abc","d","cd","bcd","abcd"]
Output: 4 
Explanation: The two words can be "ab", "cd".

Input: ["a","aa","aaa","aaaa"]
Output: 0 
Explanation: No such pair of words.
</pre>

## Thinking
https://leetcode.com/problems/maximum-product-of-word-lengths/solution/

## Coding
Time: O(n^2 * l) where n is the number of words and l is the number of total chars. The precompute takes O(l); </br>
Space: O(n)
```python
class Solution:
    def maxProduct(self, words: List[str]) -> int:
        
        max_prod = 0
        # record the words as one bitmasks of 26 bits (letters), 1 as the letter exists in word 0 as not
        masks = {}
        # get the index of the letter count from a
        bit_index = lambda c: ord(c)-ord('a')
        
        for w in words:
            bitmask = 0
            for ch in w:
                # ch exists, so put the index in bitmask to 1 using or equals (or is bitwise)
                bitmask |= 1<<bit_index(ch)
            masks[w] = bitmask
            
        for i in range(len(words)):
            for j in range(i+1,len(words)):
                w1 = words[i]
                w2 = words[j]
                # is and is 0, then each bit is different in w1 ans w2
                if masks[w1] & masks[w2] == 0:
                    max_prod = max(max_prod, len(w1)*len(w2))
        return max_prod
```

