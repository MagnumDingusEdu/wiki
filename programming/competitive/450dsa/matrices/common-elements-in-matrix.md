---
title: Common Elements in Matrix
description: 
published: true
date: 2021-05-19T14:55:02.563Z
tags: 
editor: markdown
dateCreated: 2021-05-19T07:31:55.415Z
---

# Common elements in all rows of a given matrix
Given an m x n matrix, find all common elements present in all rows in O(mn) time and one traversal of matrix.

## Examples
```cpp
Input:
mat[4][5] = {{1, 2, 1, 4, 8},
             {3, 7, 8, 5, 1},
             {8, 7, 7, 3, 1},
             {8, 1, 2, 7, 9},
            };

Output: 
1 8 or 8 1
// 8 and 1 are present in all rows.
```

## Solution 
```python
from typing import List
from bisect import bisect_left
from collections import deque


# Find common elements in all rows of given matrix
# Time complexity : O(n*m) [one traversal]
# Space complexity : O(x) where x = no of unique elements
# Worst case O(n*m)
def find_repeated_elements(mat: List[List[int]]):
    element_freq = {}
    for row in mat:
        row_set = {}
        for elem in row:
            if elem not in row_set:

                if elem not in element_freq:
                    element_freq[elem] = 1
                else:
                    element_freq[elem] += 1

                row_set[elem] = True
            else:
                continue
    print("The following elements are present in every row : [", end="")
    for elem, frequency in element_freq.items():

        if frequency == len(mat):
            print(elem, end=", ")
    print("]")


if __name__ == '__main__':
    # print(find_largest_area_in_histogram([1, 2, 3, 1, 2, 3]))
    ...
    arr = [[1, 2, 1, 4, 8],
           [3, 7, 8, 5, 1],
           [8, 7, 7, 3, 1],
           [8, 1, 2, 7, 9]]
    find_repeated_elements(arr)

```
> References: https://www.geeksforgeeks.org/common-elements-in-all-rows-of-a-given-matrix/
{.is-info}
