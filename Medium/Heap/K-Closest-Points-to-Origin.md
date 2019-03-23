## Question
We have a list of points on the plane.  Find the K closest points to the origin (0, 0).<br>

(Here, the distance between two points on a plane is the Euclidean distance.)<br>

You may return the answer in any order.  The answer is guaranteed to be unique (except for the order that it is in.)

**Example :**   
<pre>
Input: points = [[1,3],[-2,2]], K = 1
Output: [[-2,2]]
Explanation: 
The distance between (1, 3) and the origin is sqrt(10).
The distance between (-2, 2) and the origin is sqrt(8).
Since sqrt(8) < sqrt(10), (-2, 2) is closer to the origin.
We only want the closest K = 1 points from the origin, so the answer is just [[-2,2]].


Input: points = [[3,3],[5,-1],[-2,4]], K = 2
Output: [[3,3],[-2,4]]
(The answer [[-2,4],[3,3]] would also be accepted.)
</pre>

## Thinking
1. Maintaining the size of the heap helps to save the time for inserting points.<br>
2. Java version has to construct our own comparator.

## Coding
Time: O(nlogK); heap takes logK as we maintain the size of heap to K </br>
Space: O(1)<br>

Python version:
```python
class Solution:
    def kClosest(self, points: List[List[int]], K: int) -> List[List[int]]:
        heap = []
        ans = []
        
        for p in points:
            # use max heap, beacause we want to pop out the bigger ones
            heapq.heappush(heap, (-(p[0]*p[0]+p[1]*p[1]), p))
            

            if len(heap) > K:
                heapq.heappop(heap)
            
        while K > 0:
            ans.append(heapq.heappop(heap)[1])
            K-=1
        return ans
```

Java version:
```java
class Solution {
    public int[][] kClosest(int[][] points, int K) {
        // p1, p2 is used to indicate the comparitor of priorityqueue
        // have to tell how two things in maxheap is compare to each other
        // used to indicate which is larger 
        PriorityQueue<int[]> maxheap = new PriorityQueue<int[]>((p1,p2) -> (p2[0]*p2[0]+p2[1]*p2[1]-p1[0]*p1[0]-p1[1]*p1[1] ));
        for(int[] p: points)
        {
            maxheap.add(p);
            if (maxheap.size()>K)
            {
                maxheap.poll();
            }
        }
        int[][] ans = new int[K][2];
        while(K>0)
        {
            ans[--K] = maxheap.poll();
        }
        
        
        return ans;
    }
}



```
