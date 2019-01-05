## Question
In a row of trees, the i-th tree produces fruit with type tree[i].</br>

You start at any tree of your choice, then repeatedly perform the following steps:</br>

Add one piece of fruit from this tree to your baskets.  If you cannot, stop.</br>
Move to the next tree to the right of the current tree.  If there is no tree to the right, stop.</br>
Note that you do not have any choice after the initial choice of starting tree: you must perform step 1, then step 2, then back to step 1, then step 2, and so on until you stop.</br>

You have two baskets, and each basket can carry any quantity of fruit, but you want each basket to only carry one type of fruit each.</br>

What is the total amount of fruit you can collect with this procedure?</br>

**Example :**
<pre>
Input: [1,2,1]
Output: 3
Explanation: We can collect [1,2,1].


Input: [0,1,2,2]
Output: 3
Explanation: We can collect [1,2,2].
If we started at the first tree, we would only collect [0, 1].


Input: [1,2,3,2,2]
Output: 4
Explanation: We can collect [2,3,2,2].
If we started at the first tree, we would only collect [1, 2].


Input: [3,3,3,1,2,1,1,2,3,3,4]
Output: 5
Explanation: We can collect [1,2,1,1,2].
If we started at the first tree or the eighth tree, we would only collect 4 fruits.
</pre>


## Thinking
Aproach 1 in the solution.

## Coding
Time: O(n);  </br>
Space: O(n) 
```python3
class Solution:
    def totalFruit(self, tree):
        """
        :type tree: List[int]
        :rtype: int
        """
        
        cnted_tree = [] # store items as [(item, consecutive nums)]
        
        cnt = 1
        prev = tree[0]
        
        # create the list cnted_tree
        for i in range(1,len(tree)):
            if tree[i] == prev:
                cnt += 1
            else:
                cnted_tree.append((prev,cnt))
                prev = tree[i]
                cnt = 1
        cnted_tree.append((prev,cnt))
        

        if len(cnted_tree) < 3:
            return sum([x[1] for x in cnted_tree])
        
        # types is used to record and make sure we are only having at most two types of items
        types = set([cnted_tree[0][0],cnted_tree[1][0]])
        ans = 0
        cur_len = cnted_tree[0][1]+cnted_tree[1][1]
        for i in range(2,len(cnted_tree)):
            if cnted_tree[i][0] in types:
                cur_len += cnted_tree[i][1]
            else:
                ans = max(ans,cur_len)
                types = set([cnted_tree[i-1][0], cnted_tree[i][0]])
                cur_len = cnted_tree[i-1][1]+cnted_tree[i][1]
        ans = max(ans,cur_len)
        return ans
            
```

