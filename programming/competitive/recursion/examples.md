---
title: Recursion Examples
description: 
published: true
date: 2021-01-10T11:41:56.009Z
tags: 
editor: markdown
dateCreated: 2021-01-10T11:23:55.425Z
---

# Drawing a Ruler


```cpp
#include <bits/stdc++.h>

using namespace std;

// helper function to draw one tick
void drawOneTick(unsigned int length, int value = -1) {
    stringstream ss;
    for (int i = 0; i < length; ++i) ss << "-";
    if (value >= 0)
        ss << " " << value;
    cout << ss.str() << endl;
}

// draws the ticks in between a given interval
void drawTicksRecursive(unsigned int tickLength) {
    if (tickLength > 0) {
        drawTicksRecursive(tickLength - 1);
        drawOneTick(tickLength);
        drawTicksRecursive(tickLength - 1);
    }
}

// draws the entire ruler
void drawRuler(unsigned int nInches, unsigned int majorLength) {
    drawOneTick(majorLength, 0);
    for (int i = 1; i <= nInches; ++i) {
        drawTicksRecursive(majorLength - 1);
        drawOneTick(majorLength, i);

    }
}


int main() {
    drawRuler(5, 3);
}
```

Output :

```cpp
--- 0
-
--
-
--- 1
-
--
-
--- 2
-
--
-
--- 3
-
--
-
--- 4
-
--
-
--- 5
```

# Sum of Array

```cpp
// return the sum of the first n elements of arr
int arraySum(int *arr, int n) {
    if (n == 0) return 0;
    if (n == 1) return arr[0];
    return arr[n - 1] + arraySum(arr, n - 1);
}
```

# Reverse an array
```cpp
// reverse all the elements in A starting at index i and ending at j
void reverseArray(int *arr, int i, int j) {
    if (i == j) return; // odd number of elements
    if (i > j) return; // even number of elements
    if (i < j) {
        int temp = arr[i];
        arr[i] = arr[j];
        arr[j] = temp;
        reverseArray(arr, i + 1, j - 1);
    }
}
```