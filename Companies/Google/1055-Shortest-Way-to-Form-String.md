## Question
From any string, we can form a subsequence of that string by deleting some number of characters (possibly no deletions).<br>

Given two strings source and target, return the minimum number of subsequences of source such that their concatenation equals target. If the task is impossible, return -1.<br>

**Example :**   
<pre>
Input: source = "abc", target = "abcbc"
Output: 2
Explanation: The target "abcbc" can be formed by "abc" and "bc", which are subsequences of source "abc".

Input: source = "abc", target = "acdbc"
Output: -1
Explanation: The target string cannot be constructed from the subsequences of source string due to the character "d" in target string.

Input: source = "xyz", target = "xzyxz"
Output: 3
Explanation: The target string can be constructed as follows "xz" + "y" + "xz".
</pre>

Constraints:<br>

Both the source and target strings consist of only lowercase English letters from "a"-"z".<br>
The lengths of source and target string are between 1 and 1000.

## Thinking
<pre>
This is a google phone screen question. It's the same idea as 792. Number of Matching Subsequences. I recommend solving that question first.

The idea is to create an inverted index that saves the offsets of where each character occurs in source. The index data structure is represented as a hashmap, where the Key is the character, and the Value is the (sorted) list of offsets where this character appears. To run the algorithm, for each character in target, use the index to get the list of possible offsets for this character. Then search this list for next offset which appears after the offset of the previous character. We can use binary search to efficiently search for the next offset in our index.

Example with source = "abcab", target = "aabbaac"
The inverted index data structure for this example would be:
inverted_index = {
a: [0, 3] # 'a' appears at index 0, 3 in source
b: [1, 4], # 'b' appears at index 1, 4 in source
c: [2], # 'c' appears at index 2 in source
}
Initialize i = -1 (i represents the smallest valid next offset) and loop_cnt = 1 (number of passes through source).
Iterate through the target string "aabbaac"
a => get the offsets of character 'a' which is [0, 3]. Set i to 1.
a => get the offsets of character 'a' which is [0, 3]. Set i to 4.
b => get the offsets of character 'b' which is [1, 4]. Set i to 5.
b => get the offsets of character 'b' which is [1, 4]. Increment loop_cnt to 2, and Set i to 2.
a => get the offsets of character 'a' which is [0, 3]. Set i to 4.
a => get the offsets of character 'a' which is [0, 3]. Increment loop_cnt to 3, and Set i to 1.
c => get the offsets of character 'c' which is [2]. Set i to 3.
We're done iterating through target so return the number of loops (3).

The runtime is O(M) to build the index, and O(logM) for each query. There are N queries, so the total runtime is O(M + N*logM). M is the length of source and N is the length of target. The space complexity is O(M), which is the space needed to store the index.
</pre>

## Coding
Time: O();  </br>
Space: O()
```python
class Solution:
    def shortestWay(self, source: str, target: str) -> int:
        
        appear_index = collections.defaultdict(list)
        # record all the index that c appeared
        for i,c in enumerate(source):
            appear_index[c].append(i)
            
        loop_count = 1
        # the minimal index we can pick letter from the source
        i = -1
        
        for c in target:
            if c not in appear_index:
                return -1
            
            c_indices_in_appear_index = appear_index[c]
            
            # bisect_left(A, i) returns the smallest index j s.t. A[j] >= i. If no such index j exists, it returns len(A).
            # j is the index in appear_index, appear_index[c][j] is an index of source
            j = bisect.bisect_left(c_indices_in_appear_index, i)
            
            # i is larger than any value in c_indices_in_appear_index
            if j == len(c_indices_in_appear_index):
                loop_count+=1
                i = c_indices_in_appear_index[0] + 1
            else:
                i = c_indices_in_appear_index[j] +1
                
        return loop_count
```

