---
title: Sorting Algorithms
description: 
published: true
date: 2021-01-22T06:37:25.104Z
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

## Shell Sort
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

// calculate the appropriate gap using the Hibbard algorithm
// ref : https://en.wikipedia.org/wiki/Shellsort#Gap_sequences
int hValue(int size) {
    int k = 0;
    int current = 0;

    // formula for sequence : 2^k - 1
    // calculate largest number in sequence less than array size
    while (true) {
        if ((2 << k) - 1 < size) {
            current = (2 << k) - 1;
            k++;
        } else {
            break;
        }
    }
    return current;
}

// compare two values, and return true if swapped
template<typename E>
bool compare_and_swap(E *first, E *second) {
    // do not swap if already in ascending
    if (*first < *second) return false;

    // else swap
    E temp = *second;
    *second = *first;
    *first = temp;
    return true;
}


template<typename E>
void shellSort(E *arr, int size) {
    // iterate over all elements, divide gap by 2 with each pass
    // initial value of gap : floor(size/2)
    for (int gap = size / 2; gap > 0; gap /= 2) {

        // iterate through subarray, at positions separated by gaps
        // start at 0 + gap size
        for (int j = gap; j < size; ++j) {
            int current = j;

            while (current >= gap)
                if (compare_and_swap(&arr[current - gap],
                                     &arr[current])) {
                    current -= gap;
                } else break;
        }
    }
}

int main() {
    int arr[] = {4, 13, 30, 12, 17, 26, 5, 2, 13, 16, 19, 19, 25, 11, 9, 6, 3, 11, 15, 26};
    int size = sizeof(arr) / sizeof(int);
    shellSort(arr, size);
    display(arr, size);

}
```

## Quick Sort
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

// swap two values
template<typename E>
void swap(E *fp, E *sp) {
    E temp = *sp;
    *sp = *fp;
    *fp = temp;
}

// partition the array into two sub-arrays, and return the pivot
template<typename E>
int partition(E arr[], int start, int end) {
    // choose mid point as the pivot
    int pivot = start + (end - start) / 2;

    // store the value of pivot in a temp variable
    E pivotValue = arr[pivot];

    // place the pivot at the end to avoid conflict
    swap(&arr[pivot], &arr[end]);

    int beginning = start;
    int ending = end - 1; // as the last element is the pivot

    while (true) {

        // ignore elements already at the correct positions
        while (arr[beginning] < pivotValue)
            beginning++;
        while (arr[ending] > pivotValue)
            ending--;

        // traversal complete
        if (beginning >= ending) break;

        // swap elements if both are in the wrong position
        // (pass both while conditions)
        swap(&arr[beginning], &arr[ending]);
    }

    // re-swap the pivot back to it's correct place
    swap(&arr[beginning], &arr[end]);
    return beginning;
}

template<typename E>
void quickSort(E *arr, int start, int end) {

    // check if we have reached the end
    if (start >= end) return;

    // partition the array into two halves
    // and place pivot into correct spot
    int pivot = partition(arr, start, end);

    // recursively sort the two remaining halves
    quickSort(arr, start, pivot - 1);
    quickSort(arr, pivot + 1, end);

}

int main() {
    int arr[] = {4, 13, 30, 12, 17, 26, 5, 2, 13, 16, 19, 19, 25, 11, 9, 6, 3, 11, 15, 26};
    int size = sizeof(arr) / sizeof(int);
    quickSort(arr, 0, size - 1);
    display(arr, size);

}
```
## Merge Sort
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
void merge(E *arr, int start, int middle, int end) {

    // size of the two sub-arrays
    int sizeLeft = middle - start + 1;
    int sizeRight = end - middle;

    // allocate two temporary sub-arrays
    auto arrLeft = new E[sizeLeft];
    auto arrRight = new E[sizeRight];

    // fill the temporary sub-arrays
    for (int i = 0; i < sizeLeft; ++i)
        arrLeft[i] = arr[start + i];
    for (int i = 0; i < sizeRight; ++i)
        arrRight[i] = arr[middle + i + 1];

    // merge temporary arrays to real array
    int lIndex = 0, rIndex = 0, mainIndex = start;
    while (lIndex < sizeLeft && rIndex < sizeRight) {
        if (arrLeft[lIndex] <= arrRight[rIndex]) arr[mainIndex++] = arrLeft[lIndex++];
        else arr[mainIndex++] = arrRight[rIndex++];
    }

    // fill in remaining elements at the end
    while (lIndex < sizeLeft) arr[mainIndex++] = arrLeft[lIndex++];
    while (rIndex < sizeRight) arr[mainIndex++] = arrRight[rIndex++];
}

template<typename E>
void mergeSort(E *arr, int start, int end) {
    // check if array length is 1 or zero
    // base case
    if (start >= end) return;

    // choose the mid point
    int middle = start + (end - start) / 2;

    // sort both halves of the arrays
    mergeSort(arr, start, middle);
    mergeSort(arr, middle + 1, end);

    merge(arr, start, middle, end);


}

int main() {
    int arr[] = {12, 11, 13, 5, 6, 7 };
    int size = sizeof(arr) / sizeof(int);
    mergeSort(arr, 0, size - 1);
    display(arr, size);

}
```

## Counting Sort
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
void countingSort(E arr[], int size) {
    // edge case
    if (size <= 0) return;

    // find the min and max elements
    int min = arr[0], max = arr[0];

    for (int i = 0; i < size; ++i) {
        if (arr[i] < min) min = arr[i];
        if (arr[i] > max) max = arr[i];
    }

    // calculate the range of values
    int range = max - min + 1;

    // allocate a frequency  array and initialize all entries to zero
    auto frequency = new unsigned int[range]();

    // loop through the original array and fill in the frequency values
    for (int i = 0; i < size; ++i)
        frequency[arr[i] - min]++;

    // accumulate the counts
    for (int i = 1; i < range; ++i)
        frequency[i] += frequency[i - 1];


    // allocate a temporary array for storing the sorted values
    auto sorted = new E[size];

    // fill the sorted array in reverse order
    // based on the frequency values
    for (int i = size - 1; i >= 0; --i)
        sorted[--frequency[arr[i] - min]] = arr[i];


    // copy the values over to the original array
    for (int i = 0; i < size; ++i)
        arr[i] = sorted[i];

    // de-allocate the arrays
    delete[] frequency;
    delete[] sorted;
}


int main() {
    int arr[] = {3, 20, 15, 12, 6, 9, 22, 13, 25, 26, 13, 13, 17, 16, 18, 7, 20, 10, 26, 28};
    int size = sizeof(arr) / sizeof(int);
    countingSort(arr, size);
    display(arr, size);

}
```