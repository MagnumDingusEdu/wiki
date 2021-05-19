---
title: Find element
description: 
published: true
date: 2021-05-19T10:33:14.586Z
tags: 
editor: markdown
dateCreated: 2021-05-19T06:45:09.300Z
---

# Search an element in a matrix
Write an efficient algorithm that searches for a value in an m x n matrix. This matrix has the following properties:

* Integers in each row are sorted from left to right.
* The first integer of each row is greater than the last integer of the previous row.

## Examples
![mat.jpg](/mat.jpg)
```cpp
Input: 

	matrix = [[ 1, 3, 5, 7],
  					[10,11,16,20],
  					[23,30,34,60]]
	target = 3
  
Output: true

```
```cpp
Input: matrix = [[1,3,5,7],
                [10,11,16,20],
                [23,30,34,60]]
                target = 13
Output: false
```

## Constraints


```cpp
m == matrix.length
n == matrix[i].length
1 <= m, n <= 100
-104 <= matrix[i][j], target <= 104
```

## Solution
```python
from typing import List


# Search sorted matrix
# Binary search on matrix
# Time complexity : O(log m * log n) or O(log n)
def search_matrix(mat: List[List[int]], val: int) -> bool:
    height = len(mat)
    width = len(mat[0])

    i_low: int = 0  # row
    i_high: int = height - 1

    j_low: int = 0  # col
    j_high: int = width - 1

    # Find the correct row
    while True:
        # If search exhausts, return false
        if i_high < i_low:
            return False

        middle = (i_low + i_high) // 2

        first_elem = mat[middle][0]
        last_elem = mat[middle][-1]

        # check if elem is in row
        if first_elem <= val <= last_elem:
            final_row = middle
            break

        # Corner case
        if i_high == i_low:
            return False

        # redefine boundaries if not found
        elif val < first_elem:

            i_high = middle
        elif val > last_elem:
            i_low = middle + 1

    # Find the correct col using a binary search in row
    while True:
        # Check boundaries, return false if not found
        if j_low > j_high:
            return False
        # Corner case
        elif j_low == j_high:
            return mat[final_row][j_low] == val

        middle = (j_low + j_high) // 2

        if mat[final_row][middle] == val:
            return True
        elif mat[final_row][middle] < val:
            j_low = middle + 1
        elif mat[final_row][middle] > val:
            j_high = middle


if __name__ == '__main__':
    matrix = [[1, 2]]

    print(search_matrix(matrix, 2))

```
> References: https://leetcode.com/problems/search-a-2d-matrix/
{.is-info}
