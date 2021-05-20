---
title: Longest Palindrome
description: 
published: true
date: 2021-05-20T16:20:58.429Z
tags: 
editor: markdown
dateCreated: 2021-05-20T13:35:13.900Z
---

# Longest Palindrome in a String
Given a string S, find the longest palindromic substring in S. Substring of string `S: S[ i . . . . j ]` where `0 ≤ i ≤ j < len(S)`. 
Palindrome string: A string which reads the same backwards. More formally, `S` is palindrome if `reverse(S) = S`. Incase of conflict, return the substring which occurs first ( with the least starting index).

## Examples
```python
Input:
S = "aaaabbaa"
Output: 'aabbaa'
# Explanation: The longest Palindromic
# substring is "aabbaa".
```
```python
Input: 
S = "abc"
Output: a
# Explanation: "a", "b" and "c" are the 
# longest palindromes with same length.
# The result is the one with the least
# starting index.
```

## Constraints
* `Expected Time Complexity: O(|S|^2).`
* `Expected Auxiliary Space: O(1).`
* `1 ≤ |S| ≤ 10^3`

## Solution
> Python solution was going overtime, while the C++ solution was well within time
(for the DP based solution)
{.is-warning}
### C++
```cpp
#include <iostream>
#include <bits/stdc++.h>

using namespace std;

string longest_palindrome(string s) {
    int n = s.length();

    int dp[n][n];

    for (int i = 0; i < n; ++i) {
        for (int j = 0; j < n; ++j) {
            dp[i][j] = 0;
        }
    }

    int max_so_far = 0;
    int max_x_index = -1;
    int max_y_index = -1;

    for (int i = 0; i < n; ++i) {
        dp[i][i] = 1;
        if (1 > max_so_far) {
            max_so_far = 1;
            max_x_index = i;
            max_y_index = i;
        }
    }


    for (int i = 0; i < n - 1; ++i) {
        if (s[i] == s[i + 1]) {


            dp[i][i + 1] = 1;


            if (2 > max_so_far) {
                max_so_far = 2;
                max_x_index = i;
                max_y_index = i + 1;
            }
        }
    }


    for (int j = 2; j < n; ++j) {
        for (int i = 0; i < n - 2; i++) {
            if (j + i < n) {
                int x = i;
                int y = i + j;
                if (s[x] == s[y] and dp[x + 1][y - 1]) {
                    if (y - x + 1 > max_so_far) {
                        max_so_far = y - x + 1;
                        max_x_index = x;
                        max_y_index = y;
                    }
                    dp[x][y] = 1;

                }
            }
        }
    }


    return s.substr(max_x_index, max_y_index - max_x_index + 1);
}

int main() {
    cout << longest_palindrome("abcd");
}
```
### Python
```python
def longest_palindrome(s: str) -> str:
    n = len(s)

    # We will only use upper half of this DP
    dp = [[0 for _ in range(n)] for _ in range(n)]

    max_so_far = 0
    max_indeces = None

    # all diagonal elements are of length 1, so fill in 1s
    for i in range(n):
        dp[i][i] = 1
        if 1 > max_so_far:
            max_so_far = 1
            max_indeces = (i, i)

    # for all substr of length 2, if start == end, it is palindrome
    for i in range(n - 1):
        if s[i] == s[i + 1]:

            dp[i][i + 1] = 1

            # Update max_so_far
            if 2 > max_so_far:
                max_so_far = 2
                max_indeces = (i, i + 1)

    # for all other substrings
    # if start == end and middle part is also palindrome
    # add 1 to dp
    for j in range(2, n):
        for i in range(n - 2):
            if j + i < n:
                x, y = i, j + i
                if s[x] == s[y] and dp[x + 1][y - 1]:
                    if y - x + 1 > max_so_far:
                        max_so_far = y - x + 1
                        max_indeces = (x, y)
                    dp[x][y] = 1

    # print dp for debug purposes
    for row in dp:
        print(*row)

    return s[max_indeces[0]:max_indeces[1] + 1]


if __name__ == '__main__':
    a = longest_palindrome("aaaabbaa")
    print(a)

```

> References: 
https://youtu.be/UflHuQj6MVA
https://practice.geeksforgeeks.org/problems/longest-palindrome-in-a-string3411/1
{.is-info}
