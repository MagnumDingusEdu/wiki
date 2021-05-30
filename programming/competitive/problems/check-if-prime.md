---
title: Prime Numbers
description: 
published: true
date: 2021-05-30T14:32:02.812Z
tags: 
editor: markdown
dateCreated: 2021-05-30T14:13:39.576Z
---

# Check if prime / Count number of prime numbers
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

    // sqrt is expensive, directly use i*i instead
    //    int rootN = ceil(sqrt(n));

    for (int i = 2; i * i <= n; ++i) {
        if (n % i == 0) {
            return false;
        }
    }

    return true;
}

// brute force method
int countPrimes(int n) {
    int counter = 0;
    for (int i = 2; i < n; ++i) {
        if (checkPrimeOptimized(i)) {
            counter++;
        }
    }
    return counter;
}

//  Sieve of Eratosthenes
int countPrimesOptimized(int n) {

    // populate table with by default prime
    auto primeArray = new bool[n + 1];
    for (int i = 0; i < n + 1; ++i) {
        primeArray[i] = true;
    }

    // go through the numbers between 2 and √n
    // mark off all multiples from p^2 to n as non-prime
    for (int i = 2; i * i <= n; ++i) {
        // if already marked as non-prime, all multiples will be marked already
        if (!primeArray[i])
            continue;

        // mark off all multiples as non-prime
        for (int j = i * i; j < n; j += i) {
            primeArray[j] = false;
        }

    }

    //  Sieve of Eratosthenes complete
    // count number of primes less than n
    int counter = 0;
    for (int i = 2; i < n; ++i) {
        if (primeArray[i])
            counter++;
    }
    return counter;

}

int main() {
    cout << countPrimesOptimized(5000000) << endl;
    return 0;
}
```