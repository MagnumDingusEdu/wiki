---
title: Split -/+ parts
description: 
published: true
date: 2021-05-18T13:05:31.831Z
tags: 
editor: markdown
dateCreated: 2021-05-18T13:05:05.510Z
---

# Move all the negative elements to one side of the array 
Given an array of positive and negative numbers, arrange them such that all negative integers appear before all the positive integers in the array without using any additional data structure like hash table, arrays, etc. The order of appearance should be maintained.

## Example
```cpp
Input:  [12 11 -13 -5 6 -7 5 -3 -6]
Output: [-13 -5 -7 -3 -6 12 11 6 5]
```

## Solution
```python
from typing import List


# Using the partition process of quicksort
# Time complexity : O(n)
# Space complexity : O(1)
def split_negative_and_positive(arr: List[int]):
    start_pointer = 0
    for i in range(0, len(arr)):
        if arr[i] < 0:
            arr[start_pointer], arr[i] = arr[i], arr[start_pointer]
            start_pointer += 1


# Using two pointer method
# Still doesn't preserve order
# Time complexity : O(n)
# Space complexity : O(1)
def two_pointer_split(arr: List[int]):
    start_pointer = 0
    end_pointer = len(arr) - 1

    while True:
        # Advance start pointer while we are getting negative numbers
        while arr[start_pointer] < 0 and start_pointer < len(arr):
            start_pointer += 1

        # Advance end pointer while we are getting positive numbers
        while arr[end_pointer] > 0 and end_pointer >= 0:
            end_pointer -= 1

        # Check if we have met in the middle, swap if not
        if start_pointer < end_pointer:
            arr[start_pointer], arr[end_pointer] = arr[end_pointer], arr[start_pointer]
        else:
            break


if __name__ == '__main__':
    arr: List[int] = [-12, 11, -13, -5, 6, -7, 5, -3, -6]
    two_pointer_split(arr)
    print(arr)
```

> References: https://www.geeksforgeeks.org/rearrange-positive-and-negative-numbers/
{.is-info}
