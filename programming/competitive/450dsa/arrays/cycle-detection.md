---
title: Cycle Detection
description: 
published: true
date: 2021-05-18T17:52:08.842Z
tags: 
editor: markdown
dateCreated: 2021-05-18T17:52:08.842Z
---

# Find duplicate in an array of N+1 Integers
Given an array of integers nums containing n + 1 integers where each integer is in the range [1, n] inclusive.

There is only one repeated number in nums, return this repeated number.

You must solve the problem without modifying the array nums and uses only constant extra space.

## Examples
```cpp
Input: nums = [1,3,4,2,2]
Output: 2
```
```cpp
Input: nums = [3,1,3,4,2]
Output: 3
```
```cpp
Input: nums = [1,1]
Output: 1
```

## Solution
Uses Floyd's Tortoise and Hare (Cycle Detection)
```python
from typing import List, Dict


# Find duplicate number in array
# Time complexity : O(n)
# Space complexity : O(1)
# Array is immutable
def find_duplicate(arr: List[int]):
    # Initialize slow and fast pointers
    slow_pointer = arr[0]
    fast_pointer = arr[0]

    # Wait until the two pointers collide : this means there is a loop
    while True:
        slow_pointer = arr[slow_pointer]
        fast_pointer = arr[arr[fast_pointer]]
        if slow_pointer == fast_pointer:
            break

    # Reset the slow pointer
    slow_pointer = arr[0]

    # Increment both at same speed
    while slow_pointer != fast_pointer:
        slow_pointer = arr[slow_pointer]
        fast_pointer = arr[fast_pointer]


    # Return either of the two when they are equal
    return fast_pointer


if __name__ == '__main__':
    arr: List[int] = [3, 1, 3, 4, 2]

    a = find_duplicate(arr)
    print(a)
```