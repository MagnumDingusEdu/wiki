---
title: Max Rectangle Size
description: 
published: true
date: 2021-05-19T14:46:34.535Z
tags: 
editor: markdown
dateCreated: 2021-05-19T07:21:01.724Z
---

# Maximum size rectangle
Given a binary matrix. Find the maximum area of a rectangle formed only of 1s in the given matrix. 

## Examples
```cpp
Input:
n = 4, m = 4
M[][] = [[0 1 1 0],
         [1 1 1 1],
         [1 1 1 1],
         [1 1 0 0]]
         
Output: 8


// The max size rectangle is 
// 1 1 1 1
// 1 1 1 1
// and area is 4 *2 = 8.
```

## Constraints
Expected Time Complexity : `O(n*m)`
Expected Auixiliary Space : `O(m)`
```cpp
1<=n,m<=1000
0<=M[][]<=1
```

## Solution
```python
from typing import List
from bisect import bisect_left
from collections import deque


# Function to find largest area under histogram
# Three possible approaches :
# > brute force > divide and conquer (recursive) > stack based
# This is stack based solution, O(n) complexity
# O(n) because each element will be pushed once, and finally
# be popped once at some point in the algorithm.
def find_largest_area_in_histogram(histogram: List[int]) -> int:
    # Maintain stack of bar indexes
    stack = deque()

    max_area_so_far = 0

    for index, bar in enumerate(histogram):

        while not len(stack) == 0 and histogram[stack[-1]] > bar:
            # Store the top index and pop it
            popped = stack.pop()

            # Since this is an increasing stack,
            # the first smaller element to the right
            # of the popped element is the current element.
            # On the left side, it's the (new) top element of the stack

            # if stack is empty, this means this is the
            # smallest value we've seen so far
            if len(stack) == 0:
                current_area = histogram[popped] * index

            # Get the width from the top of the stack till current, and
            # multiply it with the popped height to get area
            else:
                current_area = histogram[popped] * (index - (stack[-1] + 1))

            # update the max area
            max_area_so_far = max(current_area, max_area_so_far)

        # Append the new element on to the array
        # array will be non-decreasing
        stack.append(index)

    while not len(stack) == 0:
        # if elements are still left, pop them one by one
        # this means there wasn't a smaller element than any of these
        top_index = stack.pop()

        # calculate area to N as we have reached the end
        if len(stack) == 0:
            current_area = histogram[top_index] * len(histogram)
        else:
            current_area = histogram[top_index] * (len(histogram) - (stack[-1] + 1))

        # update max_area
        max_area_so_far = max(max_area_so_far, current_area)

    return max_area_so_far


# Find largest rect in matrix
# builds a histogram after adding each row
# checks max rect for each histogram
# Time complexity : O(n)*O(m) = O(nm)
# Space complexity : O(n) [to maintain histogram]
def find_largest_rect_in_matrix(mat: List[List[int]]) -> int:
    current_histogram = mat[0]
    max_area_so_far = find_largest_area_in_histogram(current_histogram)

    for row in mat[1:]:
        for i, val in enumerate(row):
            # Break the bar if current row has 0
            # else add it to the current histogram height
            if val != 0:
                current_histogram[i] += val
            
            else:
                current_histogram[i] = 0

        current_area = find_largest_area_in_histogram(current_histogram)
        max_area_so_far = max(max_area_so_far, current_area)

    return max_area_so_far


if __name__ == '__main__':
    # print(find_largest_area_in_histogram([1, 2, 3, 1, 2, 3]))
    ...
    A = [[0, 1, 1, 0],
         [1, 1, 1, 1],
         [1, 1, 1, 1],
         [1, 1, 0, 0]]
    ans = find_largest_rect_in_matrix(A)
    print(ans)

```

> References: 
[Largest Rectangle in histogram animation](/largest_rect_histogram.mp4)
[Explanation of the histogram method](https://youtu.be/vcv3REtIvEo)
[How to use the histogram method in matrix](https://youtu.be/dAVF2NpC3j4)
https://practice.geeksforgeeks.org/problems/max-rectangle/1
{.is-info}
