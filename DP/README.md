## Table of Contents
- [Introduction](#introduction)



#### Introduction
- Two way: `Tabulation` and `Memoization`.
-  "overlapping subproblems" issue: Recursion but, we store value of subproblems in some map/table.
- `[-1,-1,-1,-1,-1]`, store at appropriate index. e.g. f(2)=2 then `[-1,-1,2,-1,-1]`.
- `Memoization` Steps:
	- Declare an array.
	- Store answers computed for subproblems.
	- Add a check step to see if it has already been computed.
- `memset(dp,-1, sizeof(dp))` or `vector<int> dp{n+1, -1}`
-  Time complexity: $O(n)$
- Space complexity: $O(n)$
- Tabulation: Base case to required answer. Bottom up.
	- For our `fibonacci series` example:
	- `dp[0]=0` and `dp[1]=1` (array or vector of size n+1) 
	-  for 2 onwards write a for loop: each iteration `dp[i]=dp[i-1]+dp[i-2]`
		- Time complexity: $O(n)$ and Space complexity: $O(1)$
	-  To optimize space: just use two variables `prev` & `prev2`
	- At the end the answer will be saved in `prev`.
		-  Time complexity: $O(n)$ and Space complexity: $O(1)$
```cpp
int prev2=0;
int prev=1;
for(int i=2; i<=n; i++)
{
	int curr=prev+prev2;
	prev2=prev;
	prev=curr;
}
cout<<prev;
```