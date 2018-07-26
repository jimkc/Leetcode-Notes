## Question
Implement a trie with insert, search, and startsWith methods.

**Example :**   
<pre>
Trie trie = new Trie();

trie.insert("apple");
trie.search("apple");   // returns true
trie.search("app");     // returns false
trie.startsWith("app"); // returns true
trie.insert("app");   
trie.search("app");     // returns true
</pre>

Note:<br>

You may assume that all inputs are consist of lowercase letters a-z.<br>
All inputs are guaranteed to be non-empty strings.<br>

## Thinking
Every step saves one char will be a lot faster than saving a substring.

## Coding
Time: O();<br>
Space: O(1)
```python3
class Trie:

    def __init__(self):
        """
        Initialize your data structure here.
        """
        self.root = {}
        self.trie = self.root

    def insert(self, word):
        """
        Inserts a word into the trie.
        :type word: str
        :rtype: void
        """
        self.trie = self.root
        for w in word:
            if w not in self.trie:
                self.trie[w] = {}
            self.trie = self.trie[w]
            
        self.trie['value'] = word
        #print (self.root)
        

    def search(self, word):
        """
        Returns if the word is in the trie.
        :type word: str
        :rtype: bool
        """
        self.trie = self.root
        
        for w in word:
            if w not in self.trie:
                return False
            self.trie = self.trie[w]
        
        return 'value' in self.trie
            
        
        

    def startsWith(self, prefix):
        """
        Returns if there is any word in the trie that starts with the given prefix.
        :type prefix: str
        :rtype: bool
        """
        self.trie = self.root
        for p in prefix:
            if p not in self.trie:
                return False
            self.trie = self.trie[p]
        return True
            
        


# Your Trie object will be instantiated and called as such:
# obj = Trie()
# obj.insert(word)
# param_2 = obj.search(word)
# param_3 = obj.startsWith(prefix)
```

