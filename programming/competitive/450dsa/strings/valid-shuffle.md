---
title: Check valid shuffle
description: 
published: true
date: 2021-05-20T14:47:22.616Z
tags: 
editor: markdown
dateCreated: 2021-05-20T13:27:05.175Z
---

# Check if a string is a valid shuffle of two distinct strings
In this example, we will check if a string is the valid shuffle of two other strings

## Examples
```python
'1XY2' is a valid shuffle of 'XY' and '12'
'Y12X' is not a valid shuffle of 'XY' and '12'
```

## Solution
```python
# Check for valid shuffle
# 3 Pointer method
def check_if_valid_shuffle(s1: str, s2: str, result: str):
    l1 = len(s1)
    l2 = len(s2)
    l3 = len(result)

    # Valid shuffle impossible if lengths not same
    if l3 != l1 + l2:
        return False

    p1 = 0  # Pointer 1
    p2 = 0  # Pointer 2
    rp = 0  # Result pointer

    while rp < l3 and p1 < l1 and p2 < l2:
        if s1[p1] == result[rp]:
            p1 += 1
            rp += 1
            continue
        elif s2[p2] == result[rp]:
            p2 += 1
            rp += 1
        else:
            return False

    return True


if __name__ == '__main__':
    print(check_if_valid_shuffle("XY", "12", "X21Y"))

```

> References: 
https://youtu.be/-rcfE1Tj2E0
https://youtu.be/-rcfE1Tj2E0
{.is-info}
