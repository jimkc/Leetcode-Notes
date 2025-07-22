### Link
https://www.1point3acres.com/bbs/thread-1136599-1-1.html

### Question 
然后设计一个1D Sparse array data structure, 来set，get。然后follow up一个dot product function. 然后再follow up一个2D Sparse matrix, 然后再follow up 一个matrix multiplication。

Follow up 的 2D matix 跟 https://leetcode.com/problems/sparse-matrix-multiplication/description/ 有點像

### Solution 
2D matrix 參考 leetcode 311.

作法是直接做matrix calculation，但是不要每個都做因為很多是0，可以參考那提leetcode先判斷0才做dot 

面試官可能會問為什麼不用hashmap，因為sparse array 可能很大不適合讀進來memory