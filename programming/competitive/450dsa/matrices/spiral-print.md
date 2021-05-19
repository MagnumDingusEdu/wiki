---
title: Spiral Print
description: 
published: true
date: 2021-05-19T09:56:31.158Z
tags: 
editor: markdown
dateCreated: 2021-05-19T06:24:47.306Z
---

# Spiral traversal of a Matrix
Given a 2D array, print it in spiral form.

## Examples
```cpp
Input:  1    2   3   4

        5    6   7   8

        9   10  11  12

        13  14  15  16

Output: 1 2 3 4 8 12 16 15 14 13 9 5 6 7 11 10 

/////////////////////////////////////////

Input:  1   2   3   4  5   6

        7   8   9  10  11  12

        13  14  15 16  17  18

Output: 1 2 3 4 5 6 12 18 17 16 15 14 13 7 8 9 10 11

```
## Solution
```python
from typing import List, Dict, Tuple
from enum import Enum


# Using a visited map
# Method one
# Time complexity : m*n (one traversal)
# Space complexity : m*n (storage dict)
def print_spiral_matrix(mat: List[List[int]]) -> List[int]:
    visited: Dict[Tuple[int, int], bool] = {}
    i = 0  # Row index
    j = 0  # Column index

    height = len(mat)
    width = len(mat[0])

    class Direction(Enum):
        RIGHT = 0
        DOWN = 1
        LEFT = 2
        UP = 3

    current_direction = 0

    answer: List[int] = [mat[0][0]]
    visited[(0, 0)] = True

    while True:

        if current_direction == Direction.RIGHT.value:
            if j + 1 < width and not (i, j + 1) in visited:

                j = j + 1
                visited[(i, j)] = True
                answer.append(mat[i][j])
            else:
                current_direction += 1
                current_direction %= 4
        elif current_direction == Direction.DOWN.value:
            if i + 1 < height and not (i + 1, j) in visited:

                i += 1
                visited[(i, j)] = True
                answer.append(mat[i][j])
            else:
                current_direction += 1
                current_direction %= 4
        elif current_direction == Direction.LEFT.value:
            if j - 1 >= 0 and not (i, j - 1) in visited:

                j -= 1
                visited[(i, j)] = True
                answer.append(mat[i][j])
            else:
                current_direction += 1
                current_direction %= 4
        elif current_direction == Direction.UP.value:
            if i - 1 >= 0 and not (i - 1, j) in visited:

                i -= 1
                visited[(i, j)] = True
                answer.append(mat[i][j])
            else:
                current_direction += 1
                current_direction %= 4
        if len(visited) == width * height:
            break
    return answer


# Method two
# Using recursion
# Time complexity : m*n (one traversal)
# Space complexity : O(1)
def print_spiral_recursive(mat: List[List[int]]) -> List[int]:
    answer: List[int] = []

    def recursive_helper(i, j, m, n):
        i: int  # row
        j: int  # col

        m: int  # row limit
        n: int  # col limit

        # We have reached outside the bounds of the matrix
        if i >= m or j >= n:
            return

        # print first row
        for p in range(j, n):
            answer.append(mat[i][p])

        # print right col
        for p in range(i + 1, m):
            answer.append(mat[p][n - 1])

        # print bottom row, if height > 1
        if m - i > 1:
            for p in reversed(range(j, n - 1)):
                answer.append(mat[m - 1][p])

        # print left col, if width > 1
        if n - j > 1:
            for p in reversed(range(i + 1, m - 1)):
                answer.append(mat[p][j])

        return recursive_helper(i + 1, j + 1, m - 1, n - 1)

    recursive_helper(0, 0, len(mat), len(mat[0]))
    return answer


# The above method can also be done using iteration

if __name__ == '__main__':
    matrix = [[1, 2, 3],
              [4, 5, 6],
              [7, 8, 9]]

    ans = print_spiral_recursive(matrix)
    print(ans)

```
> References: https://practice.geeksforgeeks.org/problems/spirally-traversing-a-matrix-1587115621/1
{.is-info}
