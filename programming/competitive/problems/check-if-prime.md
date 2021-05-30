---
title: Check if prime
description: 
published: true
date: 2021-05-30T14:13:39.576Z
tags: 
editor: markdown
dateCreated: 2021-05-30T14:13:39.576Z
---

# Check whether a given number is prime
## Cpp
```cpp
#include <bits/stdc++.h>

using namespace std;

// Time complexity : O(n)
// Space complexity : O(1)
bool checkPrimeBruteForce(int n) {
    // corner case
    if (n <= 1)
        return true;

    // check for all numbers < n if it divides
    for (int i = 2; i < n; ++i) {
        if (n % i == 0) {
            return false;
        }
    }

    return true;
}

// optimized check

// Step 1
// only check for i < root(n)
// This holds true for all n: all unique divisors of n
// are numbers less than or equal to √n

// Step 2
// all prime numbers except 2 and 3 are in the form of 6k ± 1
bool checkPrimeOptimized(int n) {
    if (n <= 3)
        return true;

    // return false if not in the form of 6k±1
    if (!((n % 6 == 5) || (n % 6 == 1))) {
        return false;
    }

    int rootN = ceil(sqrt(n));
    for (int i = 2; i < rootN; ++i) {
        if (n % i == 0) {
            return false;
        }
    }

    return true;
}

int main() {
    int t;

    cin >> t;

    while (t--) {
        int n;
        cin >> n;
        cout << (checkPrimeOptimized(n) ? "prime" : "not prime") << endl;
    }

    return 0;
}
```