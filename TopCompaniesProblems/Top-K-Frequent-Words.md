## Question
Given a non-empty list of words, return the k most frequent elements.<br>

Your answer should be sorted by frequency from highest to lowest. If two words have the same frequency, then the word with the lower alphabetical order comes first.

**Example :**   
<pre>
Input: ["i", "love", "leetcode", "i", "love", "coding"], k = 2
Output: ["i", "love"]
Explanation: "i" and "love" are the two most frequent words.
    Note that "i" comes before "love" due to a lower alphabetical order.


Input: ["the", "day", "is", "sunny", "the", "the", "the", "sunny", "is", "is"], k = 4
Output: ["the", "is", "sunny", "day"]
Explanation: "the", "is", "sunny" and "day" are the four most frequent words,
    with the number of occurrence being 4, 3, 2 and 1 respectively.
</pre>

Note:<br>
You may assume k is always valid, 1 ≤ k ≤ number of unique elements.<br>
Input words contain only lowercase letters.<br>
Follow up:<br>
Try to solve it in O(n log k) time and O(n) extra space.

## Thinking


## Coding
Time: O(nlogn); <br>
Space: O(n)
```python3
class Solution:
    def topKFrequent(self, words, k):
        """
        :type words: List[str]
        :type k: int
        :rtype: List[str]
        """
        cnt = collections.Counter(words)
        
        target = cnt.most_common()
        target.sort(key = lambda x: (-x[1], x[0]))
        
        return [x[0] for x in target[:k]]
```

Time: O(NlogK)<br>
```python
import heapq


class Solution:
    def topKFrequent(self, words, k):
        """
        :type words: List[str]
        :type k: int
        :rtype: List[str]
        """
        word_ct = {}

        for word in words:
            word_ct[word] = word_ct.get(word, 0) + 1

        heap = []

        for key in word_ct:
            heapq.heappush(heap, (-word_ct[key], key))

        result = []
        while k > 0:
            result.append(heapq.heappop(heap)[1])
            k -= 1
        return result
```
