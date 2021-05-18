---
title: Reverse the array
description: 
published: true
date: 2021-05-18T05:34:47.061Z
tags: 
editor: markdown
dateCreated: 2021-05-18T05:34:47.061Z
---

# Write a program to reverse an array or string

Given an array (or string), the task is to reverse the array/string.

## Examples
```cpp
Input  : arr[] = {1, 2, 3}
Output : arr[] = {3, 2, 1}

Input :  arr[] = {4, 5, 1, 2}
Output : arr[] = {2, 1, 5, 4}
```

## Solution

```python
#!/usr/bin/env python

from typing import List

# O(n)
def iterative_reverse(a: List[int]) -> List[int]:
    for i in range(len(a) // 2):
        start_index, end_index = i, len(a) - 1 - i

        # Swap elements from both the front and back
        a[start_index], a[end_index] = a[end_index], a[start_index]

    return a

# O(n)
def recursive_reverse(a: List[int]) -> List[int]:
    # if array is empty (base case)
    if not a:
        return []

    # add first element to end after reversing the rest of the list
    first_element, reversed_array = a[0], recursive_reverse(a[1:])

    return reversed_array + [first_element]

# O(n)
# Python specific
def direct_reverse(a: List[int]) -> List[int]:
    return a[::-1]
    # or a.reverse()


if __name__ == '__main__':
    arr = [1, 2, 3, 4, 5, 6, 7]
    arr = iterative_reverse(arr)
    print(arr)
```


> Link : https://www.geeksforgeeks.org/write-a-program-to-reverse-an-array-or-string/ 
{.is-info}

