## Question
You are playing the following Bulls and Cows game with your friend: You write down a number and ask your friend to guess what the number is. Each time your friend makes a guess, you provide a hint that indicates how many digits in said guess match your secret number exactly in both digit and position (called "bulls") and how many digits match the secret number but locate in the wrong position (called "cows"). Your friend will use successive guesses and hints to eventually derive the secret number.<br>

Write a function to return a hint according to the secret number and friend's guess, use A to indicate the bulls and B to indicate the cows.<br> 

Please note that both secret number and friend's guess may contain duplicate digits.
**Example :**   
<pre>
Input: secret = "1807", guess = "7810"

Output: "1A3B"

Explanation: 1 bull and 3 cows. The bull is 8, the cows are 0, 1 and 7.



Input: secret = "1123", guess = "0111"

Output: "1A1B"

Explanation: The 1st 1 in friend's guess is a bull, the 2nd or 3rd 1 is a cow.
</pre>

## Thinking
1. Count the exactly same indexes for A.<br>
2. Record the rest in dictionaries then check both dictionaries to count B.

## Coding
Time: O(n);  </br>
Space: O(n)
```python
class Solution:
    def getHint(self, secret: str, guess: str) -> str:
        # dic to record the number of items that is not matched exactly in secret and guess
        secret_index = {}
        guess_index = {}
        A = 0
        B = 0
        
        for i in range(len(secret)):
            if secret[i] == guess[i]:
                A+=1
            else:
                if secret[i] in secret_index:
                    secret_index[secret[i]] += 1
                else: 
                    secret_index[secret[i]] = 1
                    
                if guess[i] in guess_index:
                    guess_index[guess[i]] += 1
                else: 
                    guess_index[guess[i]] = 1
            
        
        for val, num in secret_index.items():
            if val in guess_index.keys():
                num_guess = guess_index[val]
                B+=min(num, num_guess)
        
        

        
        
        

        return "{0}A{1}B".format(A,B)
        
```

