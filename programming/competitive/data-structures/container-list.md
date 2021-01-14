---
title: Container/List
description: 
published: true
date: 2021-01-14T14:48:44.960Z
tags: 
editor: markdown
dateCreated: 2021-01-14T14:48:44.960Z
---

# CPP

```cpp
#include <bits/stdc++.h>

using std::exception;
using std::string;
using std::cout;
using std::cin;
using std::endl;

template<typename E>
class NodeList {
private:
    class Node {
    public:
        E data;
        Node *prev;
        Node *next;

    };

public:
    class Iterator {
    public:
        // overload the dereference operator to return actual value.
        E &operator*() { return value->data; }

        // check for equality
        bool operator==(const Iterator &p) const { return this->value == p.value; }

        // check for inequality
        bool operator!=(const Iterator &p) const { return this->value != p.value; }

        // move iterator forward by one element
        Iterator &operator++() {
            value = value->next;
            return *this;
        }

        // move iterator back one element
        Iterator &operator--() {
            value = value->prev;
            return *this;
        }

        friend class NodeList;

    private:
        Node *value;

        // constructor from node*
        explicit Iterator(Node *u) {
            this->value = u;
        }


    };

private:
    int n;          // number of items
    Node *header;   // head-of-list sentinel
    Node *trailer;  // tail-of-list sentinel

public:
    // initialize the list
    NodeList() {
        // initially size is 0
        n = 0;

        // create sentinels
        header = new Node;
        trailer = new Node;

        // have them point to each other
        header->next = trailer;
        trailer->prev = header;
    }

    [[nodiscard]] inline int size() const { return n; }

    [[nodiscard]] inline bool empty() const { return n == 0; }

    [[nodiscard]] Iterator begin() const {
        return Iterator(header->next);
    }

    [[nodiscard]] Iterator end() const {
        return Iterator(trailer);
    }

    // insert before specified node
    // p is the iterator, e is the element to be inserted
    void insert(const Iterator &p, const E &e) {
        // save the current links in local variables
        Node *current = p.value;
        Node *prev = current->prev;

        // allocate a new node
        Node *newNode = new Node;

        // initialize and link the new node
        newNode->data = e;
        newNode->next = current;
        newNode->prev = prev;

        // remake old links
        current->prev = newNode;
        prev->next = newNode;

        // update the new size
        n++;
    }

    // insert at the front of the list
    void insertFront(const E &e) {
        insert(begin(), e);
    }

    // insert at the end of the list
    void insertEnd(const E &e) {
        insert(end(), e);
    }

    // remove a specified element
    void erase(const Iterator &p) {
        // save current values in local variables
        Node *current = p.value;
        Node *next = current->next;
        Node *prev = current->prev;

        // unlink the current node
        prev->next = next;
        next->prev = prev;

        // deallocate the current node
        delete current;

        // update the new size
        n--;
    }

    // remove the first element
    void eraseFront() { erase(begin()); }

    // remove the last element
    void eraseEnd() { erase(--end()); }

};


int main() {

    NodeList<int> nl;

    nl.insertFront(3);
    nl.insertFront(4);
    nl.insertEnd(22);
    nl.insertEnd(21);
    nl.eraseFront();
    nl.eraseEnd();

    for (auto i : nl) {
        cout << i << " ";
    }


    return 0;

}
```