## Question
Your are given an array of integers prices, for which the i-th element is the price of a given stock on day i; and a non-negative integer fee representing a transaction fee.<br>

You may complete as many transactions as you like, but you need to pay the transaction fee for each transaction. You may not buy more than 1 share of a stock at a time (ie. you must sell the stock share before you buy again.)<br>

Return the maximum profit you can make.

**Example :**   
<pre>
Input: prices = [1, 3, 2, 8, 4, 9], fee = 2
Output: 8
Explanation: The maximum profit can be achieved by:
Buying at prices[0] = 1
Selling at prices[3] = 8
Buying at prices[4] = 4
Selling at prices[5] = 9
The total profit is ((8 - 1) - 2) + ((9 - 4) - 2) = 8.
</pre>

Note:<br>

0 < prices.length <= 50000.<br>
0 < prices[i] < 50000.<br>
0 <= fee < 50000.

## Thinking


## Coding
Time: O(n); <br>
Space: O(n)
```python3
class Solution:
    def maxProfit(self, prices, fee):
        """
        :type prices: List[int]
        :type fee: int
        :rtype: int
        """
        # cash stands for the profit while we have no stock in hand
        # hold stands for the profit while we have one stock in hand
        cash, hold = 0, -prices[0] # Initial case 
        
        for i in range(1,len(prices)):
            cash = max(cash, hold + prices[i] -fee)
            hold = max(hold, cash - prices[i])
        return max(cash,hold)
```
