---
title: Subset Sum Problem
description: 
published: true
date: 2021-07-14T22:17:10.483Z
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



```cpp
#include <bits/stdc++.h>
using namespace std;

#define ar array
#define ll long long

const int MAX_N = 1e5 + 1;
const ll MOD = 1e9 + 7;
const ll INF = 1e9;

class Solution {
public:
  bool isSubsetSum(int N, int arr[], int sum) {
    auto dp = new bool *[N + 1];
    for (int i = 0; i <= N; i++) {
      dp[i] = new bool[sum + 1];
      for (int j = 0; j <= sum; j++) {
        dp[i][j] = false;
      }
    }

    // init dp
    for (int i = 0; i <= N; i++) {
      // if sum is zero, return true as always possible
      dp[i][0] = true;
    }
    for (int i = 1; i <= sum; i++) {
      // if elements are 0, only possible if sum is 0, else false
      dp[0][i] = false;
    }

    for (int i = 1; i <= N; i++) {
      for (int j = 1; j <= sum; j++) {
        // check if current element can be included
        if (arr[i - 1] <= j) {
          // check if answer is true by both including and not including
          dp[i][j] = dp[i - 1][j] || dp[i - 1][j - arr[i - 1]];
        } else {
          dp[i][j] = dp[i - 1][j];
        }
      }
    }

    bool answer = dp[N][sum];
    // free up memory
    for (int i = 0; i <= N; i++) {
      delete[] dp[i];
    }
    delete[] dp;

    // return answer
    return answer;
  }
};

int32_t main() {
  ios_base::sync_with_stdio(false);
  cin.tie(NULL);
  auto a = Solution();
  int arr[6] = {3, 34, 4, 12, 5, 2};

  auto ans = a.isSubsetSum(6, arr, 9);
  cout << ans << endl;
}
```


