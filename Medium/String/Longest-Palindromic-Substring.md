## Question
Given a string s, find the longest palindromic substring in s. You may assume that the maximum length of s is 1000.

**Example :**   
<pre>
Input: "babad"
Output: "bab"
Note: "aba" is also a valid answer.

Input: "cbbd"
Output: "bb"
</pre>

## Thinking


## Coding
Time: O(n^2) Compareing the words takes O(n); <br>
Space: O(1)<br>

python3 version:
```python
class Solution:
    def longestPalindrome(self, s):
        """
        :type s: str
        :rtype: str
        """

        start_point = 0
        max_len = 1

        for i in range(len(s)):
            if i-max_len > 0 and s[i-max_len-1:i+1] == s[i-max_len-1:i+1][::-1]: # [ +, max_length, (i) ] add 2 to max_length
                start_point = i - max_len - 1
                max_len += 2

                
            if i-max_len >= 0 and s[i-max_len:i+1] == s[i-max_len:i+1][::-1]: # [ max_length, (i)]
                start_point = i - max_len
                max_len += 1
                

        return s[start_point : start_point + max_len]
```

java version:
```java
class Solution {
    public String longestPalindrome(String s) {
        int[] range  = new int[]{0,0};

        if (s.length() == 0){
            return "";
        }

        for(int i=0; i<s.length(); i++){
            this.extendPalindrome(s, i, i, range);
            this.extendPalindrome(s, i, i+1, range);
        }
        return s.substring(range[0], range[1]+1);
    }

    public void extendPalindrome(String s, int l, int r, int[] range){

        while(l>=0 && r <= s.length()-1 && s.charAt(l)==s.charAt(r)){
            l--;
            r++;
        }
        l++;
        r--;

        if(r-l+1 > range[1]-range[0]+1){
            range[0] = l;
            range[1] = r;

        }
    }
}
```

java version2:
```java
class Solution {
    public String longestPalindrome(String s) {
        if (s==null || s.length() < 1)
        {
            return "";
        }
        
        int start=0, end=0;
        for (int i = 0; i < s.length(); i++)
        {
            int len1 = expandAroundCenter(s, i, i);
            int len2 = expandAroundCenter(s, i, i+1);
            int len = Math.max(len1, len2);
            if (len > end -start+1)
            {
                // i is the left center if there are two center indices
                start = i - (len - 1) / 2;
                end = i + len / 2;
            }
        }
        return s.substring(start, end + 1);
    }
    
    private int expandAroundCenter(String s, int left, int right)
    {
        int L = left, R = right;
        while (L >= 0 && R < s.length()&& s.charAt(L) == s.charAt(R))
        {
            L--;
            R++;
        }
        return R-L-1;
    }
        
}
```
