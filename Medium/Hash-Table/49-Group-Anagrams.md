## Question
Given an array of strings strs, group the anagrams together. You can return the answer in any order.<br>

An Anagram is a word or phrase formed by rearranging the letters of a different word or phrase, typically using all the original letters exactly once.<br>

**Example 1:**   
<pre>
Input: strs = ["eat","tea","tan","ate","nat","bat"]
Output: [["bat"],["nat","tan"],["ate","eat","tea"]]
</pre>

**Example 2:**   
<pre>
Input: strs = [""]
Output: [[""]]
</pre>

**Example 2:**   
<pre>
Input: strs = ["a"]
Output: [["a"]]
</pre>


##Intuition

Two strings are anagrams if and only if their character counts (respective number of occurrences of each character) are the same.<br>

##Algorithm

We can transform each string s into a character count, consisting of 26 non-negative integers representing the number of a's, b's, c's, etc. We use these counts as the basis for our hash map.<br>

In Java, the hashable representation of our count will be a string delimited with '#' characters. For example, abbccc will be #1#2#3#0#0#0...#0 where there are 26 entries total. In python, the representation will be a tuple of the counts. For example, abbccc will be (1, 2, 3, 0, 0, ..., 0), where again there are 26 entries total.<br>


## Coding
Time: Time Complexity: O(NK), where N is the length of strs, and K is the maximum length of a string in strs. Counting each string is linear in the size of the string, and we count every string.
Space: O(NK), the total information content stored in ans.<br>

```java
class Solution {
    public List<List<String>> groupAnagrams(String[] strs) {
        
        final Map<String, List> ans = new HashMap<String, List>();
        final int[] count = new int[26];
        
        for (final String s: strs)
        {
            Arrays.fill(count, 0);
            for (final char c: s.toCharArray())
            {
                count[c -'a']+=1;
            }
            
            final StringBuilder sb = new StringBuilder("");
            for (int i=0; i<26; i++)
            {
                sb.append("#");
                sb.append(count[i]);
            }
            
            
            final String key = sb.toString();
            if(!ans.containsKey(key))
            {
                ans.put(key, new ArrayList());
            }
            ans.get(key).add(s);
        }
        return new ArrayList(ans.values());
    }
}
```

