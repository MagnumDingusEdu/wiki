---
title: LargestContSubarr
description: 
published: true
date: 2021-05-18T14:27:27.587Z
tags: 
editor: markdown
dateCreated: 2021-05-18T14:27:27.587Z
---

# Find Largest sum contiguous Subarray [V. IMP]
Given an array arr of N integers. Find the contiguous sub-array with maximum sum.

## Example
```cpp
Input:
N = 5
arr[] = {1,2,3,-2,5}
Output:
9

```
## Solution
```python
from typing import List


# Largest sum contiguous subarray
# Time complexity : O(n)
# Space complexity : O(1)
def largest_contiguous_subarray_sum(arr: List[int]) -> int:
    largest_so_far = float('-inf')
    current_max = 0

    for elem in arr:
        current_max += elem
        if largest_so_far < current_max:
            largest_so_far = current_max

        # Reset the sum if we go negative, as we can always start from 0
        if current_max < 0:
            current_max = 0

    return largest_so_far


if __name__ == '__main__':
    arr: List[int] = [1, 2, 3, -2, 5]

    ans = largest_contiguous_subarray_sum(arr)

    print(ans)

```