---
title: Stack
description: 
published: true
date: 2021-01-12T21:47:15.688Z
tags: 
editor: markdown
dateCreated: 2021-01-12T21:47:15.688Z
---

# C++

Stack is implemented here in two different ways, one using fixed-size arrays, and the other using a linked list. We can also use the STL Vector class instead of arrays, for an unbounded array-based stack.

## Array Based
```cpp
#include <bits/stdc++.h>
#include <ostream>

using namespace std;


template<typename E>
class ArrayStack {
private:
    enum {
        DEFAULT_CAPACITY = 100
    };
public:
    friend ostream &operator<<(ostream &os, const ArrayStack &arrayStack) {
        ArrayStack temp = arrayStack;
        while (!temp.empty()) {
            os << temp.top() << " ";
            temp.pop();
        }
        return os;
    }

private:

    E *arr;
    int capacity{};
    int topIndex{};


public:
    explicit ArrayStack(int cap = DEFAULT_CAPACITY) {
        this->arr = new E[cap];
        this->capacity = cap;
        this->topIndex = -1;
    }


public:

    // public functions
    [[nodiscard]] int size() const {
        return this->topIndex + 1;
    }

    [[nodiscard]] bool empty() const {
        return this->topIndex < 0;
    }

    [[nodiscard]] const E &top() const {
        return this->arr[topIndex];
    }

    void push(const E &e) {
        if (this->size() == this->capacity) return;
        this->arr[++topIndex] = e;
    }

    void pop() {
        if (this->empty()) return;
        --topIndex;
    }


};



int main() {
    ArrayStack<int> test;
    test.push(3);
    test.push(4);
    test.push(3243);
    test.push(69);
    test.pop();
    test.push(11);
    cout << test;
}
```