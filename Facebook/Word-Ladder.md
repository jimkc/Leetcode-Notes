## Question
Given two words (beginWord and endWord), and a dictionary's word list, find the length of shortest transformation sequence from beginWord to endWord, such that:<br>

Only one letter can be changed at a time.<br>
Each transformed word must exist in the word list. Note that beginWord is not a transformed word.<br>
Note:<br>

Return 0 if there is no such transformation sequence.<br>
All words have the same length.<br>
All words contain only lowercase alphabetic characters.<br>
You may assume no duplicates in the word list.<br>
You may assume beginWord and endWord are non-empty and are not the same.<br>

**Example :**   
<pre>
Input:
beginWord = "hit",
endWord = "cog",
wordList = ["hot","dot","dog","lot","log","cog"]

Output: 5

Explanation: As one shortest transformation is "hit" -> "hot" -> "dot" -> "dog" -> "cog",
return its length 5.


Input:
beginWord = "hit"
endWord = "cog"
wordList = ["hot","dot","dog","lot","log"]

Output: 0

Explanation: The endWord "cog" is not in wordList, therefore no possible transformation.
</pre>

## Thinking


## Coding
Time: O(); <br>
Space: O()
```python3
class Solution:
    def ladderLength(self, beginWord, endWord, wordList):
        """
        :type beginWord: str
        :type endWord: str
        :type wordList: List[str]
        :rtype: int
        """
        
        
        wordSet = set(wordList)
        deque = collections.deque([[beginWord,1]]) # Deque can only initialize list
        
        while deque:
            word, length = deque.popleft()
            
            if word == endWord:
                return length
            
            for c in 'abcdefghijklmnopqrstuvwxyz':
                for i in range(len(word)):
                    next_word = word[:i] + c + word[i+1:]
                    if next_word in wordSet:
                        wordSet.remove(next_word)
                        deque.append([next_word,length+1])
        return 0
```

Second Faster Method (Confused)
```python3
class Solution:
    def ladderLength(self, beginWord, endWord, wordList):
        """
        :type beginWord: str
        :type endWord: str
        :type wordList: List[str]
        :rtype: int
        """           
        if not endWord in wordList:
            return 0
        
        wordSet = set(wordList)
        startSet = set()
        endSet = set()
        visited = set()
        
        startSet.add(beginWord)
        endSet.add(endWord)
        visited.add(beginWord)
        visited.add(endWord)
        
        distance = 1
        w_length = len(beginWord)
        
        while len(startSet) and len(endSet):
            if len(startSet) > len(endSet):
                startSet, endSet = endSet, startSet
            
            tmpSet = set()
            for word in startSet:          
                for i in range(w_length):
                    ## important!!! this saves time
                    part1 = word[:i]
                    part2 = word[i+1:]
                    for alpha in "abcdefghijklmnopqrstuvwxyz":
                        gen_word = part1 + alpha + part2
                        if gen_word in endSet:
                            return distance + 1
                        elif not gen_word in visited and gen_word in wordSet:
                            tmpSet.add(gen_word)
                            visited.add(gen_word)
            startSet = tmpSet
            distance += 1
        return 0
```
