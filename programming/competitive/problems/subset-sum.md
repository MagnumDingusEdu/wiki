---
title: Subset Sum Problem
description: 
published: true
date: 2021-06-03T02:38:11.615Z
tags: dynamic programming, knapsack
editor: markdown
dateCreated: 2021-06-03T02:38:11.615Z
---

# Subset Sum Problem


## Iterative DP Solution 
```python
import sys
from os import path
from typing import List


def sum_exists(n, arr, r_sum) -> bool:

    dp  = [[-1 for _ in range(r_sum + 1)] for _ in range(n + 1)]

    # if r_sum = 0, empty set is answer, hence true

    for x in range(n + 1):
        dp[x][0] = True

    # if there are no elements, i.e. n = 0, answer false
    # exception : sum required is also zero
    for x in range(1, r_sum + 1):
        dp[0][x] = False

    # Iterate through rest of dp
    for arr_size in range(1, n + 1):
        for current_sum in range(1, r_sum + 1):
            last_elem = arr[arr_size - 1]
            if last_elem == current_sum:
                dp[arr_size][current_sum] = True
            elif last_elem < current_sum:
                dp[arr_size][current_sum] = \
                    dp[arr_size - 1][current_sum] or \
                    dp[arr_size - 1][current_sum - last_elem]
            else:
                dp[arr_size][current_sum] = dp[arr_size - 1][current_sum]


    return int(dp[n][r_sum])
    
if __name__ == "__main__":
    if(path.exists('input.txt')):
        sys.stdin = open("input.txt","r")
        sys.stdout = open("output.txt","w")

    n = int(input())

    arr = list(map(int, input().split()))

    req_sum = int(input())

    ans = sum_exists(n, arr, req_sum)

    print(ans)
```

> CPP Solution is giving segfaults (unresolved)
{.is-warning}

```cpp
#include <bits/stdc++.h>

using namespace std;





void print_2d_array(bool** arr, int m, int n){
	for (int i = 0; i < m; ++i)
	{
		for (int j = 0; j < n; ++j)
		{
			cout << arr[i][j] << " ";
		}
		cout << endl;
	}
}


bool check_subset(int n, int arr[], int sum){
	// base case
	if (n == 0 && sum == 0)
		return true;

	// create a dp array
	auto dp = new bool*[n + 1];
	for (int i = 0; i <= n; ++i)
	{
		dp[i] = new bool[sum];
	}


	// if array is empty, answer will be false, except for 0/0
	for (int i = 1; i<= sum; ++i)
	{
		dp[0][i] = false;
	}

	// if required sum (column) is 0, answer is true always
	for (int i = 0; i <= n; ++i)
	{
		dp[i][0] = true;
	}

	for (int i = 1; i <= n; ++i)
	{
		for (int j = 1; j <= sum; ++j)
		{
			bool excluded = dp[i - 1][j];

			// required sum is more than last value
			if (arr[i - 1] <= j)
			{
				dp[i][j] = excluded || dp[i-1][j-arr[i-1]];
			}else {
				 dp[i][j] =  excluded;
			}

		}
	}


	#ifndef ONLINE_JUDGE

	print_2d_array(dp, n+1, sum+1);
	#endif

	return dp[n][sum];

	






	// free up memory

	for (int i = 0; i <= n; ++i)
	{
		delete [] dp[i];
	}

	delete [] dp;

	return false;
	
}


int32_t main(){
	#ifndef ONLINE_JUDGE
		freopen("input.txt", "r", stdin);
		freopen("output.txt", "w", stdout);
	#endif

	
	int n, sum;

	cin >> n;

	int arr[n];

	for (int i = 0; i < n; ++i)
	{
		cin >> arr[i];
	}

	cin >> sum;

	auto ans = check_subset(n, arr, sum);

	cout << ans << endl;

}
```


