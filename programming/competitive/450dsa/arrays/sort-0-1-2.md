---
title: Sort 0/1/2 Array
description: 
published: true
date: 2021-05-18T12:51:54.314Z
tags: 
editor: markdown
dateCreated: 2021-05-18T12:51:54.314Z
---

# Sort an array of 0s, 1s and 2s 
Given an array of size N containing only 0s, 1s, and 2s; sort the array in ascending order without using any sorting algorithm.

## Example
```cpp
Input: 
  N = 5
  arr[]= {0 2 1 2 0}
Output:
  0 0 1 2 2
```

## Constraints
```cpp
1 <= N <= 10^6
0 <= A[i] <= 2
```

 ## Solution
 ```python
 from typing import List


# Sort an array containing just 0s, 1s and 2s
# Do not use any sorting algo
# Time complexity : O(n)
# Auxiliary space complexity : O(1)
def sort_0_1_2(arr: List[int]):
    zero_pointer = 0

    for i in range(0, len(arr)):
        if arr[i] == 0:
            arr[i], arr[zero_pointer] = arr[zero_pointer], arr[i]
            zero_pointer += 1

    one_pointer = zero_pointer

    for i in range(one_pointer, len(arr)):
        if arr[i] == 1:
            arr[i], arr[one_pointer] = arr[one_pointer], arr[i]
            one_pointer += 1


if __name__ == '__main__':
    arr: List[int] = [0, 2, 1, 2, 0]
    sort_0_1_2(arr)
    print(arr)
```

> References: https://practice.geeksforgeeks.org/problems/sort-an-array-of-0s-1s-and-2s4231/1
{.is-info}
