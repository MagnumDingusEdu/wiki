---
title: Rotate matrix 90°
description: 
published: true
date: 2021-05-19T15:39:50.515Z
tags: 
editor: markdown
dateCreated: 2021-05-19T07:27:25.091Z
---

# Rotate a matrix by 90 degree in clockwise direction without using any extra space
Given a square matrix, turn it by 90 degrees in a clockwise direction without using any extra space.

## Examples
```cpp
Input:
1 2 3 
4 5 6
7 8 9  
Output:
7 4 1 
8 5 2
9 6 3

Input:
1 2
3 4
Output:
3 1
4 2 
```

## Solution
```python
from typing import List
from bisect import bisect_left
from collections import deque
from pprint import pprint


# Rotate matrix by 90 degrees
# Time complexity : O(m*n)
# Space complexity : O(1)
# Method one : Flip across diagonal, then flip across vertical axis
def rotate_90_degrees(mat: List[List[int]]):
    height = len(mat)

    # Transpose
    for i in range(height):
        # Limit to top right half
        for j in range(0, i):
            # Swap them
            mat[i][j], mat[j][i] = mat[j][i], mat[i][j]

    # Flip across vertical axis
    for row in mat:
        for i in range(len(row) // 2):
            row[i], row[len(row) - i - 1] = row[len(row) - i - 1], row[i]

    for row in mat:
        print(*row, sep=" ")


# Iterative approach (doesn't actually flip, just prints)
# Time complexity : O(n*m)
# Space complexity : O(1)
def rotate_using_loops(mat: List[List[int]]):
    for j in range(len(mat[0])):
        for i in reversed(range(0, len(mat))):
            print(mat[i][j], end=" ")

        print()


if __name__ == '__main__':
    ...
    arr = [[1, 2, 3, 4],
           [5, 6, 7, 8],
           [9, 10, 11, 12],
           [13, 14, 15, 16]]
    rotate_90_degrees(arr)
    print("#######################")
    rotate_using_loops(arr)

```
> References: 
https://youtu.be/IdZlsG6P17w
https://www.geeksforgeeks.org/rotate-a-matrix-by-90-degree-in-clockwise-direction-without-using-any-extra-space/
{.is-info}
