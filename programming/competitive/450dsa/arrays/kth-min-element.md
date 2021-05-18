---
title: K’th Min / Max Elem
description: 
published: true
date: 2021-05-18T12:39:38.159Z
tags: 
editor: markdown
dateCreated: 2021-05-18T12:39:38.159Z
---

# Find the Kth smallest element in an array
Given an array and a number k where k is smaller than the size of the array, we need to find the k’th smallest element in the given array. It is given that all array elements are distinct.

## Examples
```cpp
Input: arr[] = {7, 10, 4, 3, 20, 15}

       k = 3

Output: 7



Input: arr[] = {7, 10, 4, 3, 20, 15}

       k = 4

Output: 10
```

## Solution
```python
from typing import List, Optional
from operator import gt, lt
import operator
import random

INT_MAX = float("inf")
INT_MIN = float("-inf")


############## HELPER CLASSES ##################
class Heap:
    _array: List[int]
    oper: operator

    def __init__(
            self,
            min_heap=True,
            max_heap=False,
            build_from_arr: Optional[List[int]] = None,
    ):
        self._array = []
        if min_heap and not max_heap:
            self.oper = lt
        else:
            self.oper = gt

        if build_from_arr:
            self._build_from_arr(build_from_arr)

    # O(n) average time : builds heap inplace
    def _build_from_arr(self, array: List[int]):
        # Step 1 : Ignore leaf nodes as they can't dissatisfy heap property
        # Only call sift_down for first n/2 nodes

        self._array = array

        last_non_leaf_node: int = (self.size() // 2) - 1

        while last_non_leaf_node >= 0:
            self._sift_down(last_non_leaf_node)
            last_non_leaf_node -= 1

    # O(1)
    def get_top_elem(self) -> int:
        return self.array[0]

    # O(1)
    def is_empty(self) -> bool:
        return len(self._array) == 0

    # O(1)
    def size(self) -> int:
        return len(self._array)

    def _swap(self, i1: int, i2: int) -> None:
        self._array[i1], self._array[i2] = self._array[i2], self._array[i1]

    # O(logn)
    def insert(self, val: int) -> None:
        self._array.append(val)
        self._sift_up()

    # O(logn)
    def pop(self) -> int:
        self._swap(0, self.size() - 1)
        val: int = self._array.pop()
        self._sift_down()

        return val

    # O(logn)
    def _sift_up(self) -> None:
        child_index: int = len(self._array) - 1
        parent_index: int = (child_index - 1) // 2

        # Heapify
        while parent_index >= 0:
            # Check if parent > child or vice versa
            if self.oper(self._array[child_index], self._array[parent_index]):
                # Swap elements
                self._swap(parent_index, child_index)

                child_index = parent_index
                parent_index = (child_index - 1) // 2
            else:
                break

    # generally more efficient O(logn) worst case
    def _sift_down(self, index=0) -> None:
        parent_index: int = index
        lchild_index: int = 2 * parent_index + 1
        rchild_index: int = 2 * parent_index + 2

        while lchild_index < self.size():
            r_exists: bool = rchild_index < self.size()
            min_index: int

            if r_exists:
                if self.oper(self._array[lchild_index], self._array[rchild_index]):
                    min_index = lchild_index
                else:
                    min_index = rchild_index
            else:
                min_index = lchild_index

            if self.oper(self.array[parent_index], self._array[min_index]):
                break
            else:
                self._swap(parent_index, min_index)

                parent_index = min_index
                lchild_index: int = 2 * parent_index + 1
                rchild_index: int = 2 * parent_index + 2

    @property
    def array(self) -> List[int]:
        return self._array


# Method one
# Sort the array, return the k'th element from the start
# Time Complexity : O(nlogn) [best case]
# Space complexity : O(n) [we have to build new sorted array]
def find_kth_sort(array: List[int], k: int, largest: bool = False):
    array = sorted(array, reverse=largest)  # Sort in reverse order if finding largest
    return array[k - 1]


# Method two
# Build a min / max heap and extract smallest / largest k times
# Time complexity : O(n) [build heap] + O(klogn) [extract elem] = O(n+klogn)
# Space complexity : O(1)
def find_kth_heap(array: List[int], k: int, largest: bool = False):
    heap = Heap(max_heap=largest, build_from_arr=array)

    # Handle duplicates
    prev = None
    current = None
    while k > 0 and not heap.is_empty():
        current = heap.pop()
        if current == prev:
            continue
        else:
            prev = current
            k -= 1
    return current


# Method three (objectively best method)
# Quick select, modified quick sort with similar partition function
# Time complexity : O(n) average case, O(n^2) worst case
# Space complexity : O(1)
def find_kth_quick_select(array: List[int], k: int, largest: bool = False):
    def partition(array: List[int], l_index: int, r_index: int) -> int:
        # Choose the last element as the pivot
        pivot: int = array[r_index]

        # Partition around the pivot, with all lesser elements on the left
        i = l_index

        for j in range(l_index, r_index):
            if array[j] <= pivot:
                array[i], array[j] = array[j], array[i]
                i += 1

        # Put the pivot in it's correct position
        array[i], array[r_index] = array[r_index], array[i]

        # Return the index of the pivot
        return i

    def recursive_helper(l_index: int, r_index: int, k: int):

        array_size = r_index - l_index + 1

        if 0 < k <= array_size:
            pivot: int = partition(array, l_index, r_index)

            # check if we already have the correct element
            if pivot - l_index + 1 == k:
                return array[pivot]
            # Search in the right subarray
            elif pivot - l_index + 1 < k:
                return recursive_helper(pivot + 1, r_index, k - (pivot - l_index + 1))
            # Search in the left subarray
            elif pivot - l_index + 1 > k - l_index:
                return recursive_helper(l_index, pivot - 1, k)

        else:
            return INT_MAX

    return recursive_helper(0, len(array) - 1, k)


if __name__ == "__main__":

    # Generate random non-duplicate array

    arr = set()
    arr2 = set()
    for i in range(1000):
        val = random.randint(1, 100000)
        arr.add(val)
        arr2.add(val)

    arr = list(arr)
    arr2 = list(arr2)

    print(find_kth_sort(arr, 200), find_kth_heap(arr2, 200), find_kth_quick_select(arr, 200))

```