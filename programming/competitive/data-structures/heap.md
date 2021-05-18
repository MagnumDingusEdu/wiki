---
title: Heap / PQ
description: 
published: true
date: 2021-05-18T10:27:13.135Z
tags: 
editor: markdown
dateCreated: 2021-05-18T10:27:13.135Z
---

# Python	
```python
from typing import List


class Heap:
    _array: List[int]

    def __init__(self):
        self._array = []

    def is_empty(self) -> bool:
        return len(self._array) == 0

    def size(self) -> int:
        return len(self._array)

    def _swap(self, i1: int, i2: int) -> None:
        self._array[i1], self._array[i2] = self._array[i2], self._array[i1]

    def insert(self, val: int) -> None:
        ...

    def pop(self) -> int:
        ...

    @property
    def array(self) -> List[int]:
        return self._array


class MinHeap(Heap):
    def get_min(self) -> int:
        return self._array[0]

    def _heapify_bottom(self) -> None:
        child_index: int = len(self._array) - 1
        parent_index: int = (child_index - 1) // 2

        # Heapify
        while parent_index >= 0:
            if self._array[child_index] < self._array[parent_index]:
                # Swap elements
                self._swap(parent_index, child_index)

                child_index = parent_index
                parent_index = (child_index - 1) // 2
            else:
                break

    def _heapify_top(self) -> None:
        parent_index: int = 0
        lchild_index: int = 2 * parent_index + 1
        rchild_index: int = 2 * parent_index + 2

        while lchild_index < self.size():
            r_exists: bool = rchild_index < self.size()
            min_index: int

            if r_exists:
                if self._array[lchild_index] < self._array[rchild_index]:
                    min_index = lchild_index
                else:
                    min_index = rchild_index
            else:
                min_index = lchild_index

            if self.array[parent_index] <= self._array[min_index]:
                break
            else:
                self._swap(parent_index, min_index)

                parent_index = min_index
                lchild_index: int = 2 * parent_index + 1
                rchild_index: int = 2 * parent_index + 2

    def insert(self, val: int):
        self._array.append(val)
        self._heapify_bottom()

    def pop(self):

        self._swap(0, self.size() - 1)
        min_val: int = self._array.pop()
        self._heapify_top()

        return min_val


class MaxHeap(Heap):
    def get_max(self) -> int:
        return self._array[0]

    def _heapify_bottom(self) -> None:
        child_index: int = len(self._array) - 1
        parent_index: int = (child_index - 1) // 2

        # Heapify
        while parent_index >= 0:
            if self._array[child_index] > self._array[parent_index]:
                # Swap elements
                self._swap(parent_index, child_index)

                child_index = parent_index
                parent_index = (child_index - 1) // 2
            else:
                break

    def _heapify_top(self) -> None:
        parent_index: int = 0
        lchild_index: int = 2 * parent_index + 1
        rchild_index: int = 2 * parent_index + 2

        while lchild_index < self.size():
            r_exists: bool = rchild_index < self.size()
            min_index: int

            if r_exists:
                if self._array[lchild_index] > self._array[rchild_index]:
                    min_index = lchild_index
                else:
                    min_index = rchild_index
            else:
                min_index = lchild_index

            if self.array[parent_index] >= self._array[min_index]:
                break
            else:
                self._swap(parent_index, min_index)

                parent_index = min_index
                lchild_index: int = 2 * parent_index + 1
                rchild_index: int = 2 * parent_index + 2

    def insert(self, val: int):
        self._array.append(val)
        self._heapify_bottom()

    def pop(self):

        self._swap(0, self.size() - 1)
        max_val: int = self._array.pop()
        self._heapify_top()

        return max_val


if __name__ == '__main__':
    heap = MaxHeap() # Or minHeap

    arr: List[int] = [37, 67, 18, 42, 38, 69, 79, 49, 71, 43, 82, 78, 77, 20, 51, 1, 2]
    # arr: list[int] = [37, 67, 18, 42, 38]
    for elem in arr:
        heap.insert(elem)

    while not heap.is_empty():
        print(heap.pop())

```