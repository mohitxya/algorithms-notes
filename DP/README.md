## Table of Contents
- [Introduction](#introduction)
- [Climbing Stairs](#climbing-stairs)
- [Frog Jump](#frog-jump)
- [House Robber I](#house-robber-I)
- [House Robber II](#house-robber-II)
- [Ninja's Training](#ninja's-training)



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
- Simple method:
     - Try representing as index.
     - Do all possible things.
     - Sum, max, min depending on the problem.
#### Climbing Stairs
```cpp
int stair(int ind)
{
	if(ind==0) return 1;
	if(ind==1) return 1;
	int left=stair(ind-1);
	int right=stair(ind-2);
	return left+right;
}
```

other option:
```cpp
int climbStairs(int n) {
        if(n<=2) return n;
        int prev=2, prev2=1, curr;
        for(int i=3; i<=n; i++)
        {
            curr=prev+prev2;
            prev2=prev;
            prev=curr;
        }
        return prev;
    }
```

#### Frog Jump
- Greedy solution does not work.
- `Memoization` solution: Top-down
- ![](attachments/Pasted%20image%2020250713191640.png)
-  Recurrence -> DP
```pseudo-code
f(ind)
{
	if(ind==0) return 0;
	if(dp[ind]!=-1) return dp[ind];
	left=f(ind-1)+abs(a[ind]-a[ind-1]);
	if(ind>1){right=f(ind-2)+abs(a[ind]-a[ind-2])};
	dp[ind]=min(left,right);
	return min(left,right);
}
```
- Time complexity: $O(n)$
- Space complexity: array size + stack space = $O(n)+O(n)$
- Tabulation solution: Bottom-Up
- initialize `dp array` with n 0s
```pseudo-code
int dp[n]={0,0,..}
dp[0]=0
for(i=1 to n-1)
{
	fs=dp[ind-1]+abs(a[ind]-a[ind-1])
	if(i>1) ss=dp[ind-2]+ abs(a[ind]-a[ind-2]);
	dp[ind]=min(fs,ss); 
}
return dp[n-1]
```

- space optimization: I just need `prev` and `prev2`.
```pseudo-code
cuuri=min(fs,ss);
prev2=prev;
prev=curri;

return prev
```
##### For k stairs at a time:
- The frog can jump from `ith` to any step in `range[i+1,i+k]`
```cpp
class Solution {
private:
    int jump(int index, vector<int>& heights, vector<int>& dp, int k)
    {
        if(index == 0) return 0;
        if(dp[index] != -1) return dp[index];
        
        vector<int> temp;
        for(int i = 1; i <= k; i++)
        {
            if(index >= i)
            {
                int var = jump(index - i, heights, dp, k) + abs(heights[index] - heights[index - i]);
                temp.push_back(var);
            }    
        }
        
        dp[index] = *min_element(temp.begin(), temp.end());
        return dp[index];
    }
public:
    int frogJump(vector<int>& heights, int k) {
        int n = heights.size();
        vector<int> dp(n, -1);

        return jump(n - 1, heights, dp, k);
    }
};

```
- min of a vector in `cpp: min_element(.begin(),.end())`

#### House Robber I
-  return _the maximum amount of money you can rob tonight **without alerting the police**_
- caveat being, robbing two adjacent houses alerts the police.
- So basically, maximum sum of non-adjacent elements.
- Pick subsequence with no adjacent element.
```cpp
f(ind)
{
	
	if(ind==0) return arr[ind];
	if(ind<0) return 0;
	if(dp[ind] !=-1) return dp[ind];

	pick=arr[ind] + f(ind-2);
	notpick=0+f(ind-1);

	dp[ind]=max(pick,notpick)
	return dp[ind]
}
```
- We got the recursive solution -> convert to DP
- We create a `dp[n]` array initialized to -1.
- if answer is already in the array return the value from the array.
- Tabulation approach:
```cpp
dp[0]=arr[0];
// if only on element exists dp[0] would take it.
for(int i=1; i<n; i++)
{
	int pick=arr[i];
	if(i>1) pick+=dp[i-2];
	int nonPick=dp[i-1];
	dp[i]=max(pick,nonPick);
}

return dp[n-1];
```
- Space optimization: we only need three variables.
- `prev, prev2, cur_i`
```cpp
prev=arr[0]
prev2=0

at each iteration
	pick+=prev2;
	nonpick=prev;
	cur_i=max(pick,nonpick)

	prev2=prev;
	prev=cur_i;
```

#### House Robber II
- Same question but this time the houses are organized in a circular manner.
- If we consider the last element we can't consider the first element.
- Make two reduced arrays: `arr1(arr-last element)` and `arr2(arr-first element)`, apply same function on both and return the max.
#### Ninja's Training
- Two activities can't be done on consecutive days.
- Greedy approach doesn't work. 
- approach: try all possible ways.
- Treat days as indices. 
- We need a way to store the last task. 
```
0: Task 1
1: Task 2
2: Task 3
3: No task performed
```
- Example: `f(2,1)` means max merit points for indexes from 0 to 2 given that task performed at 3 was '1'.
- recursive pseudo-code:
```
f(day,last)
{
	if(day==0){
		maxi=0
		for(int i=0; i<=2; i++)
		{
			if(i!=last)
			{
				maxi=max(maxi,points[0][i])
			}
		}
		// in the base case simply choose the activity which gives you the max score.
		return maxi
	}
	maxi=0
	for(int i=0; i<=2; i++)
	{
		if(i!=last)
		{
			activity=points[day][i]+f(day-1,i)
			maxi=max(maxi,activity)
		}
	}
	return maxi
}

```
- N x 4 size array. 
- Now, using `Memoization`: