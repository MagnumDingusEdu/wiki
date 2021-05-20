---
title: Check if rotated
description: 
published: true
date: 2021-05-20T14:29:58.369Z
tags: 
editor: markdown
dateCreated: 2021-05-20T13:24:25.840Z
---

# A Program to check if strings are rotations of each other or not
Given a string s1 and a string s2, write a snippet to say whether s2 is a rotation of s1?

## Examples
```python
# Input
s1 = 'ABCD'
s2 = 'CDAB'

OUTPUT: True

# Input
s1 = 'ABCD'
s2 = 'ACBD'

OUTPUT: False
```

## Solution
```python
# Check rotation
def check_if_strings_are_rotated(s1: str, s2: str):
    return s2 in s1 + s1 and s1 in s2 + s2


if __name__ == '__main__':
    print(check_if_strings_are_rotated("abcd", "bcda"))

```

> References: https://www.geeksforgeeks.org/a-program-to-check-if-strings-are-rotations-of-each-other/
{.is-info}
