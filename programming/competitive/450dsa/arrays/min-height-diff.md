---
title: MinHeightDiff
description: 
published: true
date: 2021-05-18T15:35:54.574Z
tags: 
editor: markdown
dateCreated: 2021-05-18T15:35:54.574Z
---

# Minimise the maximum difference between heights
Given an array arr[] denoting heights of N towers and a positive integer K, you have to modify the height of each tower either by increasing or decreasing them by K only once. After modifying, height should be a non-negative integer. 
Find out what could be the possible minimum difference of the height of shortest and longest towers after you have modified each tower.

A slight modification of the problem can be found [here](https://practice.geeksforgeeks.org/problems/minimize-the-heights-i/1/) (negative heights are allowed).

## Example
```cpp
// The array can be modified as 
// {3, 3, 6, 8}. The difference between 
// the largest and the smallest is 8-3 = 5.

Input: K = 2, N = 4
Arr[] = {1, 5, 8, 10}
Output: 5

Input: K = 3, N = 5
Arr[] = {3, 9, 12, 16, 20}
Output: 11
```

## Solution
```python
from typing import List, Dict


# Get min diff between heights using sliding window method
# Time complexity : O(n)
# Space complexity : O(n)
def minimize_height_difference(arr: List[int], k: int):
    class Combo:
        def __init__(self, value, pos):
            self.value = value
            self.pos = pos

    # List to store all possible combinations of (elem + k) and (elem - k)
    # <value,index>
    combos = []

    # Keep track of the number of times an index appears in the current window
    visited: Dict[int] = {}

    # Initialize the visited map with 0s
    for index in range(len(arr)):
        visited[index] = 0

    # Step 1 : Generate combos
    for index, elem in enumerate(arr):
        # Ignore elem - k if it is non-negative (as per ques)
        if elem - k >= 0:
            combos.append(Combo(elem - k, index))
        combos.append(Combo(elem + k, index))

    # Sort the generated combos by value
    combos.sort(key=lambda combo: combo.value)

    # Initialize the sliding window
    start_index = 0
    end_index = 0
    elem_count = 0

    # Method :
    # Ensure that all indexes appear in the window at least once

    while elem_count < len(arr) and end_index < len(combos):
        # increment elem_count if we add a unvisited elem to the window
        if visited[combos[end_index].pos] == 0:
            elem_count += 1

        visited[combos[end_index].pos] += 1
        end_index += 1

    # At this point we have all indexes at least once
    # Calculate and store the initial answer
    # We are using end_index - 1 as it was added the last time we exited the while loop
    ans: int = combos[end_index - 1].value - combos[start_index].value

    # Step 2 : Advance the sliding window
    while end_index < len(combos):
        # advance the start index, update value of elem_count if necessary
        if visited[combos[start_index].pos] == 1:
            elem_count -= 1
        visited[combos[start_index].pos] -= 1
        start_index += 1

        # Try to bring back the position count to n
        while elem_count < len(arr) and end_index < len(combos):
            if visited[combos[end_index].pos] == 0:
                elem_count += 1

            visited[combos[end_index].pos] += 1
            end_index += 1

        # Ensure that you still have all n positions in the window
        # and update the answer
        if elem_count == len(arr):
            ans: int = min(combos[end_index - 1].value - combos[start_index].value, ans)
        else:
            # If this block executes, this means that we couldn't include all elements in our window
            # Lock final answer and exit
            break
    return ans


if __name__ == '__main__':
    arr: List[int] = [3, 9, 12, 16, 20]

    a = minimize_height_difference(arr, 3)
    print(a)
```
> References: 
https://practice.geeksforgeeks.org/problems/minimize-the-heights3351/1#
https://youtu.be/X2TufR5vY98
https://youtu.be/EHCGAZBbB88

{.is-info}
