---
title: Sorting Algorithms
description: 
published: true
date: 2021-01-22T09:59:22.908Z
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

        // handle edge case, getting stuck in while loop when values same
        if (arr[beginning] == arr[ending]) beginning++;

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
    int arr[] = {18, 15, 5, 1, 20, 25, 20, 5, 1, 18, 12, 13, 22, 3, 30, 19, 18, 13, 20, 22};
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
    
    // deallocate the arrays
    delete[] arrLeft;
    delete[] arrRight;
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

## Radix Sort
```cpp
#include <bits/stdc++.h>

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


// special case of counting sort while specifying the place to use as key
// any other stable sort can be used in it's place
template<typename E>
void countingSortExp(E *arr, int size, int place_exp) {
    // edge case
    if (size <= 0) return;

    // find the max element
    E max = (arr[0] / place_exp) % 10;

    for (int i = 0; i < size; ++i) {
        E current = (arr[i] / place_exp) % 10;
        if (current > max) max = current;
    }

    // calculate the range of values
    int range = max + 1;

    // allocate a frequency  array and initialize all entries to zero
    auto frequency = new unsigned int[range]();

    // loop through the original array and fill in the frequency values
    for (int i = 0; i < size; ++i)
        frequency[(arr[i] / place_exp) % 10]++;

    // accumulate the counts
    for (int i = 1; i < range; ++i)
        frequency[i] += frequency[i - 1];


    // allocate a temporary array for storing the sorted values
    auto sorted = new E[size];

    // fill the sorted array in reverse order
    // based on the frequency values
    for (int i = size - 1; i >= 0; --i)
        sorted[--frequency[((arr[i] / place_exp) % 10)]] = arr[i];


    // copy the values over to the original array
    for (int i = 0; i < size; ++i)
        arr[i] = sorted[i];

    // de-allocate the arrays
    delete[] frequency;
    delete[] sorted;
}

template<typename E>
void radixSort(E *arr, int size) {
    // edge case
    if (size == 0) return;

    // find the max element
    int max = arr[0];
    for (int i = 0; i < size; ++i)
        if (arr[i] > max) max = arr[i];

    // loop through the positions of the highest number
    // i will go from 1, 10, 100 ... till the largest possible value
    for (int i = 1; max / i > 0; i *= 10)
        countingSortExp(arr, size, i);

}

int main() {
    int arr[] = {3, 20, 15, 12, 6, 9, 22, 13, 25, 26, 13, 13, 17, 16, 18, 7, 20, 10, 26, 28};
    int size = sizeof(arr) / sizeof(int);
    radixSort(arr, size);
    display(arr, size);

}
```
# Python
## Bubble Sort
```python
import typing

def bubble_sort(array: typing.List[int]):
    # Get the size of the array
    size: int = len(array)

    iteration_count = 0
    swapped_count = 0

    for i in range(size):

        swapped = False
        iteration_count += 1

        # Loop through the array, leaving i elements from the end
        # as they are guaranteed to be sorted after each pass
        for j in range(1, size - i):
            if array[j] < array[j - 1]:
                # swap elements if in incorrect order
                array[j], array[j - 1] = array[j - 1], array[j]
                swapped = True
                swapped_count += 1

        if not swapped:
            break

    # print(f"{swapped_count} swaps, {iteration_count} iterations")


if __name__ == '__main__':
    arr: typing.List[int] = [3, 20, 15, 12, 6, 9, 22, 13, 25, 26, 13, 13, 17, 16, 18, 7, 20, 10, 26, 28]
    bubble_sort(arr)

    print(arr)

```

## Selection Sort
```python
from typing import List, Any


def selection_sort(arr: List[Any]):
    # Get the size of the array
    size: int = len(arr)

    iteration_count: int = 0

    # Loop through the array, ruling out the first value after each iteration
    for i in range(size):
        # find the minimum element
        minimum: int = i
        for j in range(i, size):
            if arr[j] < arr[minimum]:
                minimum = j

        # put the minimum element at the start
        arr[i], arr[minimum] = arr[minimum], arr[i]

        # Increase the iteration count
        iteration_count += 1

    print(f"{iteration_count} iterations")


if __name__ == '__main__':
    array: List[int] = [3, 20, 15, 12, 6, 9, 22, 13, 25, 26, 13, 13, 17, 16, 18, 7, 20, 10, 26, 28]
    selection_sort(array)

    print(array)

```
## Insertion Sort
```python
from typing import List, Any


def insertion_sort(arr: List[Any]):
    # Get the size of the array
    size: int = len(arr)

    iteration_count: int = 0

    # Loop through the array, ruling out the first value
    for i in range(1, size):
        # save the current value
        current = arr[i]

        # save the final value after shifting
        value: int = -1

        # shift all elements larger than current to the right
        for k in reversed(range(i)):
            if arr[k] > current:
                arr[k + 1] = arr[k]
                value = k

        # save the current variable at the correct position
        arr[value] = current

        # Increase the iteration count
        iteration_count += 1

    print(f"{iteration_count} iterations")


if __name__ == '__main__':
    array: List[int] = [3, 20, 15, 12, 6, 9, 22, 13, 25, 26, 13, 13, 17, 16, 18, 7, 20, 10, 26, 28]
    insertion_sort(array)

    print(array)
```
## Shell Sort
```python
from typing import List, Any


def shell_sort(arr: List[Any]):
    # Get the size of the array
    size: int = len(arr)

    pass_count: int = 0

    # Initialize gap to half of size
    gap: int = int(size / 2)

    # Iterate through the array in "gap" number of passes
    # half value of gap after each pass, stop when gap = 0
    while True:
        if gap <= 0: break
        pass_count += 1

        # Loop through gap separated sub-array, swap out of order elements
        for i in range(gap, size):
            current = i
            while current - gap >= 0 and arr[current] < arr[current - gap]:
                arr[current - gap], arr[current] = arr[current], arr[current - gap]
                current -= gap

        # Half the value of gap till it's 1
        gap = int(gap / 2)

    print(f"{pass_count} passes")


if __name__ == '__main__':
    array: List[int] = [3, 20, 15, 12, 6, 9, 22, 13, 25, 26, 13, 13, 17, 16, 18, 7, 20, 10, 26, 28]
    shell_sort(array)

    print(array)
```
# Quick Sort
```python
from typing import List, Any


def partition(arr: List[Any], first: int, last: int) -> int:
    pivot: int = first + int((last - first) / 2)

    # move pivot to the last to avoid interference
    arr[last], arr[pivot] = arr[pivot], arr[last]

    # Initialize start and end pointers
    start: int = first
    end: int = last - 1

    while True:
        # Advance pointers to the point where there are conflicting values
        while arr[start] < arr[last]:
            start += 1
        while arr[end] > arr[last]:
            end -= 1

        # Get out of infinite loop in edge case
        if arr[start] == arr[end]:
            start += 1

        # Break loop if complete
        if start >= end:
            break

        # Swap conflicting values
        arr[start], arr[end] = arr[end], arr[start]

    # bring back pivot to correct position
    arr[start], arr[last] = arr[last], arr[start]
    return start


def quick_sort(arr: List[Any], first: int, last: int) -> None:
    if first >= last:
        return

    pivot: int = partition(arr, first, last)

    quick_sort(arr, first, pivot - 1)
    quick_sort(arr, pivot + 1, last)


if __name__ == '__main__':
    array: List[int] = [18, 15, 5, 1, 20, 25, 20, 5, 1, 18, 12, 13, 22, 3, 30, 19, 18, 13, 20, 22]
    quick_sort(array, 0, len(array) - 1)

    print(array)
```
## Merge Sort
```python
from typing import List, Any


def merge(arr: List[Any], first: int, mid: int, last: int) -> None:
    left_arr = []
    right_arr = []

    for i in range(first, mid + 1):
        left_arr.append(arr[i])
    for i in range(mid + 1, last + 1):
        right_arr.append(arr[i])

    left_size = len(left_arr)
    right_size = len(right_arr)

    left_iterator = 0
    right_iterator = 0
    sorted_iterator = first

    while left_iterator < left_size and right_iterator < right_size:
        if left_arr[left_iterator] < right_arr[right_iterator]:
            arr[sorted_iterator] = left_arr[left_iterator]
            left_iterator += 1
        else:
            arr[sorted_iterator] = right_arr[right_iterator]
            right_iterator += 1
        sorted_iterator += 1

    while left_iterator < left_size:
        arr[sorted_iterator] = left_arr[left_iterator]
        sorted_iterator += 1
        left_iterator += 1
    while right_iterator < right_size:
        arr[sorted_iterator] = right_arr[right_iterator]
        right_iterator += 1
        sorted_iterator += 1


def merge_sort(arr: List[Any], first: int, last: int) -> None:
    if first >= last:
        return

    middle: int = first + int((last - first) / 2)

    merge_sort(arr, first, middle)
    merge_sort(arr, middle + 1, last)

    merge(arr, first, middle, last)


if __name__ == '__main__':
    array: List[int] = [18, 15, 5, 1, 20, 25, 20, 5, 1, 18, 12, 13, 22, 3, 30, 19, 18, 13, 20, 22]
    merge_sort(array, 0, len(array) - 1)

    print(array)
```
