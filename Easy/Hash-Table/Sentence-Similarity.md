## Question
Given two sentences words1, words2 (each represented as an array of strings), and a list of similar word pairs pairs, determine if two sentences are similar.<br>

For example, "great acting skills" and "fine drama talent" are similar, if the similar word pairs are pairs = [["great", "fine"], ["acting","drama"], ["skills","talent"]].<br>

Note that the similarity relation is not transitive. For example, if "great" and "fine" are similar, and "fine" and "good" are similar, "great" and "good" are not necessarily similar.<br>

However, similarity is symmetric. For example, "great" and "fine" being similar is the same as "fine" and "great" being similar.<br>

Also, a word is always similar with itself. For example, the sentences words1 = ["great"], words2 = ["great"], pairs = [] are similar, even though there are no specified similar word pairs.<br>

Finally, sentences can only be similar if they have the same number of words. So a sentence like words1 = ["great"] can never be similar to words2 = ["doubleplus","good"].<br>

Note:<br>

The length of words1 and words2 will not exceed 1000.<br>
The length of pairs will not exceed 2000.<br>
The length of each pairs[i] will be 2.<br>
The length of each words[i] and pairs[i][j] will be in the range [1, 20].<br>

## Thinking


## Coding
Time: O(p+n); p for pairs n for words. </br>
Space: O(p).
```python3
class Solution:
    def areSentencesSimilar(self, words1, words2, pairs):
        """
        :type words1: List[str]
        :type words2: List[str]
        :type pairs: List[List[str]]
        :rtype: bool
        """
        if len(words1) != len(words2):
            return False
        
        
        p_seen = set()
        for p in pairs:
            p_seen.add((p[0],p[1]))
            p_seen.add((p[1],p[0]))
        
        for i in range(len(words1)):
            if words1[i] != words2[i] and (words1[i], words2[i]) not in p_seen:
                return False
        return True
```

