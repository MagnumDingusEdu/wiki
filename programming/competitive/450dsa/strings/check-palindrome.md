---
title: Check Palindrome
description: 
published: true
date: 2021-05-20T14:22:01.531Z
tags: 
editor: markdown
dateCreated: 2021-05-20T13:16:51.558Z
---

# Check if Palindrome String
Given a string S, check if it is palindrome or not.

## Examples
```python
# Example 1
Input: S = "abba"
Output: 1

# Example2
Input: S = "abc" 
Output: 0
```

## Constraints
* `Expected Time Complexity: O(n) `
* `Expected Auxiliary Space: O(1)` 
* `1 <= Length of S <= 105`

## Solution
```python
from typing import List


# Iterative
def check_palindrome(s: str):
    is_palindrome = True
    for i in range(len(s) // 2):
        if s[i] != s[len(s) - i - 1]:
            is_palindrome = False
            break

    return is_palindrome


# recursive
def check_palin_recur(s: str):
    if len(s) <= 1:
        return 1

    return int(s[0] == s[-1] and check_palin_recur(s[1:-1]))


if __name__ == '__main__':
    test_str = "a" * 1000
    print(check_palin_recur(test_str))

```
> References: https://practice.geeksforgeeks.org/problems/palindrome-string0817/1
{.is-info}
