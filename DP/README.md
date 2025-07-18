## Table of Contents
- [Introduction](#introduction)
- 1D problems
	- [Climbing Stairs](#climbing-stairs)
	- [Frog Jump](#frog-jump)
	- [House Robber I](#house-robber-I)
	- [House Robber II](#house-robber-II)
- 2D/3D problems
	- [Ninja's Training](#ninja's-training)
	- [Grid Unique Paths](#grid-unique-paths)
	- [Minimum Path Sum](#minimum-path-sum)
	- [Minimum Falling Path](#minimum-falling-path)
	- [Ninja and his friends](#ninja-and-his-friends)
- DP on subsequences
	 - [Subset Sum](#subset-sum) 
	 - [Partition Equal Subset Sum](#partition-equal-subset-sum)
	 - [Minimize Sum Difference](#minimize-sum-difference)
	 - [Count Partitions with difference](#count-partitions-with-difference)
	 - [Minimum coins](#minimum-coins)



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
```cpp
int fn(int day, int last, vector<vector<int>> &points, vector<vector<int>> &dp)
{
    if(dp[day][last]!=-1) return dp[day][last];
    if(day==0)
    {
        int maxi=0;
        for(int i=0; i<=2; i++)
        {
            if(i!=last)
            {
                maxi=max(maxi,points[0][i]);
            }
        }
        return maxi;
    }
    int maxi=0;
    for(int i=0; i<=2; i++)
    {
        if(i!=last)
        {
            int activity=points[day][i] + fn(day-1,i,points,dp);
            maxi=max(maxi,activity);
        }
    }
    return dp[day][last]=maxi;
}

  
int ninjaTraining(int n, vector<vector<int>> &points)
{
    vector<vector<int>> dp(n,vector<int>(4,-1));
    int ans=fn(n-1,3,points,dp);
    return ans;
}
```
- Tabulation method: 
```pseudo-code
dp[0][0]=max(arr[0][1], arr[0][2])
dp[0][1]=max(arr[0][0],arr[0][2])
dp[0][2]=max(arr[0][0],arr[0][1])
dp[0][3]=max(arr[0][0..through..2])

for(day=1 to n-1)
{
	for(last 0 through 3)
	{
		dp[day][last]=0
		for(task 0 through 3)
		{
			if(task!=last)
			{
				int point = points[day][task]+dp[day-1][task]
				dp[day][last]=max(dp[day][last], point)
			}
		}
		
	}
}

return dp[n-1][3];

```
- Time complexity: $O(N*3*4)$
- Space complexity: $O(N*4)$
- space optimization: 
- we just need a vector and a dummy vector instead of a vector of vectors. Basically a size 4 vector or array.
- replace day-1 with `vector<int> prev`.

#### Grid Unique Paths
```
if(i==0 && j==0) return 1
if(i<0 || j<0) return 0
if(dp[i][j]!=-1) return dp[i][j];

up=f(i-1,j)
left=f(i,j-1)

return dp[i][j]=up+left
```
- convert this to a DP solution. 
- Time complexity: for each block we take  up or left.
- so, $2^{m*n}$ paths
- Space complexity: recursive stack space: path length.   
- Time complexity: $O(n)$
- Space complexity: $O((n-1)+(m-1))+O(n*m)$
- Tabulation:
- base condition: `dp[0][0]=1`
```
for(i=0 to m-1)
	for(j=0 to n-1)
		if(i=0 && j==0) dp[0][0]=1
		else
			up=0, left=0
			if(i>0) up=dp[i-1][j]
			if(j>0)left=dp[i][j-1]
			dp[i][j]=up+left 
```
- Time complexity: $O(N*M)$
- Space complexity: $O(N*M)$
- Space optimization: We are just using previous row and column.
- Dry run of tabulation yourself 

#### Minimum Path Sum
- Minimizes the sum of cost of all numbers along the path.
- Greedy method won't work. No Uniformity.
- `right,down` becomes `left,up`
```
f(i,j)
	if i and j equals 0, return a[0][0] //value of that block
	if(i<0 or j<0) return INT_MAX;
	if(dp[i][j]!=-1) return dp_i_j
	up=a[i][j]+f(i-1,j)
	left=a[i][j]+f(i,j-1)
	
	return dp_i_j=min(up,left)
```
- Time complexity: $O(n*m)$
- Space complexity: $O(n*m)+O(m+n)$
- Tabulation:  
```
for i 0 to n-1
	for j 0 to m-1
		if(i and j = 0 ) dp[i][j]=a[0][0];
		else
			i>0:up=a_i_j+dp[i-1][j]
			j>0:left=a_i_j+dp[i][j-1]
			dp[i][j]=min(up,left)
			
```
#### Minimum Falling Path
```cpp
class Solution {
private:
    int falling(int i, int j, vector<vector<int>>& a, vector<vector<int>>& dp)
    {
        if(i==0 && (j>=0 && j<a[0].size())) return a[i][j];
        if(i<0 || (j<0 || j>=a.size())) return 1e9;
        if(dp[i][j]!=-1) return dp[i][j];
        int one=a[i][j]+falling(i-1,j-1,a, dp);
        int two=a[i][j]+falling(i-1,j,a, dp);
        int three=a[i][j]+falling(i-1,j+1,a, dp);
        return dp[i][j]=min({one,two,three});
    }
public:
    int minFallingPathSum(vector<vector<int>>& matrix) {
        int m=matrix.size();
        int n=matrix[0].size();
        int mini = 1e9;
        vector<vector<int>> dp(m,vector<int>(n,-1));
        for(int j=0; j < matrix[0].size(); j++) {
            mini = min(mini, falling(m-1, j, matrix, dp)); // start from first row
        }
        return mini;
    }
};
```
- The `memoization` solution isn't fast enough. 
- Tabulation solution:
```cpp
class Solution {
public:
    int minFallingPathSum(vector<vector<int>>& matrix) {
        int m = matrix.size();
        int n = matrix[0].size();
        vector<int> dp(n,-1), curr(n,-1);
        for(int j = 0; j < n; j++) {
            dp[j] = matrix[0][j];
        }
        for(int i = 1; i < m; i++) {
            for(int j = 0; j < n; j++) {
                int up = dp[j];
                int left_diag = (j > 0) ? dp[j-1] : 1e6;
                int right_diag = (j < n-1) ? dp[j+1] : 1e6;
                curr[j] = matrix[i][j] + min({up, left_diag, right_diag});
            }
            dp = curr;
        }
        return *min_element(dp.begin(), dp.end());
    }
};
```

#### Ninja and his friends
- Doing it individually doesn't work.
>Whenever there's a variable ending point. Start the recursion from the starting point.
-  base case: destination, out of bound.
- Always write out of bounds first.
- They both reach destination at the same time.
- For every movement of Alice, Bob can also move to 3 different paths.
- 3X3
```
f(i1,j1,i2,j2)
{
	if(j1<0 || j1>=m || j2<0 || j2>m) return -1e8
	if dp!=-1 return dp
	if(i==n-1)
	{
		if(j1==j2) return a[i][j1];
		else return a[i][j1]+a[i][j2];
	}
	// explore all paths
	maxi=0
	for(dj1 -1 to +1)
		for(dj2 -1 to +1)
			if(j1==j2) maxi=max of maxi,a[i][j1]+f(i+1,j1+dj1,j2+dj2)
			else maxi=max of maxi,a[i][j1]+a[i][j2]+f(i+1,j1+dj1,j2+dj2)
	return dp=maxi
}
```
- Time complexity: $3^{n}*3^{n}$ exponential (without `memoization`)
- Space complexity: $O(n)$ auxiliary stack space
- N x M x M matrix.
- Time complexity: $O(N*M*M)$
- Space complexity: $O(N*M*M)$ + $O(N)$

**Tabulation method:**
```
1. Write the base case.
	- for j1 0 to m-1
	- for j2 0 to m-1
	- if j1=j2 dp[n-1][j1][j2]= grid[n-1][j1]
	- else dp[n-1][j1][j2]=grid[n-1][j1]+grid[n-1][j2]
2. Express every state in for loops.
	- i n-2 to 0
	- j1 0 to m-1
	- j2 0 to m-1
	- for(dj1 -1 to +1)
		for(dj2 -1 to +1)
			if(j1==j2) maxi=max of maxi,a[i][j1]+f(i+1,j1+dj1,j2+dj2)
			else maxi=max of maxi,a[i][j1]+a[i][j2]+f(i+1,j1+dj1,j2+dj2)
3. Space optimization
```
- Base case:
```cpp
for(int j1=0; j1 < m; j1++) {
    for(int j2=0; j2 < m; j2++) {
        if(j1 == j2)
            dp[n-1][j1][j2] = grid[n-1][j1];
        else
            dp[n-1][j1][j2] = grid[n-1][j1] + grid[n-1][j2];
    }
}
```
- Transition:
```cpp
for(int i = n-2; i >= 0; i--) {
    for(int j1 = 0; j1 < m; j1++) {
        for(int j2 = 0; j2 < m; j2++) {
        // try all paths
            int maxi = INT_MIN;
            for(int dj1 = -1; dj1 <= 1; dj1++) {
                for(int dj2 = -1; dj2 <= 1; dj2++) {
                    int nj1 = j1 + dj1;
                    int nj2 = j2 + dj2;
                    if(nj1 >=0 && nj1 < m && nj2 >=0 && nj2 < m) {
                        int value = (j1 == j2) ? grid[i][j1] : grid[i][j1] + grid[i][j2];
                        value += dp[i+1][nj1][nj2];
                        maxi = max(maxi, value);
                    }
                }
            }
            dp[i][j1][j2] = maxi;
        }
    }
}

```
- To space optimize: Use only `curr` and `front`
```cpp
return front[0][m-1];
```
- **Time Complexity:** $O(N * M * M * 9)$
- Space complexity: $O(M^{2})$ (After space optimization)

#### Subset Sum
```
f(ind,target)
{
	if(target==0) return true;
	if(ind==0) return (a[0]==target)
	if(dp!=-1) return dp
	bool notpick=f(ind-1,target);
	bool pick= false
		if(target>=a[ind]) pick=f(ind-1,target-a[ind])

	return dp=pick || notpick
}

// If you want to count the possible subets.
// return 1 instead of true.
// instead of || add '+' them.
```
- Time complexity: $O(2^{n})$
- Space complexity: $O(n)$
- After `memoization`:
- Time complexity: $O(N*target)$
- Space complexity: $O(N*target)+$$O(n)$
- Tabulation:
```
1. declare dp array of same size. dp[n][target]
2. for i=0 to n-1: dp[i][0]=true
3. dp[0][arr[0]] is also true.
4. Form the nested loop:
	- ind: 1 to n-1.
	- target: 1 to target
	- same recurrence condition.
5. return dp[n-1][k]
```
- `dp[i][t]` is true, if subset with sum 't' can be formed from first i elements.
- For every index i `dp[i][0]` is always possible.
- Also, `dp[0][arr[0]]` is always true.
- Time and space complexity: $O(N*target)$
- After space optimization, $O(target)$

#### Partition Equal Subset Sum
- s1=s2
- `s=total sum/2`
- if s is odd then it's not  possible.
- set target to s.
#### Minimize Sum Difference
  - Same process as the tabulation method for subset sum.
```
for(i = 0 to total sum)
	if(dp[n-1][i]==true)
		s1=i
		s2=totsum-i
		mini=min(mini,abs(s2-s1))
```
- We find out all the sums that are possible. We find out the other sum by the equation `total - sum= other sum`.
- To further optimize only check till half.
```cpp
bool subsetSumToK(int n, int k, vector<int>& arr) {
    vector<vector<bool>> dp(n, vector<bool>(k + 1, 0));
    // base condition:
    for(int i = 0; i < n; i++) {
        dp[i][0] = true;  // Sum 0 is always possible
    }
    if(arr[0] <= k)
        dp[0][arr[0]] = true;  
    // for loops:
    for(int ind = 1; ind < n; ind++) {
        for(int target = 1; target <= k; target++) {
            bool notTake = dp[ind - 1][target];
            bool take = false;
            if(arr[ind] <= target)
                take = dp[ind - 1][target - arr[ind]];
            dp[ind][target] = take || notTake;
        }
    }
    return dp[n - 1][k];
}
```
- This approach only works with positive numbers.
- Use Meet in the Middle approach.
- Power set method:
```
write 0 to 2^n-1 in bit representation.
for e.g. S="abc" 0 0 1 says pick a, 0 1 0 says pick b.

for (num= 0 to (1<<n)-1)
	for(i = 0 to n-1)
		if(num & (1<<i))
			sub+s[i]
	print(sub)	
```
- Time complexity: $O(2^{n}*n)$
- Space complexity: $O(1)$
```cpp
class Solution {
public:
    int minimumDifference(vector<int>& nums) {
        // We use half the size of the input array for convenience.
        int halfSize = nums.size() >> 1; // Same as dividing by 2 using right-shift.
        // f and g will store subsets sums for the first and second half respectively.
        vector<vector<int>> sumFirstHalf(halfSize + 1), sumSecondHalf(halfSize + 1);

        // Iterate over all possible subsets for both halves.
        for (int subset = 0; subset < (1 << halfSize); ++subset) {
            int sum1 = 0, count1 = 0;
            int sum2 = 0, count2 = 0;
           // Calculate the subsets sums and their sizes for both halves.
            for (int j = 0; j < halfSize; ++j) {
                if (subset & (1 << j)) {
                    sum1 += nums[j]; // Add to sum for first half.
                    ++count1; // Increment count for first half.
                    sum2 += nums[halfSize + j]; // Add to sum for second half.
                    ++count2; // Increment count for second half.
                } else {
                    sum1 -= nums[j]; // Subtract from sum for first half.
                    sum2 -= nums[halfSize + j]; // Subtract from sum for second half.
                }
            }
            // Store the calculated sums in f and g based on their respective counts.
            sumFirstHalf[count1].push_back(sum1);
            sumSecondHalf[count2].push_back(sum2);
        }
        // Sort the subsets sums for each possible count for easier lookup later.
        for (int i = 0; i <= halfSize; ++i) {
            sort(sumFirstHalf[i].begin(), sumFirstHalf[i].end());
            sort(sumSecondHalf[i].begin(), sumSecondHalf[i].end());
        }
        // Initialize the answer with the largest possible integer value.
        int answer = INT_MAX;
        // Iterate over all pairs of subset from first and second halves.
        for (int i = 0; i <= halfSize; ++i) {
            for (int firstHalfSum : sumFirstHalf[i]) {
                int target = -firstHalfSum; // We want to find the closest to -firstHalfSum in second half.
                // Binary search for the closest value in the second half with complementing size.
                int left = 0, right = sumSecondHalf[halfSize - i].size() - 1;
                while (left < right) {
                    int mid = (left + right) >> 1;
                    if (sumSecondHalf[halfSize - i][mid] >= target)
                        right = mid;
                    else
                        left = mid + 1;
                }
                // Update answer with the closest match before or at the binary search result.
                answer = min(answer, abs(firstHalfSum + sumSecondHalf[halfSize - i][left]));
                if (left > 0)
                    answer = min(answer, abs(firstHalfSum + sumSecondHalf[halfSize - i][left - 1]));
            }
        }
        // Return the minimum difference found.
        return answer;
    }
};
```
- [ ] Re-Attempt
#### Count Partitions with difference
```
s1-s2=D
s1+s2=Total

2s1=D+Total

s1=(D+Total)/2
s2=(Total-D)/2

Find subsets whose total sum is: (Total-D)/2

Total-D>=0
Total-D has to be even
```
- `if(num[0]==0) dp[0][0]=2`, else `dp[0][0]=1`
- `if(num[0]!=0 && num[0]<=tar) dp[0][num[0]]=1`
- Rest is same. 
#### Minimum coins
- Minimum number of coins to achieve an amount. 
- Greedy solution doesn't work.
> infinite supply -> in take index doesn't move.
- for base case always think about the index 0.
```
f(ind,T)
{
	if(ind==0)
	{
		if(T%arr[0]==0) return T/a[i];
		else return INT_MAX;
	}
	int nottake=0+f(ind-1,T);
	int take=INT_MAX;
	if(coins[ind]<=T) take=1+f(ind,T-coins[ind]);

	return min(take,nottake);
}
```
- Time complexity: $O(2^{n})$
- Space complexity: $O(target)$
- ![](attachments/Pasted%20image%2020250718123516.png)
- `Memoization`: over index and target.
- `dp[N][target+1]`
- Time complexity: $O(N*T)$
- Space complexity: $O(N*T)+O(T)$
- Tabulation:
```
for(T: 0 to target)
{
	// if index is 0 how can we achieve the target? 
	// if target is divisible by a[0], just add it's multiple to dp
	if(T%a[0]==0) dp[0][T]=T/a[0];
	else dp[0][T]=INT_MAX;
}

for(int ind=1; ind<n; ind++)
{
	for(int T=0 to target)
	{
		int nottake=0+dp(ind-1,T);
		int take=INT_MAX;
		if(coins[ind]<=T) take=1+dp(ind,T-coins[ind]);
	
		dp[ind][T]= min(take,nottake);
	}
}

return dp[n-1][target]
```
- Then space optimize.