---
title: 0 - 1 KnapSack
description: 
published: true
date: 2021-06-02T22:28:03.384Z
tags: 
editor: markdown
dateCreated: 2021-06-02T22:28:03.384Z
---

# 0 - 1 Knapsack (Dynamic Programming)



# Solution
There are two approaches to this question
* bottom up (memoiaziton)
* top down (dynamic programming)

Both have the same time and space complexity, but the top down approach cannot run into stack limits which might be possible with the recursive solution.


## Example
```cpp
Input:
N = 3
W = 4
values[] = {1,2,3}
weight[] = {4,5,1}
Output: 3
```
```cpp
Input:
N = 3
W = 3
values[] = {1,2,3}
weight[] = {4,5,6}
Output: 0
```
```cpp
Input:
N = 3
W = 50
values[] = {60,100,120}
weight[] = {10,20,30}
Output: 220
```

## Solution 1 (Bottom up) (Python)
```python
from typing import List



def knapsack(capacity: int, weights: List[int], values: List[int], size):
    store = {}

    def helper(remaining_capacity, current_size):

        if tuple((remaining_capacity, current_size)) in store:
            return store[(remaining_capacity, current_size)]

        if remaining_capacity == 0 or current_size == 0:
            return 0

        last_weight = weights[current_size - 1]
        last_value = values[current_size - 1]

        excluded_value = helper(remaining_capacity, current_size - 1)

        if last_weight <= remaining_capacity:
            answer = max(excluded_value, last_value + helper(
                remaining_capacity - last_weight, current_size - 1
            ))
        else:
            answer = excluded_value

        store[(remaining_capacity, current_size)] = answer
        return answer

    return helper(capacity, size)



    
class Solution:

    # Function to return max value that can be put in knapsack of capacity W.
    def knapSack(self, W, wt, val, n):
        return knapsack(W,wt,val,n)
#{ 
#  Driver Code Starts
#Initial Template for Python 3
import atexit
import io
import sys

# Contributed by : Nagendra Jha

if __name__ == '__main__':
    test_cases = int(input())
    for cases in range(test_cases):
        n = int(input())
        W = int(input())
        val = list(map(int,input().strip().split()))
        wt = list(map(int,input().strip().split()))
        ob=Solution()
        print(ob.knapSack(W,wt,val,n))
# } Driver Code Ends
```

## Solution 2 (top-down) (C++)
```cpp
#include <bits/stdc++.h>

#define int long long
using namespace std;


class Solution {
public:
    //Function to return max value that can be put in knapsack of capacity W.
    int knapSack(int capacity, const int weights[], const int values[], int size) {

        // base case
        if (capacity == 0 || size == 0)
            return 0;

        // create dp of size (capacity + 1) * (size + 1)
        auto t = new int *[capacity + 1];
        for (int i = 0; i < capacity + 1; ++i) {
            t[i] = new int[size + 1];
        }

        // initialize dp with base case
        // answer is 0 if size is 0
        for (int i = 0; i <= capacity; ++i) {
            t[i][0] = 0;
        }
        // answer is 0 if capacity is 0
        for (int i = 0; i <= size; ++i) {
            t[0][i] = 0;
        }

        // loop through the entire dp and fill it up
        for (int current_capacity = 1; current_capacity <= capacity; ++current_capacity) {
            for (int current_length = 1; current_length <= size; ++current_length) {

                int value = values[current_length - 1];
                int weight = weights[current_length - 1];

                int excluded_profit = t[current_capacity][current_length - 1];

                if (weight <= current_capacity) {
                    t[current_capacity][current_length] =
                            max(
                                    excluded_profit,
                                    value + t[current_capacity - weight][current_length - 1]);
                } else {
                    t[current_capacity][current_length] = excluded_profit;
                }

            }
        }

        int answer = t[capacity][size];
        // clean up memory
        for (int i = 0; i <= capacity; ++i) {
            delete[] t[i];
        }
        delete[] t;

        return answer;

    }
};

int32_t main() {

    auto obj = Solution();
    int n, w;
    cin >> n >> w;
    int wt[n], val[n];

    for (int i = 0; i < n; ++i) {
        cin >> wt[i] >> val[i];
    }

    cout << obj.knapSack(w, wt, val, n) << endl;

};
```