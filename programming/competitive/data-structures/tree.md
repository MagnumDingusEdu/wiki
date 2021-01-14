---
title: Tree
description: 
published: true
date: 2021-01-14T10:46:51.736Z
tags: 
editor: markdown
dateCreated: 2021-01-14T10:46:51.736Z
---

# CPP
Generic Templated Tree 
```cpp
#include <bits/stdc++.h>

using namespace std;

template<typename E>
class TreeNode {
public:
    E data;
    vector<TreeNode<E> *> children;

    explicit TreeNode(E data) {
        this->data = data;
    }

    ~TreeNode() {
        for (auto &i : this->children) delete i;
    }
};

template<typename E>
void printTree(TreeNode<E> *root) {
    if (root == nullptr) return; // edge case
    cout << root->data << " : ";
    for (TreeNode<E> *&i : root->children) {
        cout << i->data << ", ";
    }
    cout << endl;
    for (auto &i : root->children) {
        printTree(i);
    }
}

template<typename E>
TreeNode<E> *takeInput() {
    cout << "Enter data: ";
    int rootData;
    cin >> rootData;
    auto root = new TreeNode<E>(rootData);

    int n;
    cout << "Enter number of children of " << rootData << ": ";
    cin >> n;
    for (int i = 0; i < n; ++i) {
        auto child = takeInput<E>();
        root->children.push_back(child);
    }
    return root;
}

template<typename E>
TreeNode<E> *takeLevelWiseInput() {
    int rootData;
    cout << "Enter root data: " << endl;
    cin >> rootData;

    auto root = new TreeNode<E>(rootData);

    queue<TreeNode<E> *> qq; // pending nodes

    qq.push(root);
    while (!qq.empty()) {
        auto front = qq.front();
        qq.pop();
        cout << "Enter number of children of " << front->data << ": ";
        int numChild;
        cin >> numChild;
        for (int i = 0; i < numChild; ++i) {
            int childData;
            cout << "Enter " << i << " child of " << front->data << ": ";
            cin >> childData;
            auto temp = new TreeNode<E>(childData);
            front->children.push_back(temp);
            qq.push(temp);
        }
    }
    return root;

}

template<typename E>
void printTreeLevelWise(TreeNode<E> *root) {
    queue<TreeNode<E> *> qq; // queue of pending nodes
    qq.push(root);

    while (!qq.empty()) {
        TreeNode<E> *front = qq.front();
        qq.pop();
        cout << front->data << " : ";
        for (TreeNode<E> *&i: front->children) {
            qq.push(i);
            cout << i->data << ", ";
        }
        cout << endl;

    }
}

template<typename E>
int numberOfNodes(TreeNode<E> *root) {
    int total = 1;
    for (auto &i : root->children) {
        total += numberOfNodes(i);
    }
    return total;
}

template<typename E>
int heightOfTree(TreeNode<E> *root) {
    int maxHeight = 0;
    for (auto &i : root->children) {
        int h = heightOfTree(i);
        if (h > maxHeight) maxHeight = h;
    }
    return maxHeight + 1;

}

template<typename E>
void printAllNodesAtLevelK(TreeNode<E> *root, int k) {
    if (root == nullptr) return;
    if (k == 0) {
        cout << root->data << endl;
        return;
    }
    for (auto &i : root->children) {
        printAllNodesAtLevelK(i, k - 1);
    }
}

template<typename E>
int countLeafNodes(TreeNode<E> *root) {
    if (root == nullptr) return 0;
    if (root->children.empty()) return 1;
    int total = 0;
    for (auto &i : root->children) {
        total += countLeafNodes(i);
    }
    return total;
}

template<typename E>
void printUsingPreOrder(TreeNode<E> *root) {
    if (root == nullptr) return;
    cout << root->data << " ";

    for (auto &i : root->children) {
        printUsingPreOrder(i);
    }
}

template<typename E>
void postOrderTraversal(TreeNode<E> *root) {
    if (root == nullptr) return;


    for (auto &i : root->children) {
        postOrderTraversal(i);
    }
    cout << root->data << " ";
}

template<typename E>
void deleteEntireTree(TreeNode<E> *root) {
    if (root == nullptr) return;
    for (auto &i : root->children) {
        deleteEntireTree(i);
    }
    delete root;
}

int main() {
//    auto root = new TreeNode<int>(3);
//    auto node1 = new TreeNode<int>(4);
//    auto node2 = new TreeNode<int>(5);
//
//    root->children.push_back(node1);
//    root->children.push_back(node2);
    auto root = takeLevelWiseInput<int>();

    printTreeLevelWise(root);
    cout << "Number of nodes: " << numberOfNodes<int>(root) << endl;
    cout << "Height: " << heightOfTree<int>(root) << endl;
    cout << "pre order :";
    printUsingPreOrder<int>(root);
    cout << "post order: ";
    postOrderTraversal<int>(root);
    delete root;
}

// 1 3 2 3 4 2 5 6 2 7 8 0 0 0 0 1 9 0

```