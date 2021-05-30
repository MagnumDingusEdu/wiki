---
title: Fibonacci
description: 
published: true
date: 2021-05-30T15:15:20.328Z
tags: 
editor: markdown
dateCreated: 2021-05-30T15:15:20.328Z
---

# Print the first n fibonacci numbers

Efficient approach using memorization
```cpp
#include <bits/stdc++.h>
#define int long long
using namespace std;


int recursiveHelper(int n, map<int, int> &saved) {
    if (saved.count(n) > 0) {
        return saved[n];
    }
    if (n == 0)
        return 0;
    if (n == 1)
        return 1;
    if (n == 2)
        return 1;
    int answer = recursiveHelper(n - 1, saved) + recursiveHelper(n - 2, saved);
    saved[n] = answer;
    return answer;
}

int efficientFibonacci(int n) {
    map<int, int> saved;
    return recursiveHelper(n, saved);
}

int32_t main() {
    int n;
    cin >> n;

    for (int i = 0; i < n; ++i) {
        cout << efficientFibonacci(i) << endl;
    }
    return 0;
}
```