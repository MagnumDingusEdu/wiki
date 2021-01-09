---
title: Circular Linked List
description: 
published: true
date: 2021-01-09T05:58:38.175Z
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

# Python

```python
class CNode:
    data = None
    next = None

    def __init__(self, data, nex=None):
        self.data = data
        self.next = nex


class CircularLinkedList:
    cursor = None

    def __init__(self):
        self.cursor = None

    def is_empty(self):
        return self.cursor is None

    def back(self):
        return self.cursor.data

    def front(self):
        return self.cursor.next.data

    def advance(self):
        self.cursor = self.cursor.next

    def add(self, data):
        new_node = CNode(data)
        if self.is_empty():
            new_node.next = new_node
            self.cursor = new_node
        else:
            new_node.next = self.cursor.next
            self.cursor.next = new_node

    def remove(self):
        if self.is_empty():
            return

        old = self.cursor.next
        if old == self.cursor:
            self.cursor = None
        else:
            self.cursor.next = old.next

        del old

    def __repr__(self):
        temp = self.cursor.next
        if self.is_empty():
            return ""

        output = ""

        while True:
            output += temp.data + " "
            if temp.next != self.cursor.next:
                temp = temp.next
            else:
                break
        return output


if __name__ == '__main__':
    playlist = CircularLinkedList()
    playlist.add("Song 1")
    playlist.add("Song 2")
    playlist.add("Song 3")
    playlist.advance()
    playlist.advance()
    playlist.remove()
    playlist.add("Song 4")
    print(playlist)

    del playlist
```