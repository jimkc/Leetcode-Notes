## Question
Given two binary strings, return their sum (also a binary string).<br>

The input strings are both non-empty and contains only characters 1 or 0.<br>

**Example :**
<pre>
Input: a = "11", b = "1"
Output: "100"
</pre>

**Example :**
<pre>
Input: a = "1010", b = "1011"
Output: "10101"
</pre>


## Thinking
By using padding, we can use the same method to walk through the array.

## Coding
Time: O(n); Go through the array once. </br>
Space: O(n) 
```python3
class Solution(object):
    def addBinary(self, a, b):
        """
        :type a: str
        :type b: str
        :rtype: str
        """
        l1, l2 = list(a), list(b)
        n1 = len(l1) 
        n2 = len(l2)
        
        # pad zero
        if n1 < n2:
            for i in range(n2-n1):
                l1 = ["0"] + l1 
        else:
            for i in range(n1-n2):
                l2 = ["0"] + l2 
                
        # reverse iterate
        plusone = False
        for i in range(len(l2))[::-1]:     
            if l1[i] == l2[i] == "1":
                if plusone: 
                    l1[i] = "1"
                else:
                    l1[i] = "0"
                plusone = True
                if i == 0:
                    l1 = ["1"] + l1
            elif l1[i] == l2[i] == "0":
                if plusone:
                    l1[i] = "1"
                plusone = False
            else:
                if plusone:
                    l1[i] = "0"
                    plusone = True
                    if i == 0:
                        l1 = ["1"] + l1
                else:
                    l1[i] = "1"
                    plusone = False    
                    
        return "".join(l1)
        
        
```

