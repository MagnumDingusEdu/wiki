---
title: Subset Count With Given Diff
description: 
published: true
date: 2021-07-14T23:44:54.518Z
tags: 
editor: markdown
dateCreated: 2021-07-14T23:44:54.518Z
---

# Count number of 2-subset partitions with given difference
```cpp
#include <algorithm>
#include <bits/stdc++.h>
#include <sstream>
#include <unordered_map>

using namespace std;

#define ar array
#define ll long long

const int MAX_N = 1e5 + 1;
const ll MOD = 1e9 + 7;
const ll INF = 1e9;
#include <bits/stdc++.h>
using namespace std;

class Solution {
public:
  int getSubsetSumCount(int N, int arr[], int sum) {
    auto dp = new int *[N + 1];
    for (int i = 0; i <= N; i++) {
      dp[i] = new int[sum + 1];
      for (int j = 0; j <= sum; j++) {
        dp[i][j] = 0;
      }
    }

    // init dp
    for (int i = 0; i <= N; i++) {
      // if sum is zero, return true as always possible
      dp[i][0] = 1;
    }
    for (int i = 1; i <= sum; i++) {
      // if elements are 0, only possible if sum is 0, else false
      dp[0][i] = 0;
    }

    for (int i = 1; i <= N; i++) {
      for (int j = 1; j <= sum; j++) {

        // check if current element can be included
        if (arr[i - 1] <= j) {
          // check if answer is true by both including and not including
          // add count of both possiblities to current
          dp[i][j] = dp[i - 1][j] + dp[i - 1][j - arr[i - 1]];
        } else {
          dp[i][j] = dp[i - 1][j];
        }

        // cout << dp[i][j] << " ";
      }

      // cout << endl;
    }

    int answer = dp[N][sum];
    // free up memory
    for (int i = 0; i <= N; i++) {
      delete[] dp[i];
    }
    delete[] dp;

    // return answer
    return answer;
  }
  int countWithGivenDiff(int N, int arr[], int diff) {
    // s2-s1 = diff
    // s2+s1 = sum
    // => s2 = (sum + diff)/2
    // answer = countSubsetsWithSum(s2)

    int sum = 0;
    for (int i = 0; i < N; i++)
      sum += arr[i];

    return getSubsetSumCount(N, arr, (sum + diff) / 2);
  }
};

int32_t main() {
  ios_base::sync_with_stdio(false);
  cin.tie(NULL);
  auto a = Solution();
  int arr[5] = {1, 3, 5, 7, 9};

  auto ans = a.countWithGivenDiff(5, arr, 2);
  cout << ans << endl;
}
```