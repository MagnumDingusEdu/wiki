---
title: Reverse String
description: 
published: true
date: 2021-05-20T14:14:42.822Z
tags: 
editor: markdown
dateCreated: 2021-05-20T13:13:37.750Z
---

# Reverse a String
Write a function that reverses a string. The input string is given as an array of characters s.

## Examples
```python
# Example 1:
Input: s = ["h","e","l","l","o"]
Output: ["o","l","l","e","h"]

# Example 2:
Input: s = ["H","a","n","n","a","h"]
Output: ["h","a","n","n","a","H"]
```

## Constraints
* `1 <= s.length <= 105`
* `s[i] is a printable ascii character`
* `Space complexity : O(1)`
* `Time complexity: O(n)`

## Solution
```python
from typing import List


def reverse_string(string: str):
    string = list(string)  # Convert to mutable object

    # Iterate through first half, swap elements from the second half
    for i in range(len(string) // 2):
        string[i], string[len(string) - i - 1] = string[len(string) - i - 1], string[i]

    return "".join(string)


# Python specific
def reverse_string_python(string: str):
    return "".join(reversed(string))


# Using recursion
def rev_string_recursively(string: str):
    if len(string) <= 1:
        return string
    return string[-1] + rev_string_recursively(string[1:-1]) + string[0]


if __name__ == '__main__':
    test_str = "Hannah"
    print(rev_string_recursively(test_str))

```

> References: https://leetcode.com/problems/reverse-string/
{.is-info}
