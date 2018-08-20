## Question
Given a string containing only digits, restore it by returning all possible valid IP address combinations.

**Example :**   
<pre>
Input: "25525511135"
Output: ["255.255.11.135", "255.255.111.35"]
</pre>

## Thinking


## Coding
Time: O(n);<br>
Space: O(n)
```python3
class Solution:
    def restoreIpAddresses(self, s):
        """
        :type s: str
        :rtype: List[str]
        """
        ans = []
        self.permute(s, '', 0, ans)
        return ans
        
    # string: string left to handle
    # ipdone: string that is already checked valid for ip pos
    # ipnum: nums of ip pos done
    def permute(self, string, ipdone, ipnum, ans):
        # Check valid for ip
        if ipnum == 4 and string == '':
            ans.append(ipdone[1:])
        
        for i in range(len(string)):
            s = string[:i+1]
            
            if int(s) <= 255:
                if len(s)>1 and s[0] == '0': # Prevent case like '010'
                    continue
                elif len(string)-i > (4-ipnum)*3: # Check if the left digits wont be too much for ip
                    continue
                self.permute(string[i+1:], ipdone + '.' + string[:i+1], ipnum+1,ans)        
            else:
                break```

