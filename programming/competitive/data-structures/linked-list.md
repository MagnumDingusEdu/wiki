---
title: Singly Linked List
description: 
published: true
date: 2021-01-09T04:06:02.976Z
tags: 
editor: markdown
dateCreated: 2021-01-09T04:01:54.713Z
---


# C++


```cpp
#include <bits/stdc++.h>

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

public:
    SinglyLinkedList() {
        this->head = nullptr;
    };

    ~SinglyLinkedList() {
        while (!isEmpty())
            removeFront();
    };

    [[nodiscard]] bool isEmpty() const { return this->head == nullptr; }

    void removeFront() {
        SNode<E> *temp = this->head->next;
        delete this->head;
        this->head = temp;
    }

    [[nodiscard]] const E &front() const { return this->head->data; }

    void addFront(const E &data) {
        auto *newNode = new SNode<E>(data);
        newNode->next = this->head;
        this->head = newNode;
    }

    friend ostream &operator<<(ostream &os, const SinglyLinkedList &linkedList) {
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

int main() {
    SinglyLinkedList<string> sll;
    sll.addFront("hello");
    sll.addFront("world");
    cout << sll;
    return 0;
}
```

