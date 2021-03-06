---
title: Singly Linked List
description: 
published: true
date: 2021-01-22T01:23:53.147Z
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
        if(this->isEmpty()) return;

        if (this->head->next == nullptr) {
            this->head = nullptr;
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
            this->head->next = nullptr;
            return;
        }

        newNode->next = this->head;
        this->head = newNode;
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


int main() {
    SinglyLinkedList<string> sll;
    sll.addFront("hello");
    sll.addFront("world");
    cout << sll;
    return 0;
}
```

# Python

```python
import typing


class SNode:
    def __init__(self, data, nex=None):
        self.data = data
        self.next: typing.Optional[SNode] = nex


class SinglyLinkedList:

    def __init__(self) -> None:
        self.head: typing.Optional[SNode] = None

    def is_empty(self) -> bool:
        return self.head is None

    def front(self):
        return self.head.data

    def remove_front(self) -> None:
        if self.head is not None:
            temp = self.head.next
            del self.head
            self.head = temp

    def add_front(self, data):
        new_node = SNode(data)
        new_node.next = self.head
        self.head = new_node

    def __repr__(self):
        if self.is_empty():
            return ""

        temp: SNode = self.head
        output: str = ""
        while True:
            output += str(temp.data) + " "
            if temp.next is not None:
                temp = temp.next
            else:
                break
        return output

    def __del__(self):
        while not self.is_empty():
            self.remove_front()


if __name__ == '__main__':
    sll = SinglyLinkedList()
    sll.add_front(2)
    sll.add_front(3)
    sll.add_front(4)
    print(sll)
    del sll

```