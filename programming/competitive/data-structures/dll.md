---
title: Doubly Linked List
description: 
published: true
date: 2021-01-09T04:06:51.034Z
tags: 
editor: markdown
dateCreated: 2021-01-09T04:06:51.034Z
---

# C++

```cpp
#include <bits/stdc++.h>

using namespace std;

template<typename E>
class DNode {
public:
    E data;
    DNode<E> *next{};
    DNode<E> *prev{};

    DNode() = default;

    explicit DNode(const E &data, DNode<E> *n = nullptr, DNode<E> *p = nullptr) {
        this->data = data;
        this->next = n;
        this->prev = p;
    }
};

template<typename E>
class DoublyLinkedList {
private:
    DNode<E> *header;
    DNode<E> *trailer;
protected:
    void remove(DNode<E> *n);

    void addBefore(DNode<E> *n, const E &data);

public:
    DoublyLinkedList();;

    ~DoublyLinkedList();;

    [[nodiscard]] bool isEmpty() const;

    const E &front();

    const E &back();

    inline void addFront(const E &data) {
        addBefore(this->header->next, data);
    }

    inline void addBack(const E &data) {
        addBefore(this->trailer, data);
    }

    void removeFront() {
        remove(this->header->next);
    }

    void removeBack() {
        remove(this->trailer->prev);
    }


    friend ostream &operator<<(ostream &os, const DoublyLinkedList &linkedList) {
        if (linkedList.isEmpty()) {
            return os;
        }
        auto temp = linkedList.header->next;

        while (true) {
            os << temp->data << " ";
            if (temp->next != linkedList.trailer)
                temp = temp->next;
            else
                break;
        }

        return os;
    }


};

template<typename E>
void DoublyLinkedList<E>::remove(DNode<E> *n) {
    DNode<E> *prev = n->prev;
    DNode<E> *next = n->next;

    prev->next = next;
    next->prev = prev;

    delete n;
}

template<typename E>
void DoublyLinkedList<E>::addBefore(DNode<E> *n, const E &data) {
    auto newNode = new DNode<E>(data);
    newNode->next = n;
    newNode->prev = n->prev;
    n->prev->next = newNode;
    n->prev = newNode;

}

template<typename E>
DoublyLinkedList<E>::DoublyLinkedList() {
    this->header = new DNode<E>;            // sentinels
    this->trailer = new DNode<E>;
    this->header->next = this->trailer;
    this->trailer->prev = this->header;
}

template<typename E>
DoublyLinkedList<E>::~DoublyLinkedList() {
    while (!isEmpty())
        removeFront();

    delete this->header;
    delete this->trailer;
}

template<typename E>
bool DoublyLinkedList<E>::isEmpty() const {
    return this->header->next == this->trailer;
}

template<typename E>
const E &DoublyLinkedList<E>::front() {
    return this->header->next->data;
}

template<typename E>
const E &DoublyLinkedList<E>::back() {
    return this->trailer->prev->data;
}

int main() {
    DoublyLinkedList<string> dll;
    dll.addFront("hello");
    dll.addFront("world");

    dll.removeBack();
    dll.removeFront();
    dll.addBack("324");
    cout << dll;
    return 0;
}
```