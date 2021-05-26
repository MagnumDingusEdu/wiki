---
title: Find Path in Maze
description: 
published: true
date: 2021-05-26T16:29:27.698Z
tags: 
editor: markdown
dateCreated: 2021-05-26T16:29:27.698Z
---

# Path traversal in a maze
You are provided with a grid of pixels and the task is to reach at all the four corners from a single pixel inside the grid, covering the minimum number of pixels. One is allowed to move in eight directions as is shown in Fig. 1, if a connection is available. Write an algorithm to generate all the four paths.

<center style="margin-top : 20px">
<img src="/assets/path-finding-image.jpg" alt="drawing" width="200"/>
  
  <small>Fig. 1</small>
  
</center>

For example, let there be a 5×8 grid and we have to start from pixel number 20. All the pixels are numbered in row-wise manner starting from '1', as is shown in Fig. 2. The required sequences of pixels to be followed so as to reach each of the four corners are marked in Fig. 2.

<center style="margin-top : 20px">
<img src="/assets/path-finding-image-2.jpg" alt="drawing" width="500"/>
  
  <small>Fig. 2</small>
  
</center>

You have to develop and implement the algorithm that will produce the four paths from the given starting pixel.

## Input
* Line 1 contains three space integers `M, N and S`, i.e. the size of the grid is `M×N` and starting pixel is `S`.
* Following `M×N` lines contains space separated 9 numbers. The first is an integer, say `i`, representing a pixel number followed by a sequence of eight bits `(0/1)` showing the directions one can move from pixel `i`. The order in which directions are considered in the given sequence of bits is same as shown in Fig. 1.

## Output:

There will be four lines in the output. Each line contains space separated pixel numbers that are to be followed to reach a corner from the given starting pixel.
* First line contains path to corner 1 as shown in Fig. 2.
* Second line contains path to corner 2 as shown in Fig. 2.
* Third line contains path to corner 3 as shown in Fig. 2.
* Fourth line contains path to corner 4 as shown in Fig. 2.

## Constraints
All values are in the range of `1` to `1000`

## Sample Input
```cpp
Input : 

5 8 20
1 0 0 1 1 1 0 0 0 
2 0 0 1 0 1 1 1 0 
3 0 0 1 1 0 1 1 0 
4 0 0 0 0 1 1 1 0 
5 0 0 1 1 1 0 0 0 
6 0 0 1 1 1 1 1 0 
7 0 0 1 1 0 1 1 0 
8 0 0 0 0 1 0 1 0
9 1 1 1 0 1 0 0 0 
10 1 1 0 0 0 1 1 1 
11 0 1 1 1 0 0 0 0 
12 1 0 0 0 1 1 1 1 
13 1 1 0 0 1 0 0 0 
14 1 1 1 1 0 0 0 1 
15 0 0 0 0 1 1 1 1 
16 1 0 0 0 1 0 0 1
17 1 1 0 0 1 0 0 0 
18 0 0 1 1 1 0 0 0 
19 0 1 1 1 0 1 1 0 
20 1 0 0 0 1 0 1 1 
21 1 0 0 0 1 0 0 0 
22 0 1 1 0 1 0 0 0 
23 1 0 0 0 0 1 1 1 
24 1 0 0 0 0 0 0 0
25 1 0 0 1 1 0 0 0 
26 1 1 1 1 0 0 0 0 
27 0 0 0 0 1 0 1 1 
28 1 0 0 1 1 0 0 1 
29 1 0 0 0 1 1 0 0 
30 1 1 0 1 1 0 0 0 
31 0 0 1 1 1 1 0 0 
32 0 0 0 0 1 1 1 0
33 1 0 1 0 0 0 0 0 
34 0 0 0 0 0 0 1 1 
35 1 0 0 0 0 0 0 1 
36 1 1 1 0 0 0 0 0 
37 1 0 0 0 0 0 1 1 
38 1 1 1 0 0 0 0 0 
39 1 1 1 0 0 0 1 1 
40 1 0 0 0 0 0 1 1

Output : 

20 12 3 2 1 
20 28 36 29 21 13 6 7 8 
20 12 3 10 17 25 33 
20 28 36 29 21 13 6 15 22 30 39 40

/*
Explanation

The size of the grid is 5×8, i.e. 40 and pixels are numbered in row-wise
manner starting from 1. Next 40 lines contain the direction information for
each pixel. For example, from pixel number 1 one can move only in directions
3, 4, and 5 (refer Fig. 1 for direction numbers) therefore bit numbers 1, 2, 6,
7, and 8 are zero. Similarly, in case of pixel number 11, one can move in 
directions 2, 3, and 4 therefore only three bits at the said positions are set.
The output contains the pixel numbers to be followed to reach the four marked 
corners (refer Fig. 2) starting from pixel 20.
*/
```
# Solution

## Python
```python
from typing import List
from collections import deque


class Node:
    def __init__(self, value):
        self.value = value
        self.children = []

    def __repr__(self):
        return str(self.value)


def bfs(head: Node):
    q: deque[List[Node]] = deque()

    q.append([head])

    results = []
    visited = {}

    # Loop while q is not empty
    while q:
        currentResult = q.popleft()
        currentNode = currentResult[-1]

        if currentNode in visited:
            continue
        else:
            visited[currentNode] = True

        for c in currentNode.children:
            q.append([*currentResult, c])

        results.append(currentResult)

    return results


if __name__ == '__main__':
    rows, columns, startPoint = list(map(int, input().split()))

    node_map = {}

    for i in range(1, rows * columns + 1):
        node_map[i] = Node(i)

    for i in range(1, rows * columns + 1):
        nodeIndex, d1, d2, d3, d4, d5, d6, d7, d8 = list(map(int, input().split()))

        if d1:
            childIndex = nodeIndex - columns
            node_map[nodeIndex].children.append(node_map[childIndex])

        if d2:
            childIndex = nodeIndex - columns + 1
            node_map[nodeIndex].children.append(node_map[childIndex])

        if d3:
            childIndex = nodeIndex + 1
            node_map[nodeIndex].children.append(node_map[childIndex])

        if d4:
            childIndex = nodeIndex + columns + 1
            node_map[nodeIndex].children.append(node_map[childIndex])

        if d5:
            childIndex = nodeIndex + columns
            node_map[nodeIndex].children.append(node_map[childIndex])

        if d6:
            childIndex = nodeIndex + columns - 1
            node_map[nodeIndex].children.append(node_map[childIndex])

        if d7:
            childIndex = nodeIndex - 1
            node_map[nodeIndex].children.append(node_map[childIndex]);

        if d8:
            childIndex = nodeIndex - columns - 1
            node_map[nodeIndex].children.append(node_map[childIndex])

    head = node_map[startPoint]

    topLeft = 1
    topRight = columns
    bottomLeft = (columns * (rows - 1)) + 1
    bottomRight = columns * rows

    results = bfs(head)
    print(*[r for r in results if r[-1].value == topLeft][0], sep=" ")
    print(*[r for r in results if r[-1].value == topRight][0], sep=" ")
    print(*[r for r in results if r[-1].value == bottomLeft][0], sep=" ")
    print(*[r for r in results if r[-1].value == bottomRight][0], sep=" ")

```
## CPP
```cpp


#include <iostream>
#include <vector>
#include <queue>
#include <map>

using namespace std;

class Node {
public:
    int value{};
    vector<Node *> children;
public:
    Node() = default;

    explicit Node(int value) : value(value) {
        this->children = vector<Node *>();
    }
};

vector<vector<Node *>> bfs(Node *head) {
    auto q = queue<vector<Node *>>();
    q.push(vector<Node *>(1, head));

    auto visited = map<Node *, bool>();
    auto results = vector<vector<Node *>>();
    while (!q.empty()) {
        vector<Node *> currentResult = q.front();
        q.pop();
        Node *currentNode = currentResult.back();

        if (visited[currentNode])
            continue;
        else {
            visited[currentNode] = true;
        }

        for (auto &c : currentNode->children) {
            vector<Node *> newResult = currentResult;
            newResult.push_back(c);
            q.push(newResult);
        }

        results.push_back(currentResult);
    }

    return results;
}


int main() {
    int row, column, pixels;
    cin >> row >> column >> pixels;
    vector<Node> arr(row * column + 1);
    for (int i = 1; i < row * column + 1; ++i) {
        arr[i].value = i;
    }
    for (int i = 0; i < row * column + 1; ++i) {
        int index;
        cin >> index;
        int childIndex;
        int p1, p2, p3, p4, p5, p6, p7, p8;
        cin >> p1 >> p2 >> p3 >> p4 >> p5 >> p6 >> p7 >> p8;
        if (p1) {
            childIndex = index - column;
            arr[index].children.push_back(&arr[childIndex]);
        }
        if (p2) {
            childIndex = index - column + 1;
            arr[index].children.push_back(&arr[childIndex]);
        }
        if (p3) {
            childIndex = index + 1;
            arr[index].children.push_back(&arr[childIndex]);
        }
        if (p4) {
            childIndex = index + column + 1;
            arr[index].children.push_back(&arr[childIndex]);
        }
        if (p5) {
            childIndex = index + column;
            arr[index].children.push_back(&arr[childIndex]);
        }
        if (p6) {
            childIndex = index + column - 1;
            arr[index].children.push_back(&arr[childIndex]);
        }
        if (p7) {
            childIndex = index - 1;
            arr[index].children.push_back(&arr[childIndex]);
        }
        if (p8) {
            childIndex = index - column - 1;
            arr[index].children.push_back(&arr[childIndex]);
        }
        arr[index].children.push_back(&arr[childIndex]);
    }

    auto head = &arr[pixels];

    auto topLeft = 1;
    auto topRight = column;
    auto bottomLeft = (column * (row - 1)) + 1;
    auto bottomRight = column * row;

    auto results = bfs(head);

    // top left
    for (auto &possibleResult : results) {
        if (possibleResult.back()->value == topLeft) {
            for (auto &node : possibleResult) {
                cout << node->value << " ";
            }
            cout << endl;
            break;
        }
    }
    // top right
    for (auto &possibleResult : results) {
        if (possibleResult.back()->value == topRight) {
            for (auto &node : possibleResult) {
                cout << node->value << " ";
            }
            cout << endl;
            break;
        }
    }
    // bottom left
    for (auto &possibleResult : results) {
        if (possibleResult.back()->value == bottomLeft) {
            for (auto &node : possibleResult) {
                cout << node->value << " ";
            }
            cout << endl;
            break;
        }
    }
    // bottom right
    for (auto &possibleResult : results) {
        if (possibleResult.back()->value == bottomRight) {
            for (auto &node : possibleResult) {
                cout << node->value << " ";
            }
            cout << endl;
            break;
        }
    }

    return 0;

}
```

## Rust
```rust


use std::io;
use std::thread::sleep;
use std::time::Duration;
use std::collections::{VecDeque, HashMap};

#[allow(unused_macros)]
macro_rules! read_vec {
    ($out:ident as $type:ty) => {
        let mut temp_input = String::new();
        io::stdin().read_line(&mut temp_input).unwrap();
        let $out = temp_input
            .trim()
            .split_whitespace()
            .map(|s| s.parse::<$type>().unwrap())
            .collect::<Vec<$type>>();
    };
}


fn bfs(mat: Vec<Vec<u32>>, head: u32) -> Vec<Vec<u32>> {
    let mut kyu: VecDeque<Vec<u32>> = VecDeque::new();
    // Push initial value into the queue
    kyu.push_back(vec![head]);

    let mut visited: HashMap<u32, bool> = HashMap::new();
    let mut results: Vec<Vec<u32>> = vec![];

    while !kyu.is_empty() {
        let current_result = kyu.pop_front().expect("Failed to get queue's front element");

        let current_node = current_result.last().expect("Current result is empty").clone();

        if visited.contains_key(&current_node) {
            continue;
        } else {
            visited.insert(current_node, true);
        }

        for child in &mat[current_node as usize] {
            let mut new_result = current_result.clone();
            new_result.push(child.clone());
            kyu.push_back(new_result);
        }

        results.push(current_result);
    }

    results
}

fn print_vec(arg: &Vec<u32>) {
    for node in arg{
        print!("{} ", node);
    }
    println!()
}

fn main() {
    read_vec!(inp as u32);

    let (m, n, start_index) = (inp[0], inp[1], inp[2]);

    let mut matrix = vec![vec![0]; (m * n + 1) as usize];
    for i in 1..(m * n + 1) {
        matrix[i as usize].clear();
    }

    for _ in 1..(m * n + 1) {
        read_vec!(inp as u32);
        let index = inp[0];
        if inp[1 as usize] == 1 {
            let child_index = index - n;
            matrix[index as usize].push(child_index);
        }
        if inp[2 as usize] == 1 {
            let child_index = index - n + 1;
            matrix[index as usize].push(child_index);
        }
        if inp[3 as usize] == 1 {
            let child_index = index + 1;
            matrix[index as usize].push(child_index);
        }
        if inp[4 as usize] == 1 {
            let child_index = index + n + 1;
            matrix[index as usize].push(child_index);
        }
        if inp[5 as usize] == 1 {
            let child_index = index + n;
            matrix[index as usize].push(child_index);
        }
        if inp[6 as usize] == 1 {
            let child_index = index + n - 1;
            matrix[index as usize].push(child_index);
        }
        if inp[7 as usize] == 1 {
            let child_index = index - 1;
            matrix[index as usize].push(child_index);
        }
        if inp[8 as usize] == 1 {
            let child_index = index - n - 1;
            matrix[index as usize].push(child_index);
        }
    }

    let head = start_index;
    let top_left: u32 = 1;
    let top_right: u32 = n;
    let bottom_left: u32 = (n * (m - 1)) + 1;
    let bottom_right: u32 = m * n;


    let results = bfs(matrix, head);


    for i in &results {
        if i.last()
            .expect("Unable to fetch last element")
            .clone() == top_left {
            print_vec(i);
            break;
        }
    }
    for i in &results {
        if i.last()
            .expect("Unable to fetch last element")
            .clone() == top_right {
            print_vec(i);
            break;
        }
    }
    for i in &results {
        if i.last()
            .expect("Unable to fetch last element")
            .clone() == bottom_left {
            print_vec(i);
            break;
        }
    }
    for i in &results {
        if i.last()
            .expect("Unable to fetch last element")
            .clone() == bottom_right {
            print_vec(i);
            break;
        }
    }
    sleep(Duration::from_millis(1));
}

```

