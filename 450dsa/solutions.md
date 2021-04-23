---
title: 450 DSA Solutions
description: 
published: true
date: 2021-04-23T14:48:19.035Z
tags: 
editor: markdown
dateCreated: 2021-04-23T14:45:47.129Z
---

# Reverse the array
```python
from typing import List


def iterative_reverse(a: List[int]) -> List[int]:
    for i in range(len(a) // 2):
        start_index, end_index = i, len(a) - 1 - i

        # Swap elements from both the front and back
        a[start_index], a[end_index] = a[end_index], a[start_index]

    return a


def recursive_reverse(a: List[int]) -> List[int]:
    # if array is empty (base case)
    if not a:
        return []

    # add first element to end after reversing the rest of the list
    first_element, reversed_array = a[0], recursive_reverse(a[1:])

    return reversed_array + [first_element]


# Python specific
def direct_reverse(a: List[int]) -> List[int]:
    return a[::-1]
    # or a.reverse()


if __name__ == '__main__':
    arr = [1, 2, 3, 4, 5, 6, 7]
    arr = iterative_reverse(arr)
    print(arr)
```

# Find minimum / maximum in array
```python
from typing import List

if __name__ == '__main__':
    arr: List[int] = [1, 2, 3, 4, 5, 6, 7]

    minimum, maximum = arr[0], arr[0]

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