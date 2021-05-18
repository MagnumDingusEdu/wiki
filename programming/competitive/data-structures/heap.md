---
title: Heap / PQ
description: 
published: true
date: 2021-05-18T11:35:53.870Z
tags: 
editor: markdown
dateCreated: 2021-05-18T10:27:13.135Z
---

# Python	
```python
from typing import List, Optional
from operator import gt, lt
import operator


class Heap:
    _array: List[int]
    oper: operator

    def __init__(self, min_heap=True, max_heap=False, build_from_arr: Optional[List[int]] = None):
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


if __name__ == '__main__':

    arr: List[int] = [37, 67, 18, 42, 38, 69, 79, 49, 71, 43, 82, 78, 77, 20, 51, 1, 2]

    # Build in place in O(n) time
    heap = Heap(max_heap=False, build_from_arr=arr)  # Or minHeap

    # Build one by one in O(nlogn) time
    # Space complexity = O(n)

    # arr: list[int] = [37, 67, 18, 42, 38]
    # for elem in arr:
    #     heap.insert(elem)
    while not heap.is_empty():
        print(heap.pop())
```