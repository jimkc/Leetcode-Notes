## Question
Given a collection of intervals, merge all overlapping intervals.

**Example :**   
<pre>
Input: [[1,3],[2,6],[8,10],[15,18]]
Output: [[1,6],[8,10],[15,18]]
Explanation: Since intervals [1,3] and [2,6] overlaps, merge them into [1,6].

Input: [[1,4],[4,5]]
Output: [[1,5]]
Explanation: Intervals [1,4] and [4,5] are considerred overlapping.
</pre>

## Thinking

## Coding
Time: O(nlogn); The sorting aldorithm of python is **Timsort** which is average O(nlogn), and the adding process goes through the whole array once. </br>
Space: O(n)
```python3
# Definition for an interval.
# class Interval:
#     def __init__(self, s=0, e=0):
#         self.start = s
#         self.end = e

class Solution:
    def merge(self, intervals):
        """
        :type intervals: List[Interval]
        :rtype: List[Interval]
        """
        
        intervals.sort(key =lambda x:x.start)
        
        #print ([(item.start,item.end) for item in intervals])
        merge = []
        
        for inter in intervals:
            if not merge or merge[-1].end < inter.start:
                merge.append(inter)
            else:
                merge[-1].end = max(merge[-1].end,inter.end)
        return merge
```

