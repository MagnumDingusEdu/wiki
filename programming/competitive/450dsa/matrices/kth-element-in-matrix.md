---
title: Kth Element in Mat
description: 
published: true
date: 2021-05-19T16:09:30.003Z
tags: 
editor: markdown
dateCreated: 2021-05-19T07:30:11.270Z
---

#  Kth element in Matrix 
Given a N x N matrix, where every row and column is sorted in non-decreasing order. Find the kth smallest element in the matrix.

## Examples
```cpp
Input:
N = 4
mat[][] =     [[ 16, 28, 60, 64 ],
               [ 22, 41, 63, 91 ],
               [ 27, 50, 87, 93 ],
               [ 36, 78, 87, 94 ]]
K = 3
Output: 27
```

## Constraints
Expected Time Complexity: `O(N*Log(N))`
Expected Auxiliary Space: `O(N)`
```cpp
1 <= N <= 50
1 <= mat[][] <= 10000
1 <= K <= N*N
```

## Solution
```python
from heapq import *
from typing import List


# Kth smallest element in matrix
# Uses a min heap
# Matrix size : mxn
# Time complexity : O(k*log(cols)) + O(cols)
# in short :  O(n+k*log(n))
# Space complexity : O(n)

def kth_smallest_element_matrix(mat: List[List[int]], k: int):
    heap = []
    for i, elem in enumerate(mat[0]):
        # Push each element of the first row on to the heap with
        # it's row and column index
        heappush(heap, (elem, 0, i))

    for i in range(k - 1):
        elem = heappop(heap)
        i = elem[1]
        j = elem[2]

        # Push the next element of that column to the heap
        if i + 1 < len(mat):
            heappush(heap, (mat[i + 1][j], i + 1, j))

    return heappop(heap)[0]


if __name__ == '__main__':
    ...
    arr = [[10, 20, 30, 40],
           [15, 25, 35, 45],
           [25, 29, 37, 48],
           [32, 33, 39, 50]]
    ans = kth_smallest_element_matrix(arr, 7)
    print(ans)

```
> References: https://practice.geeksforgeeks.org/problems/kth-element-in-matrix/1
{.is-info}
