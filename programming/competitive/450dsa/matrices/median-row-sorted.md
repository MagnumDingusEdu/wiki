---
title: Median Row sorted
description: 
published: true
date: 2021-05-19T11:22:22.999Z
tags: 
editor: markdown
dateCreated: 2021-05-19T06:49:32.539Z
---

# Median in a row-wise sorted Matrix 
Given a row wise sorted matrix of size RxC where R and C are always odd, find the median of the matrix.

## Examples

```cpp
Input:
R = 3, C = 3
M = [[1, 3, 5], 
     [2, 6, 9], 
     [3, 6, 9]]

Output: 5

```
```cpp
Input:
R = 3, C = 1
M = [[1], [2], [3]]
Output: 2
```

## Constraints


```cpp
1<= R,C <=150
1<= matrix[i][j] <=2000
```

## Solution
```python
from typing import List

from bisect import bisect_right


# Median in a row-wise sorted Matrix
# Binary search on matrix
# Time complexity :
# size : (m x n) (rows x columns)
# upper_bound function for each row = log(n)
# for each row : m * log(n)
# repeated log(2^32) times for 32 bit integers
# therefore, complexity = log(2^32)*m*log(n) = 32m*log(n)
def median_of_matrix(mat: List[List[int]]) -> int:
    # each element is row wise sorted
    # min element = min element of first col
    # max element = max element of last column
    min_elem = mat[0][0]
    max_elem = mat[0][0]

    height = len(mat)
    width = len(mat[0])

    for row in mat:
        if row[0] < min_elem:
            min_elem = row[0]
        if row[-1] > max_elem:
            max_elem = row[-1]

    # Now, use modified
    # binary search to find the number of
    # elements that are less than current average
    # to find the median

    # for a median, n/2 elements should be less than it
    desired_count = (width * height + 1) // 2

    while min_elem < max_elem:
        elem_count = 0

        middle = min_elem + (max_elem - min_elem) // 2

        # count number of elements less than middle in each row
        for row in mat:
            count = bisect_right(row, middle)
            elem_count += count

        # check if we have reached the median
        if elem_count < desired_count:
            min_elem = middle + 1
        else:
            max_elem = middle

    return min_elem


if __name__ == '__main__':
    matrix = [[1, 3, 5], [2, 6, 9], [3, 6, 9]]
    print(median_of_matrix(matrix))

```
> References: 
https://practice.geeksforgeeks.org/problems/median-in-a-row-wise-sorted-matrix1527/1
https://www.geeksforgeeks.org/find-median-row-wise-sorted-matrix/

{.is-info}
