## Question
Suppose you have a random list of people standing in a queue. Each person is described by a pair of integers (h, k), where h is the height of the person and k is the number of people in front of this person who have a height greater than or equal to h. Write an algorithm to reconstruct the queue.<br>

Note:<br>
The number of people is less than 1,100.

**Example :**   
<pre>
Input:
[[7,0], [4,4], [7,1], [5,0], [6,1], [5,2]]

Output:
[[5,0], [7,0], [5,2], [6,1], [4,4], [7,1]]
</pre>

## Thinking
1st element: We insert [7,0] at 0 in ordered_line: [[7,0]]<br>
2nd element: We insert [7,1] at 1 in ordered_line: [[7,0], [7,1]]<br>
3rd element: We insert [6,1] at 1 in ordered_line: [[7,0], [6,1], [7,1]]<br>
(Notice how it moved all elements at index 1 or greater to the right 1)<br>
4th element: We insert [5,0] at 0 in ordered_line: [[5,0], [7,0], [6,1], [7,1]]<br>
(Notice how, although we have inserted another element in front of [6,1], because of the order we are doing it it, the new element is not taller or the same height so it doesn't matter)<br>
5th element: We insert [5,2] at 2 in ordered_line: [[5,0], [7,0], [5,2], [6,1], [7,1]]<br>
6th element: We insert [4,4] at 4 in ordered_line: [[5,0], [7,0], [5,2], [6,1], [4,4], [7,1]]<br>


## Coding
Time: O(nlogn);<br>
Space: O(n)
```python3
class Solution:
    def reconstructQueue(self, people):
        """
        :type people: List[List[int]]
        :rtype: List[List[int]]
        """
        if not people:
            return []
        ans = []
        people.sort(key = lambda x: (-x[0],x[1]) ) # Height from big to small, number of person from small to big
        
        for p in people:
            ans.insert(p[1], p) 
        
        return ans
```

