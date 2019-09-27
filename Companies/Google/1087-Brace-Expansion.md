## Question
A string S represents a list of words.<br>
<br>
Each letter in the word has 1 or more options.  If there is one option, the letter is represented as is.  If there is more than one option, then curly braces delimit the options.  For example, "{a,b,c}" represents options ["a", "b", "c"].<br>
<br>
For example, "{a,b,c}d{e,f}" represents the list ["ade", "adf", "bde", "bdf", "cde", "cdf"].<br>
<br>
Return all words that can be formed in this manner, in lexicographical order.<br>

**Example :**   
<pre>
Input: "{a,b}c{d,e}f"
Output: ["acdf","acef","bcdf","bcef"]

Input: "abcd"
Output: ["abcd"]
</pre>

Note:<br>

1 <= S.length <= 50<br>
There are no nested curly brackets.<br>
All characters inside a pair of consecutive opening and ending curly brackets are different.

## Thinking
When we met "{" it means the char we met later are going to become options that we need to do permutation with the previous results until we met "}".

## Coding
Time: O(n^2); </br>
Space: O(n^2)
```python
class Solution:
    def expand(self, S: str) -> List[str]:
        
        ans = [""]
        options = []
        doPermutation = False
        for c in S:
            if c == ",":
                continue
            elif c == "{":
                # The chars later are supposed to do permutation with previous results
                doPermutation = True
            elif c == "}":
                ans = [pre+c for pre in ans for c in options]
                # refresh the options to none
                options = []
                # because we are out of the curly brackets
                doPermutation = False
            elif doPermutation:
                # the char in the curly brackets
                options.append(c)
            else:
                # condition that meets char not in curly brackets
                ans = [x+c for x in ans]
                
        return sorted(ans)
                

```

```python
def permute(self, S):
        A = S.replace('{', ' ').replace('}', ' ').strip().split(' ')
        B = [sorted(a.split(',')) for a in A]
        return [''.join(c) for c in itertools.product(*B)]
```

