## Question
Alice plays the following game, loosely based on the card game "21".<br>

Alice starts with 0 points, and draws numbers while she has less than K points.  During each draw, she gains an integer number of points randomly from the range [1, W], where W is an integer.  Each draw is independent and the outcomes have equal probabilities.<br>

Alice stops drawing numbers when she gets K or more points.  What is the probability that she has N or less points?


**Example :**   
<pre>
Input: N = 10, K = 1, W = 10
Output: 1.00000
Explanation:  Alice gets a single card, then stops.

Input: N = 6, K = 1, W = 10
Output: 0.60000
Explanation:  Alice gets a single card, then stops.
In 6 out of W = 10 possibilities, she is at or below N = 6 points.

Input: N = 21, K = 17, W = 10
Output: 0.73278
</pre>

## Thinking
Every step of dp can be break down to two probabilities to add together ex: pb(i)=pb(i-w) x pb(w) = dp[i-w] x 1/w

## Coding
Time: O(n);  </br>
Space: O()
```python
class Solution:
    def new21Game(self, N: int, K: int, W: int) -> float:
        # K is the threshold to stop getting numbers from W
        # N is the threshold we want to target (if exceed then count)
        # 1~W is the number we can add to our current number each time
        
        # if K==0, 100% the number is smaller than N
        # if N >= K+W, then 100% we will stop at a number that is <= N
        if K == 0 or N >= K+W:
            return 1
        
        # dp is the probability we can get point i
        # we do nothing to get 0 it is initialized , so d[0] = 1
        dp = [1.0]+[0]*N
        
        # dp[i] = dp[i-W]*1/W + dp[i-W-1]*1/W +....+ dp[i-1]*1/W
        # Because if we get number i-W than we can pick its complement W with probability 1/W to get sum i
        # let dp[i-W]+....dp[i-1] = wsum, the sum of i to at most previous w probabilities
        wsum = 1 # start with i=1, wsum = dp[0]
        for i in range(1,len(dp)):
            dp[i] = wsum/W
            # if the number < K and in window W , there is a chance it can add 1 number to exceed K
            # if it exceeds K, it means the game ends, we wont get the next number to add with it, wsum is for the next round
            if i < K:
                wsum += dp[i]
            # to maintain wsum = dp[i-w]+...dp[i-1]
            if i-W >= 0:
                wsum -= dp[i-W] # this is dp[i-W-1] for next round
            
        return sum(dp[K:])
```

