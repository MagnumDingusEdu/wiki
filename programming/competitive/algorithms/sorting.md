---
title: Sorting Algorithms
description: 
published: true
date: 2021-01-22T02:26:43.215Z
tags: 
editor: markdown
dateCreated: 2021-01-22T02:14:55.781Z
---

# CPP
## Bubble Sort
```cpp
#include <bits/stdc++.h>

#define DEBUG
using namespace std;

// swap two values
template<typename E>
void swap(E *fp, E *sp) {
    E temp = *sp;
    *sp = *fp;
    *fp = temp;
}

template<typename E>
void display(E arr[], int size) {
    stringstream ss;
    ss << "[ ";
    for (int i = 0; i < size; ++i) {
        ss << arr[i] << " ";
    }
    ss << "]";
    cout << ss.str() << endl;
}

template<typename E>
void bubbleSort(E arr[], int size) {

    bool sorted = true;
    int swapCount = 0;
    // we are looking ahead for next, so iterate through the array
    // leaving out the last element
    for (int i = 0; i < size - 1; ++i) {
        sorted = true;
        // the last i number of digits in the list will always be sorted
        for (int j = 0; j < size - 1 - i; ++j) {
            int current = j;
            int next = j + 1;
            if (arr[current] > arr[next]) {
                swap(arr + current, arr + next);
                sorted = false;
                swapCount++;
            }
        }

        // if there was no swap, the array is sorted.
        if (sorted) break;

        // debugging information
#ifdef DEBUG
        cout << "Iteration #" << i + 1 << endl;
        display(arr, size);
#endif

    }

#ifdef DEBUG
    cout << "Total Swaps : " << swapCount << endl;
#endif
}

int main() {
    int arr[] = {1, 23, 12, 9, 30, 2, 50};
    bubbleSort(arr, 7);
    display(arr, 7);
}
```

## Selection Sort
```cpp
#include <bits/stdc++.h>

#define DEBUG
using namespace std;

// swap two values
template<typename E>
void swap(E *fp, E *sp) {
    E temp = *sp;
    *sp = *fp;
    *fp = temp;
}

template<typename E>
void display(E arr[], int size) {
    stringstream ss;
    ss << "[ ";
    for (int i = 0; i < size; ++i) {
        ss << arr[i] << " ";
    }
    ss << "]";
    cout << ss.str() << endl;
}

template<typename E>
void selectionSort(E arr[], int size) {

    int min_id;
    int totalSwaps = 0;

    // move the starting boundary of the unsorted subarray on every iteration
    // no use trying to sort the last element alone, so leave one out
    for (int i = 0; i < size - 1; ++i) {

        // find minimum element in unsorted subarray
        min_id = i;
        for (int j = i + 1; j < size; ++j) {
            if (arr[j] < arr[min_id]) min_id = j;
        }

        // swap the minimum element with the start
        // of the unsorted subarray
        if (arr[i] != arr[min_id]) {
            swap(arr + i, arr + min_id);
            totalSwaps++;
        }

        // debugging information
#ifdef DEBUG
        cout << "Iteration #" << i + 1 << endl;
        display(arr, size);
#endif

    }

#ifdef DEBUG
    cout << "Total Swaps : " << totalSwaps << endl;
#endif

}

int main() {
    int arr[] = {1, 23, 12, 9, 30, 2, 50};
    selectionSort(arr, 7);
    display(arr, 7);
}
```