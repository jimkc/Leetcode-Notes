## Question
A strobogrammatic number is a number that looks the same when rotated 180 degrees (looked at upside down).<br>

Find all strobogrammatic numbers that are of length = n.

**Example :**   
<pre>
Input:  n = 2
Output: ["11","69","88","96"]
</pre>


## Thinking
1. The first method gets the permutation of the left part fist and then map the right part.<br>
2. The second is more efficient.<br>
Some observation to the sequence: <br>

n == 1: [0, 1, 8]<br>

n == 2: [11, 88, 69, 96]<br>

How about n == 3?<br>
=> it can be retrieved if you insert [0, 1, 8] to the middle of solution of n == 2<br>

n == 4?<br>
=> it can be retrieved if you insert [11, 88, 69, 96, 00] to the middle of solution of n == 2<br>

n == 5?<br>
=> it can be retrieved if you insert [0, 1, 8] to the middle of solution of n == 4<br>

the same, for n == 6, it can be retrieved if you insert [11, 88, 69, 96, 00] to the middle of solution of n == 4<br>


## Coding
Time: O();  </br>
Space: O()
```python
class Solution:
    def findStrobogrammatic(self, n: int) -> List[str]:
        
        stbg_map = {"0":"0","1":"1", "6":"9", "8":"8", "9":"6"}
        stbg_nums = ["0","1","6","8","9"]
        
        
        
        m = n//2
        patterns = []
        
        
        
        # append left part of patterns
        self.per(m, stbg_nums, patterns, "")
        
        for i in range(len(patterns)):
            tmp = patterns[i]
            for j in range(len(tmp)-1,-1,-1):
                patterns[i]+=stbg_map[tmp[j]]
        
        
        
        if n%2 != 0:
            mid_idx = n//2
            pat_len = len(patterns)
            
            for i in range(pat_len):
                tmp = patterns[i]
                patterns[i] = tmp[:mid_idx]+"1"+ tmp[mid_idx:]
                patterns.append(tmp[:mid_idx]+"8"+ tmp[mid_idx:])
                if m >0:
                    patterns.append(tmp[:mid_idx]+"0"+ tmp[mid_idx:])
            
        if m == 0:
            patterns.append("0")
        
        return patterns             
        
    
    def per(self, m, stbg_nums, pattern, tmp):
        
        if m == 0:
            pattern.append(tmp)
            return
        
        for item in stbg_nums:
            if tmp == "" and item=="0":
                continue
            self.per(m-1, stbg_nums, pattern, tmp+item)
            
```

```python
class Solution:
    def findStrobogrammatic(self, n):
        evenMidCandidate = ["11","69","88","96", "00"]
        oddMidCandidate = ["0", "1", "8"]
        if n == 1:
            return oddMidCandidate
        if n == 2:
            return evenMidCandidate[:-1]
        if n % 2:
            pre, midCandidate = self.findStrobogrammatic(n-1), oddMidCandidate
        else: 
            pre, midCandidate = self.findStrobogrammatic(n-2), evenMidCandidate
        premid = (n-1)//2
        return [p[:premid] + c + p[premid:] for c in midCandidate for p in pre]
```

