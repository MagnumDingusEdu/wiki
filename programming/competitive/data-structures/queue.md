---
title: Queue
description: 
published: true
date: 2021-01-13T08:08:35.045Z
tags: 
editor: markdown
dateCreated: 2021-01-13T08:08:35.045Z
---

# CPP
## Fixed Array Based

```cpp
#include <bits/stdc++.h>
#include <ostream>

using namespace std;


template<typename E>
class QueueWithArray {
private:
    long int currentSize{};
    long int firstIndex{};
    long int nextIndex{};
    E *data;

    long int capacity{};
public:
    friend ostream &operator<<(ostream &os, const QueueWithArray &withArray) {
        auto ni = withArray.nextIndex;
        auto fi = withArray.firstIndex;
        auto size = withArray.size();

        while (size > 0) {
            os << withArray.data[fi] << " ";
            fi = fi % withArray.capacity + 1;
            size--;
        }
        return os;
    }

private:
    enum {
        INITIAL_CAPACITY = 10
    };

public:
    explicit QueueWithArray(long int capacity = INITIAL_CAPACITY) {
        this->capacity = capacity;
        this->nextIndex = 0;
        this->firstIndex = -1;
        this->currentSize = 0;
        this->data = new E[capacity];
    }

    [[nodiscard]] bool isEmpty() const {
        return this->currentSize == 0;
    }

    [[nodiscard]] long int size() const {
        return this->currentSize;
    }

    void enqueue(const E &e) {
        if (currentSize == capacity) {
            cout << "Queue full";
            return;
        } else if (isEmpty()) {
            firstIndex = 0;
            nextIndex = 1;
            data[firstIndex] = e;
            currentSize++;
            return;
        } else {
            nextIndex = (nextIndex % capacity) + 1;
            this->data[nextIndex - 1] = e;
            currentSize++;
        }


    }

    E dequeue() {
        if (isEmpty()) {
            cout << "Queue empty" << endl;
            exit(1);
        }
        E val = data[firstIndex];
        firstIndex = (firstIndex % capacity) + 1;
        currentSize--;
        return val;
    }

    E &front() {
        if (isEmpty()) {
            cout << "Queue is empty";
            exit(1);
        }
        return this->data[firstIndex];
    }
};

int main() {
    QueueWithArray<int> qq;
    qq.enqueue(3);
    qq.enqueue(4);
    qq.enqueue(2);
    qq.enqueue(24);
    qq.dequeue();
    qq.enqueue(69);
    cout << qq << endl;
    cout <<
         qq.size() <<
         endl <<
         qq.front() << endl;
}
```