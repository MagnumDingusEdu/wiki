---
title: Max Ones in row
description: 
published: true
date: 2021-05-19T11:54:48.430Z
tags: 
editor: markdown
dateCreated: 2021-05-19T06:54:26.258Z
---

# Row with max 1s 
Given a boolean 2D array of n x m dimensions where each row is sorted. Find the 0-based index of the first row that has the maximum number of 1's.

## Examples

```cpp
Input: 
N = 4 , M = 4
Arr[][] = [[0, 1, 1, 1],
           [0, 0, 1, 1],
           [1, 1, 1, 1],
           [0, 0, 0, 0]]
Output: 2
```
```cpp
Input: 
N = 2, M = 2
Arr[][] = [[0, 0], [1, 1]]
Output: 1
```

## Constraints


```cpp
Expected Time Complexity: O(N+M)
Expected Auxiliary Space: O(1)

1 ≤ N, M ≤ 103
0 ≤ Arr[i][j] ≤ 1 
```

## Solution
> The GFG page doesn't actually use sorted test cases so the optimized solution will fail. 
The brute force one works just fine.
{.is-danger}

```python
from typing import List
from bisect import bisect_left


# Find max number of ones in each row
# Condition : array is sorted row wise, only has 1s and 0s
# Trivial solution => iterate through entire array, count 1s per row
# Time complexity : O(m*n)
def trivial_counter(mat: List[List[int]]):
    max_count = 0
    max_row = 0
    for i, row in enumerate(mat):
        count = row.count(1)
        if count > max_count:
            max_row = i
            max_count = count
    if max_count == 0:
        return -1
    return max_row


# Method two
# Use binary search to get th index of the first one in each row
# Complexity : rows * log(columns) = m*log(n)
# ===============================================================
# possible improvement : check whether the previous max index is
# already 1 before binary search, otherwise ignore row
# average complexity < mlogn, but worst case is still the same
def binary_counter(mat: List[List[int]]):
    max_count = 0
    width = len(mat[0])
    for row in mat:

        # ignore row if less ones than previous max
        if row[width - max_count - 1] != 0:
            # count ones using inbuilt bisection
            one_count = width - bisect_left(row, 1)

            # update max rows seen so far
            max_count = max(max_count, one_count)

    return max_count


# Method 3
# Most optimized method
# Time complexity : O(m+n)
# Returns the 0-indexed row with max ones
def optimized_counter(mat: List[List[int]]) -> int:
    width = len(mat[0])

    # Initialize max count with index of 1 of first row
    max_one_count = width - bisect_left(mat[0], 1)
    current_max_row = 0

    # For each remaining row, check if the
    # next index is also one
    # if yes, iterate till you hit 0
    for i, row in enumerate(mat[1:]):
        # check if we have one more one than previous max
        if row[width - max_one_count - 1] == 1:
            current_max_row = i + 1
            while max_one_count < width and row[width - max_one_count - 1] == 1:
                max_one_count += 1
    return current_max_row


if __name__ == '__main__':
    arr = [[0, 0, 0, 0, 1, 1, 1, 1, 1],
           [0, 0, 0, 0, 1, 1, 1, 1, 1],
           [0, 0, 1, 1, 1, 1, 1, 1, 1],
           [0, 0, 1, 1, 1, 1, 1, 1, 1],
           [0, 0, 0, 0, 0, 0, 0, 1, 1],
           [0, 1, 1, 1, 1, 1, 1, 1, 1],
           [0, 0, 0, 0, 0, 0, 1, 1, 1],
           [0, 0, 0, 1, 1, 1, 1, 1, 1]]

    ans = optimized_counter(arr)
    print(ans)

```


> References: https://practice.geeksforgeeks.org/problems/row-with-max-1s0023/1
[.is-info]
