## Question
Given an array nums of n integers, are there elements a, b, c in nums such that a + b + c = 0? Find all unique triplets in the array which gives the sum of zero.<br>

Note:<br>

The solution set must not contain duplicate triplets.

**Example :**   
<pre>
Given array nums = [-1, 0, 1, 2, -1, -4],

A solution set is:
[
  [-1, 0, 1],
  [-1, -1, 2]
]
</pre>

## Thinking
1.This question is similar to 3Sum-Closet, but implemented in  a different way.<br>
2. We use i to iterate through array, and j to iterate from i+1 to last, also we record nums[i] in a set to know what kind of 
numbers we have walked through before, if -(nums[i]+nums[j]) is in seen, then we put it in the answer.<br>
3. Second method more concise and safe space and time with similar concept.

## Coding
Time: O(n^2);<br>
Space: O(n^2)<br>

python3 version:
```python
class Solution:
    def threeSum(self, nums):
        """
        :type nums: List[int]
        :rtype: List[List[int]]
        """
        
        
        ans = set()
        nums.sort()
        
        for i in range(len(nums[:-2])): # In case there are list smaller than length 3
            v = nums[i]
            seen = set()
            
            if i >= 1 and v >= 0 and v == nums[i-1]: # Cover the cases of [0,0....] also beacause it is sorted it wont appear [a,a,-2a]
                continue
            
            for j in range(i+1,len(nums)):
                x = nums[j]
                
                if -v-x in seen:
                    #print (v,x,seen)
                    ans.add((v,-v-x,x)) # Non duplicate because the array is sorted first
                if x not in seen:
                    seen.add(x)
                
        
        
        return  (list(map(list,ans)))
```

java version:<br>
```java
class Solution {
    public List<List<Integer>> threeSum(int[] numbers) {
        
        HashSet<Integer> seen = new HashSet<>();
        Arrays.sort(numbers);
        List<List<Integer>> ans = new ArrayList<>();
        
        for(int i=0; i+2<numbers.length; i++){
            //skip the same result
            if (i>0 && numbers[i] == numbers[i-1]){
                continue;
            }
            
            int target = -numbers[i];
            int j = i+1;
            int k = numbers.length-1;
            
            while(j<k){
                if (numbers[j]+numbers[k] == target){
                    ans.add(Arrays.asList(numbers[i], numbers[j], numbers[k]));
                    j++;
                    k--;
                    //avoid adding same results
                    while(j<k && numbers[j]==numbers[j-1]){j++;}
                    while(j<k && numbers[k]==numbers[k+1]){k--;}
                }
                else if((numbers[j]+numbers[k])> target){
                    k--;
                }
                else{
                    j++;
                }
                
                
            }
                
        }
        return ans;
    }
}
```
