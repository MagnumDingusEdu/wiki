---
title: Queue
description: 
published: true
date: 2021-01-13T08:47:17.342Z
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

## Dynamic Array Based
```cpp
#include <bits/stdc++.h>
#include <ostream>

using namespace std;


template<typename E>
class QueueWithDynamicArray {
private:
    long int currentSize{};
    long int firstIndex{};
    long int nextIndex{};
    E *data;

    long int capacity{};

    void doubleCapacity() {
        E *newData = new E[2 * capacity];
        auto fi = firstIndex;
        auto s = size();

        if (s == 0) {
            this->data = newData;
            return;
        }
        int i = 0;
        while (s > 0) {
            newData[i++] = this->data[fi];
            fi = (fi + 1) % capacity;
            s--;
        }

        delete[] this->data;
        this->data = newData;
        this->firstIndex = 0;
        this->nextIndex = i;
        this->capacity = 2 * this->capacity;


    }

public:
    friend ostream &operator<<(ostream &os, const QueueWithDynamicArray &withArray) {
        auto fi = withArray.firstIndex;
        auto size = withArray.size();

        while (size > 0) {
            os << withArray.data[fi] << " ";
            fi = (fi + 1) % withArray.capacity;
            size--;
        }
        return os;
    }

private:
    enum {
        INITIAL_CAPACITY = 5
    };

public:
    explicit QueueWithDynamicArray(long int capacity = INITIAL_CAPACITY) {
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
        if (currentSize == capacity) doubleCapacity();

        if (isEmpty()) {
            firstIndex = 0;
            nextIndex = 1;
            data[firstIndex] = e;
            currentSize++;
            return;
        } else {
            this->data[nextIndex] = e;
            this->data[nextIndex] = e;
            nextIndex = (nextIndex + 1) % capacity;

            currentSize++;
        }


    }

    E dequeue() {
        if (isEmpty()) {
            cout << "Queue empty" << endl;
            firstIndex = -1;
            nextIndex = 0;
            exit(1);
        }
        E val = data[firstIndex];
        firstIndex = (firstIndex + 1) % capacity;
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

    long int cap() { return capacity; }
};

int main() {
    QueueWithDynamicArray<int> qq;
    for (int i = 0; i < 100; ++i) {
        qq.enqueue(i + 1);
    }
    cout << qq << "|" << qq.cap() << endl;

}
```

## SLL Based
```cpp
#include <bits/stdc++.h>
#include <ostream>

using namespace std;


template<typename E>
class SNode {
public:
    E data;
    SNode<E> *next;

    explicit SNode(const E &data, SNode<E> *n = nullptr) {
        this->data = data;
        this->next = n;
    }
};

template<typename E>
class SinglyLinkedList {
private:
    SNode<E> *head;
    SNode<E> *tail;

public:
    SinglyLinkedList() {
        this->head = nullptr;
        this->tail = nullptr;
    };

    ~SinglyLinkedList() {
        while (!isEmpty())
            removeFront();
    };

    [[nodiscard]] bool isEmpty() const { return this->head == nullptr; }

    void removeFront() {
        if (this->isEmpty()) return;

        if (this->head->next == nullptr) {
            this->head = nullptr;
            this->tail = nullptr;
            return;
        }
        SNode<E> *temp = this->head->next;
        delete this->head;
        this->head = temp;
    }

    [[nodiscard]] const E &front() const { return this->head->data; }

    void addFront(const E &data) {
        auto *newNode = new SNode<E>(data);

        if (this->isEmpty()) {
            this->head = newNode;
            this->tail = newNode;
            this->head->next = nullptr;

            return;
        }

        newNode->next = this->head;
        this->head = newNode;
    }

    void pushBack(const E &data) {
        auto *newNode = new SNode<E>(data);
        if (this->isEmpty()) {
            this->head = newNode;
            this->tail = newNode;
            this->head->next = nullptr;

            return;
        }
        this->tail->next = newNode;
        this->tail = newNode;

    }

    friend std::ostream &operator<<(std::ostream &os, const SinglyLinkedList &linkedList) {
        if (linkedList.isEmpty()) {
            return os;
        }
        auto temp = linkedList.head;

        while (true) {
            os << temp->data << " ";
            if (temp->next != nullptr)
                temp = temp->next;
            else
                break;
        }

        return os;
    }


};


template<typename E>
class QueueWithSLL {
    SinglyLinkedList<E> data;
    long int currentSize{};

public:
    QueueWithSLL() {
        this->data = SinglyLinkedList<E>();
        this->currentSize = 0;

    }

    [[nodiscard]] bool isEmpty() const { return this->data.isEmpty(); }

    [[nodiscard]] E &front() const { return this->data.front(); }

    [[nodiscard]] long int size() const { return this->currentSize; }

    void enqueue(const E &e) {
        this->data.pushBack(e);
        this->currentSize++;
    }

    E dequeue() {
        E val = this->data.front();
        this->data.removeFront();
        this->currentSize--;
        return val;
    }

    friend ostream &operator<<(ostream &os, const QueueWithSLL &sll) {
        os << sll.data;
        return os;
    }


};

int main() {
    QueueWithSLL<int> qq;
    for (int i = 0; i < 100; ++i) {
        qq.enqueue(i + 1);
        if(i % 6 == 0) qq.dequeue();
    }
    cout << qq  << endl;

}
```