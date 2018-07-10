## Question
In the following, every capital letter represents some hexadecimal digit from 0 to f.<br>

The red-green-blue color "#AABBCC" can be written as "#ABC" in shorthand.  For example, "#15c" is shorthand for the color "#1155cc".<br>

Now, say the similarity between two colors "#ABCDEF" and "#UVWXYZ" is -(AB - UV)^2 - (CD - WX)^2 - (EF - YZ)^2.<br>

Given the color "#ABCDEF", return a 7 character color that is most similar to #ABCDEF, and has a shorthand (that is, it can be represented as some "#XYZ"<br>

**Example :**
<pre>
Input: color = "#09f166"
Output: "#11ee66"
Explanation:  
The similarity is -(0x09 - 0x11)^2 -(0xf1 - 0xee)^2 - (0x66 - 0x66)^2 = -64 -9 -0 = -73.
This is the highest among any shorthand color.
</pre>

Note:

color is a string of length 7.
color is a valid RGB color: for i > 0, color[i] is a hexadecimal digit from 0 to f
Any answer which has the same (highest) similarity as the best answer will be accepted.
All inputs and outputs should use lowercase letters, and the output is 7 characters.

## Thinking


## Coding
Time: O(n); </br>
Space: O(1) 
```python3
class Solution:
    def similarRGB(self, color):
        """
        :type color: str
        :rtype: str
        """
        
        
        def getClosest(s):
            return min(['00', '11', '22', '33', '44', '55', '66', '77', '88', '99', 'aa', 'bb', 'cc', 'dd', 'ee', 'ff'],
                       key = lambda x:abs(int(s,16) - int(x,16)))
        
        most_sim = [getClosest(color[i:i+2]) for i in range(1,len(color),2)]
        return '#'+''.join(most_sim)
```
