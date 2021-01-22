---
title: Searching Algorithms
description: 
published: true
date: 2021-01-22T01:52:39.055Z
tags: 
editor: markdown
dateCreated: 2021-01-22T01:52:39.055Z
---

# CPP
## Linear Search
```cpp
// returns the index of the element searched, -1 if not found
template<typename E>
int linearSearch(E arr[], int size, E elem) {
    for (int i = 0; i < size; ++i)
        if (arr[i] == elem) return i;
    return -1;
}
```


## Binary Search

* Iterative

```cpp
template<typename E>
int binarySearchIterative(E arr[], int l, int r, E elem) {
    while (l <= r) {
        int mid = l + (r - l) / 2;

        // if the middle element is correct, return
        if (arr[mid] == elem) return mid;

        // ignore the half that is greater than the element
        if (arr[mid] > elem)
            r = mid - 1;
        else
            l = mid + 1;
    }

    // element not present
    return -1;
}

```
* Recursive
```cpp
template<typename E>
int binarySearchRecursive(E arr[], int l, int r, E elem) {
    if (r < l) return -1; // not present in the array

    int mid = l + (r - l) / 2;

    // if the middle element is correct, return
    if (arr[mid] == elem) return mid;

    // check if element is in left subarray
    if (arr[mid] > elem)
        return binarySearchRecursive(arr, l, mid - 1, elem);
    else
        return binarySearchRecursive(arr, mid + 1, r, elem);
}
```