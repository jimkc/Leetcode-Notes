## Question
Given a string, find the length of the longest substring without repeating characters.

**Example :**   
<pre>
Given "abcabcbb", the answer is "abc", which the length is 3.

Given "bbbbb", the answer is "b", with the length of 1.

Given "pwwkew", the answer is "wke", with the length of 3. Note that the answer must be a substring, "pwke" is a subsequence and not a substring.
</pre>

## Thinking

## Coding
Time: O(n);<br>
Space: O(n)<br>

python3 version
```python
class Solution:
    def lengthOfLongestSubstring(self, s):
        """
        :type s: str
        :rtype: int
        """
        
        if s=='':
            return 0
        
        dic={}
        start=0 # start index of the current watching sequence
        max_length=0
        for index,char in enumerate(s):
            if char not in dic:
                dic[char]=index
            else:
                # While we found c that has appeared before
                if dic[char]>=start: # Means that we found duplicate in the watching sequence
                    max_length=max(index-start,max_length) 
                    start=dic[char]+1  # Update to see a new sequence , add 1 because char has appeared twice
                dic[char]=index
                
        return max(max_length,len(s)-1-start+1)
```




java version:
```java
class Solution {
    public int lengthOfLongestSubstring(String s) {
        int n = s.length();
        int ans = 0;
        Map<Character, Integer> indexMap = new HashMap<>(); // current index
        
        for(int i=0, j=0; j<n; j++)
        {
            char c = s.charAt(j);
            if(indexMap.containsKey(c))
            {
                // the dupicate of c is in the sliding window
                if(i <= indexMap.get(c))
                {
                    i = indexMap.get(c)+1;
                }
            }
            
            ans = Math.max(ans, j-i+1);
            indexMap.put(c, j);
        }
        return ans;
    }
}
```
