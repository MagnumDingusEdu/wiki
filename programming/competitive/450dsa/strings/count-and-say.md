---
title: Count and Say
description: 
published: true
date: 2021-05-20T15:15:03.237Z
tags: 
editor: markdown
dateCreated: 2021-05-20T13:32:02.905Z
---

# Count and Say
The count-and-say sequence is a sequence of digit strings defined by the recursive formula:

* `countAndSay(1) = "1"`
* `countAndSay(n)` is the way you would "say" the digit string from `countAndSay(n-1)`, which is then converted into a different digit string.

To determine how you "say" a digit string, split it into the **minimal** number of groups so that each group is a contiguous section all of the **same character**. Then for each group, say the number of characters, then say the character. To convert the saying into a digit string, replace the counts with a number and concatenate every saying.

For example, the saying and conversion for digit string `"3322251"`:

![countandsay.jpg](/assets/countandsay.jpg)

Given a positive integer n, return the nth term of the count-and-say sequence.

## Examples
```python
# This is the base case.
Input: n = 1
Output: "1"
```

```python
Input: n = 4
Output: "1211"

# Explanation:
# countAndSay(1) = "1"
# countAndSay(2) = say "1" = one 1 = "11"
# countAndSay(3) = say "11" = two 1's = "21"
```

```python
# nth sequence is as follows
 1.     1
 2.     11
 3.     21
 4.     1211
 5.     111221 
 6.     312211
 7.     13112221
 8.     1113213211
 9.     31131211131221
 10.    13211311123113112211
```

## Constraints
* `1 <= n <= 30`

## Solution
```python
def count_and_say(num: int):
    def rec_helper(num: int):

        num = str(num)

        current = num[0]
        i = 1
        while i < len(num) and current == num[i]:
            i += 1

        if i == len(num):
            return str(i) + current

        return str(i) + current + rec_helper(int(num[i:]))

    return int(rec_helper(num))


def nth_sequence(num: int):
    if num <= 1:
        return 1
    previous = nth_sequence(num - 1)
    return count_and_say(previous)


if __name__ == '__main__':
    a = nth_sequence(4)
    print(a)

```
> References: https://leetcode.com/problems/count-and-say/
{.is-info}
