---
title: Rotate Array
description: 
published: true
date: 2021-05-18T14:14:29.309Z
tags: 
editor: markdown
dateCreated: 2021-05-18T14:14:29.309Z
---

# Cyclically rotate an array by one 
Given an array, rotate the array by one position in clock-wise direction.

## Example
```cpp
Input:
N = 8
A[] = {9, 8, 7, 6, 4, 2, 1, 3}
Output:
3 9 8 7 6 4 2 1
```

## Solution
```python
from typing import List


# Rotate array
# Space complexity : O(1)
# Time complexity : O(n)
def rotate_array(arr: List[int]):
    last_elem: int = arr[-1]
    for i in reversed(range(1, len(arr))):
        arr[i] = arr[i - 1]

    arr[0] = last_elem


if __name__ == '__main__':
    arr: List[int] = [1, 2, 3, 4, 5, 6, 7]

    rotate_array(arr)
    print(arr)
```