---
title: Sorting Algorithms
description: 
published: true
date: 2021-01-22T02:40:29.017Z
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
## Insertion Sort
```cpp
#include <bits/stdc++.h>

#define DEBUG
using namespace std;

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
void insertionSort(E arr[], int size) {

    // start at 1, we assume first item is already at the correct position
    for (int i = 1; i < size; ++i) {
        int current = arr[i];

        int j;
        // loop through sorted subarray, and shift all greater items to the right
        for (j = i - 1; j >= 0 && arr[j] > current; --j)
            arr[j + 1] = arr[j];

        // store current item at correct position
        arr[j + 1] = current;

        // debugging information
#ifdef DEBUG
        cout << "Iteration #" << i + 1 << endl;
        display(arr, size);
#endif
    }


}

int main() {
    int arr[] = {1, 2, 2, 7 , 3};
    int size = sizeof(arr) / sizeof(int);
    insertionSort(arr, size);
    display(arr, size);
}
```