---
title: Balanced Parentheses Problem
description: 
published: true
date: 2021-01-13T07:32:37.601Z
tags: 
editor: markdown
dateCreated: 2021-01-13T07:32:37.601Z
---

# Solution (using stack)
```cpp
#include <bits/stdc++.h>

using namespace std;

char oppositePair(const char &val) {
    switch (val) {
        case ')':
            return '(';
        case '}':
            return '{';
        case ']':
            return '[';
        case '>':
            return '<';
        default:
            return '\0';
    }
}

bool checkBalanced(const char *str) {
    stack<char> ss;
    char val;
    for (int i = 0; str[i]; ++i) {
        val = str[i];
        switch (val) {
            case '(':
            case '[':
            case '{':
            case '<':
                ss.push(val);
                break;
            case ')':
            case ']':
            case '}':
            case '>': {
                if (ss.top() == oppositePair(val) && !ss.empty()) ss.pop();
                else return false;
                break;
            }
            default:
                break;
        }
    }
    return true;
}

int main() {
    cout << checkBalanced(string("[()]{}{[()())]()}").c_str());
    return 0;
}
```