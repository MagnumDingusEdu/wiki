---
title: Count and Say
description: 
published: true
date: 2021-05-20T13:32:02.905Z
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

## Constraints
* `1 <= n <= 30`

## Solution
```python
```
> References: https://leetcode.com/problems/count-and-say/
{.is-info}
