## Question
Given a list of words and two words word1 and word2, return the shortest distance between these two words in the list.

**Example :**
<pre>
Assume that words = ["practice", "makes", "perfect", "coding", "makes"].

Given word1 = “coding”, word2 = “practice”, return 3.
Given word1 = "makes", word2 = "coding", return 1.
</pre>
Note: You may assume that word1 does not equal to word2, and word1 and word2 are both in the list.

## Thinking
Go through the whole array, whenever we found intersection of word1 and word2 without considering the words other than them between the two
words, we count the distance and compare to the current shortest distance.
## Coding
Time: O(n); Go through the array once only. </br>
Space: O(1) 
```python3
class Solution:
    def shortestDistance(self, words, word1, word2):
        """
        :type words: List[str]
        :type word1: str
        :type word2: str
        :rtype: int
        """
        word_index = {} 
        index = 0
        shortest_dist = len(words)
        
        
        pre_word = words[0]
        pre_index = 0
        for i,w in enumerate(words):
            if w == word1:
                if pre_word == word1:
                    pass
                elif pre_word == word2:
                    dist = i - pre_index
                    if dist < shortest_dist:
                        shortest_dist = dist
                pre_word = word1
                pre_index = i
            elif w == word2:
                if pre_word == word2:
                    pass
                elif pre_word == word1:
                    dist = i - pre_index
                    if dist < shortest_dist:
                        shortest_dist = dist
                pre_word = word2
                pre_index = i
        return shortest_dist
                    
                    
```

