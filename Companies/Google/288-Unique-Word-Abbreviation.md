## Question
An abbreviation of a word follows the form <first letter><number><last letter>. Below are some examples of word abbreviations:

<pre>
a) it                      --> it    (no abbreviation)

     1
     ↓
b) d|o|g                   --> d1g

              1    1  1
     1---5----0----5--8
     ↓   ↓    ↓    ↓  ↓    
c) i|nternationalizatio|n  --> i18n

              1
     1---5----0
     ↓   ↓    ↓
d) l|ocalizatio|n          --> l10n
</pre>

**Example :**   
<pre>
Given dictionary = [ "deer", "door", "cake", "card" ]

isUnique("dear") -> false
isUnique("cart") -> true
isUnique("cane") -> false
isUnique("make") -> true
</pre>

Assume you have a dictionary and given a word, find whether its abbreviation is unique in the dictionary. <br>
A word's abbreviation is unique if no other word from the dictionary has the same abbreviation.<br>

## Thinking

## Coding
Time: O(n);</br>
Space: O(n)
```python
class ValidWordAbbr:
    def __init__(self, dictionary: List[str]):
        self.abrv_dic = collections.defaultdict(set)

        for word in dictionary:
            if len(word) < 3:
                self.abrv_dic[word] = set([word])
            else:
                abrv_word = "{}{}{}".format(word[0],len(word[1:-1]),word[-1])
                self.abrv_dic[abrv_word].add(word)
        
    def isUnique(self, word: str) -> bool:
        abrv_word = ""
        if len(word) < 3:
            abrv_word = word
        else:
            abrv_word = "{}{}{}".format(word[0],len(word[1:-1]),word[-1])
        
        if abrv_word not in self.abrv_dic:
            return True
        elif len(self.abrv_dic[abrv_word]) == 1 and word in self.abrv_dic[abrv_word]: 
            return True
        else:
            return False
            
# Your ValidWordAbbr object will be instantiated and called as such:
# obj = ValidWordAbbr(dictionary)
# param_1 = obj.isUnique(word)
```

