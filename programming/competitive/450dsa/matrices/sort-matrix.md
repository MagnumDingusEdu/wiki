---
title: Sort Matrix
description: 
published: true
date: 2021-05-19T12:20:30.446Z
tags: 
editor: markdown
dateCreated: 2021-05-19T07:18:15.021Z
---

# Sorted matrix
You are given an NxN matrix. Sort all elements of the matrix.

## Examples
```cpp
Input:
N=4
Mat=[[10,20,30,40],
    [15,25,35,45] 
    [27,29,37,48] 
    [32,33,39,50]]
Output:
    10 15 20 25 
    27 29 30 32
    33 35 37 39
    40 45 48 50
```

## Constraints
Expected Time Complexity: `O(N^2LogN)`
Expected Auxillary Space: `O(N^2)`

```cpp
1<=N<=1000
1<=Mat[i][j]<=105
```

## Solution
> Solution with O(1) space complexity missing
{.is-warning}

```python
from typing import List
from bisect import bisect_left


# Sort matrix
# copy matrix into array, sort array, copy back
# Time complexity : O(m*n*log(m*n))
def sort_mat(mat: List[List[int]]):
    arr = []

    for row in mat:
        for elem in row:
            arr.append(elem)

    arr.sort()

    current_index = 0

    for i, row in enumerate(mat):
        for j, _ in enumerate(row):
            mat[i][j] = arr[current_index]
            current_index += 1


if __name__ == '__main__':
    arr = [[0, 0, 0, 0, 1, 1, 1, 1, 1],
           [0, 0, 0, 0, 1, 1, 1, 1, 1],
           [0, 0, 1, 1, 1, 1, 1, 1, 1],
           [0, 0, 1, 1, 1, 1, 1, 1, 1],
           [0, 0, 0, 0, 0, 0, 0, 1, 1],
           [0, 1, 1, 1, 1, 1, 1, 1, 1],
           [0, 0, 0, 0, 0, 0, 1, 1, 1],
           [0, 0, 0, 1, 1, 1, 1, 1, 1]]

    sort_mat(arr)
    print(arr)

```
> References: https://practice.geeksforgeeks.org/problems/sorted-matrix2333/1
{.is-info}

