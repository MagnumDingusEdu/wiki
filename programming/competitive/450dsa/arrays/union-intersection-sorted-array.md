---
title: Union/Intersection of Sorted Arrays
description: 
published: true
date: 2021-05-18T14:08:52.049Z
tags: 
editor: markdown
dateCreated: 2021-05-18T14:08:52.049Z
---

# Find the Union and Intersection of the two sorted arrays.
Given two sorted arrays, find their union and intersection.

## Example
```cpp
Input : arr1[] = {1, 3, 4, 5, 7}
        arr2[] = {2, 3, 5, 6} 
Output : Union : {1, 2, 3, 4, 5, 6, 7} 
         Intersection : {3, 5}

Input : arr1[] = {2, 5, 6}
        arr2[] = {4, 6, 8, 10} 
Output : Union : {2, 4, 5, 6, 8, 10} 
         Intersection : {6}
```
## Solution
```python
from typing import List


# Union and Intersection
# Time complexity : O(m + n)
# Space complexity O(m + n)
def union_and_intersection(arr1: List[int], arr2: List[int]):
    union = []
    intersection = []

    i, j = 0, 0

    while True:
        if i >= len(arr1) or j >= len(arr2):
            break

        if arr1[i] < arr2[j]:
            union.append(arr1[i])
            i += 1
            continue
        elif arr1[i] > arr2[j]:
            union.append(arr2[j])
            j += 1

        elif arr1[i] == arr2[j]:
            union.append(arr1[i])
            intersection.append(arr1[i])
            i += 1
            j += 1

    while i < len(arr1):
        union.append(arr1[i])
        i += 1
    while j < len(arr2):
        union.append(arr2[j])
        j += 1

    print(union, intersection)


if __name__ == '__main__':
    arr1: List[int] = [1, 2, 3, 4, 4, 5, 6, 8]
    arr2: List[int] = [4, 5, 7, 8, 8, 9]
    union_and_intersection(arr1, arr2)
```