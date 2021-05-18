---
title: Find the Min / Max Element of an Array
description: 
published: true
date: 2021-05-18T05:39:22.464Z
tags: 
editor: markdown
dateCreated: 2021-05-18T05:39:22.464Z
---

# Maximum and minimum of an array
Write a C function to return minimum and maximum in an array. Your program should make the minimum number of comparisons. 

## Examples
```cpp

// input
arr[] = { 1000, 11, 445, 1, 330, 3000 };

// output
Minimum element is 1
Maximum element is 3000
```

## Solution
```python
from typing import List

if __name__ == '__main__':
    arr: List[int] = [1, 2, 3, 4, 5, 6, 7]

    minimum, maximum = arr[0], arr[0]

    # O(n)
    for val in arr:
        if val < minimum:
            minimum = val
        if val > maximum:
            maximum = val

    # Python specific
    minimum = min(arr)
    maximum = max(arr)

    print(minimum, maximum)
```

> Link: https://www.geeksforgeeks.org/maximum-and-minimum-in-an-array/
{.is-info}
