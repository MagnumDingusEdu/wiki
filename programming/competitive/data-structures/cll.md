---
title: Circular Linked List
description: 
published: true
date: 2021-01-09T05:48:08.648Z
tags: 
editor: markdown
dateCreated: 2021-01-09T05:48:08.648Z
---

# C++

```cpp
#include <bits/stdc++.h>
#include <ostream>

using namespace std;

template<typename E>
class CNode {
public:
    E data;
    CNode<E> *next{};

    CNode() = default;

    explicit CNode(const E &data, CNode<E> *n = nullptr) {
        this->data = data;
        this->next = n;
    }
};

template<typename E>
class CircularLinkedList {
private:
    CNode<E> *cursor;
public:

    CircularLinkedList() : cursor(nullptr) {}

    ~CircularLinkedList() { while (!isEmpty()) remove(); }

    [[nodiscard]] bool isEmpty() const {
        return this->cursor == nullptr;
    }

    [[nodiscard]] const E &back() const {
        return this->cursor->data;
    }

    [[nodiscard]] const E &front() const {
        return this->cursor->next->data;
    }

    void advance() {
        this->cursor = this->cursor->next;
    }

    void add(const E &data) {
        auto *newNode = new CNode<E>(data);
        if (isEmpty()) {
            newNode->next = newNode;
            this->cursor = newNode;
        } else {
            newNode->next = this->cursor->next;
            this->cursor->next = newNode;
        }
    }

    void remove() {
        if (isEmpty()) {
            return;
        }
        CNode<E> *old = this->cursor->next;
        if (old == this->cursor)
            this->cursor = nullptr;
        else
            this->cursor->next = old->next;

        delete old;
    }

    friend ostream &operator<<(ostream &os, const CircularLinkedList &circleList) {
        CNode<E> *temp = circleList.cursor->next;
        if (circleList.isEmpty()) {
            os << "";
            return os;
        }
        while (true){
            os << temp->data << " ";
            if (temp->next != circleList.cursor->next)
                temp = temp->next;
            else
                break;
        }
        return os;
    }

};

int main() {
    CircularLinkedList<string> playlist;
    playlist.add("Song 1");
    playlist.add("Song 2");
    playlist.add("Song 3");
    playlist.advance();
    playlist.advance();
    playlist.remove();
    playlist.add("Song 4");

    // song 4 song 3 song 2
    cout << playlist << endl;
}
```
