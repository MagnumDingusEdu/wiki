---
title: Find Duplicates
description: 
published: true
date: 2021-05-20T14:25:12.856Z
tags: 
editor: markdown
dateCreated: 2021-05-20T13:20:39.312Z
---

# Print all the duplicates in the input string
Write an efficient program to print all the duplicates and their counts in the input string 

## Examples
```python
# Input : 
"test string"

# Output:
's',  count = 2
't',  count = 3
```

## Solution
```python
from typing import List
from collections import Counter


# Print duplicates
# Time Complexity : O(n)
# Space complexity : O(unique_elements)
def print_unique(s: str):
    freq_map = Counter(s) # Generate map automatically
    print(*[(a, v) for a, v in freq_map.items() if v > 1])


if __name__ == '__main__':
    test_str = "geeksforgeeks"
    print_unique(test_str)

```
> References: https://www.geeksforgeeks.org/print-all-the-duplicates-in-the-input-string/
{.is-info}
