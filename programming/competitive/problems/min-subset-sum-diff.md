---
title: Minimum Subset Sum Diff
description: 
published: true
date: 2021-07-14T23:33:04.397Z
tags: 
editor: markdown
dateCreated: 2021-07-14T23:33:04.397Z
---

# Min difference between two subset sums
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
  int minDifference(int N, int arr[]) {

    int sum = 0;
    for (int i = 0; i < N; i++)
      sum += arr[i];

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

    // return the max value going back from sum/2
    // reference : https://youtu.be/-GtpxG6l_Mc
    for (int i = sum / 2; i >= 0; i--) {
      if (dp[N][i]) {
        // cout << sum << endl <<  i << endl;
        return sum - (2 * i);
      }
    }

    int answer = dp[N][sum / 2];
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
  int arr[5] = {1, 4};

  auto ans = a.minDifference(2, arr);
  cout << ans << endl;
}
```