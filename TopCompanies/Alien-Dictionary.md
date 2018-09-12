## Question
There is a new alien language which uses the latin alphabet. However, the order among letters are unknown to you. You receive a list of non-empty words from the dictionary, where words are sorted lexicographically by the rules of this new language. Derive the order of letters in this language

**Example :**   
<pre>
Input:
[
  "wrt",
  "wrf",
  "er",
  "ett",
  "rftt"
]

Output: "wertf"

Input:
[
  "z",
  "x"
]

Output: "zx"

Input:
[
  "z",
  "x",
  "z"
] 

Output: "" 

Explanation: The order is invalid, so return "".
</pre>

Note:<br>

You may assume all letters are in lowercase.<br>
You may assume that if a is a prefix of b, then a must appear before b in the given dictionary.<br>
If the order is invalid, return an empty string.<br>
There may be multiple valid order of letters, return any one of them is fine.<br>

## Thinking


## Coding
Time: O(); <br>
Space: O()
```python3
class Solution(object):
    def alienOrder(self, words):
        map = {}
        letters = [0 for i in range(26)]  
        for i in range(len(words)):
            for j in range(len(words[i])):
                key=ord(words[i][j])-ord('a') 
                letters[key]=0 # Record the level order of the char
                map[key]=set() # Record char i -> char j direction
        
        for i in range(len(words)-1):
            word1 = words[i]
            word2 = words[i+1]
            idx = 0
            for j in range(min(len(word1),len(word2))):
                if(word1[j]!=word2[j]): 
                    key1 = ord(word1[j])-ord('a')
                    key2 = ord(word2[j])-ord('a')
                    count = letters[key2]
                    if(key2 not in map[key1]): # There is no arrow from key1 to key2
                        letters[key2] =count+1 # level+1
                        map[key1].add(key2)  # Add the direction
                    break # The char after is not important, we've found why word1 is smaller than word2
        dictionary = collections.deque()
        res = ''
        for i in range(26):
            if(letters[i]==0 and i in map):
                dictionary.appendleft(i)
        
        while(len(dictionary)!=0):
            nextup = dictionary.pop()
            res+=(chr(nextup+ord('a')))
            greaterSet = map[nextup]
            for greater in greaterSet:
                letters[greater]-=1
                if(letters[greater]==0):
                    dictionary.appendleft(greater)
        if(len(map)!=len(res)):
            return ""
        return res
```

