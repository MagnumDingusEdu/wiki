---
title: Partition Equal Subset Sum
description: 
published: true
date: 2021-07-14T22:32:27.451Z
tags: 
editor: markdown
dateCreated: 2021-07-14T22:32:27.451Z
---

# Partition Equal Subset Sum
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

class Solution {
private:
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

public:
  bool canPartition(vector<int> &nums) {
    int sum = 0;
    for (auto &num : nums) {
      sum += num;
    }

    // sum has to be even in order to be able to split the array into two
    if (sum % 2 != 0)
      return false;

    auto arr = new int[nums.size()];
    copy(nums.begin(), nums.end(), arr);
    
      
    // if a subset with half the sum exists, we have reached a conclusion
    return isSubsetSum(nums.size(), arr, sum / 2);
  }
};
```