---
title: Vector
description: 
published: true
date: 2021-01-14T14:00:57.974Z
tags: 
editor: markdown
dateCreated: 2021-01-14T14:00:57.974Z
---

# CPP
```cpp
#include <bits/stdc++.h>

using namespace std;

class RuntimeException : public exception {
private:
    string errorMsg;
public:
    explicit RuntimeException(const string &err) {
        this->errorMsg = err;
    }

    [[nodiscard]] const char *what() const noexcept override {
        return errorMsg.c_str();
    }
};


template<typename E>
class ArrayVector {
private:
    int capacity{};   // current size of the array
    int n{};          // number of elements in the vector

    E *data;        // array for storing the data

public:
    ArrayVector() {
        this->capacity = 0;
        this->n = 0;
        this->data = nullptr;
    }

    [[nodiscard]] int size() const {
        return this->n;
    }

    [[nodiscard]] bool empty() const {
        return size() == 0;
    }

    // overloading the [] operator
    E &operator[](int i) {
        return this->data[i];
    }

    E &at(int i) noexcept(false) {
        if (i < 0 || i > n) throw RuntimeException("illegal index in function 'at()'");
        return this->data[i];
    }

    // shifts all elements down
    void erase(int i) {
        for (int j = i + 1; j < n; ++j) {
            this->data[j - 1] = this->data[j];
        }
        n--;
    }

    // insert element at specified position
    void insert(int position, const E &e) {
        // check for overflow, and double the capacity if full
        if (n >= capacity) reserve(max(1, 2 * capacity));

        // reverse for loop to move elements up
        for (int j = n - 1; j >= position; --j) {
            this->data[j + 1] = this->data[j];
        }

        // insert the element
        this->data[position] = e;

        // update the element count
        n++;
    }

    // pre-reserve more space for vector
    void reserve(int size) {
        // check if we already have that capacity
        if (capacity >= n && capacity != 0) return;

        // allocate a bigger array
        E *newArray = new E[size];

        // move elements from old to new array
        for (int i = 0; i < n; ++i) {
            newArray[i] = this->data[i];
        }

        // deallocate old array
        delete[] this->data;

        // set data to new array
        data = newArray;

        // update the capacity
        capacity = size;


    }

};

int main() {

    ArrayVector<int> vv;

    // reserve space for 5 elements
    vv.reserve(5);

    vv.insert(0, 10);
    vv.insert(1, 20);

    cout << vv.at(0) << endl << vv.at(1) << endl;

    vv.erase(0);

    cout << vv.at(0) << endl ;

    // should throw exception
    //vv.at(10);


    return 0;

}
```