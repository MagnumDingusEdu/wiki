---
title: Merge Sort Matrix
description: 
published: true
date: 2021-05-28T20:36:11.883Z
tags: 
editor: markdown
dateCreated: 2021-05-28T20:36:11.883Z
---

# 2D Mergesort 
Implement merge sort for a two-dimensional array. In case of odd dimension, the first division contains more number of elements than the second one. The complete execution of merge sort arranges the elements in increasing order either moving row-wise or column-wise.

For example, let there be a `4×4` two dimensional array. The complete process to be implemented is illustrated in Fig. 1. Similarly, Fig. 2 demonstrates the scenario for a `3×3` two dimensional array. One has to keep on dividing till a single element is remaining. During merging, first the row elements get sorted in increasing order followed by sorting of elements lying in the same column.

## Input
* Line 1 contains two space separated integers `M` and `N`, i.e. the size of two dimensional array.
* Line 2 contains `M∗N` unique integers separated by space. These are the contents of a two dimensional array in row-major order.

## Output
* Line 1 is space separated `M∗N` integers sequence. These are the final contents of a two dimensional array in row-major order.

## Constraints 
* All integers are in the range of 1 to 1000

## Examples
```cpp
Input : 
4 4
18 4 16 8 23 13 20 11 28 24 26 25 1 30 15 19

Output : 
1 8 16 18 4 13 19 23 11 15 20 28 24 25 26 30
```
```cpp
Input : 
3 3
18 9 11 1 4 15 13 23 20
Output : 
1 4 11 9 15 18 13 20 23
```

# Solution

## Rust
```rust


fn read_input_vec() -> Vec<u32> {
    let mut line = String::new();
    std::io::stdin().read_line(&mut line).expect("input");
    let nums = line.trim().split(' ').flat_map(str::parse::<u32>).collect::<Vec<_>>();
    nums
}
 
fn print_vec(arg: &Vec<u32>) {
    for node in arg {
        print!("{} ", node);
    }
}
 
 
fn merge_four_matrices(matrix: &mut Vec<Vec<u32>>,
                       start_row: usize,
                       end_row: usize,
                       start_col: usize,
                       end_col: usize,
                       mid_row: usize,
                       mid_col: usize,
) {
    for k in start_row..(end_row + 1)
    {
        let fa = mid_col - start_col + 1;
        let fb = end_col - mid_col;
 
        let mut larr = vec![0; fa];
        let mut rarr = vec![0, fb];
 
 
        for i in 0..fa
        {
            larr[i] = matrix[k][start_col + i];
        }
        for j in 0..fb
        {
            rarr[j] = matrix[k][mid_col + 1 + j] as usize;
        }
 
        let mut i = 0;
        let mut j = 0;
        let mut p = start_col;
 
        while i < fa && j < fb
        {
            if larr[i] <= rarr[j] as u32
            {
                matrix[k][p] = larr[i];
                i += 1;
            } else {
                matrix[k][p] = rarr[j] as u32;
                j += 1;
            }
            p += 1;
        }
        while i < fa
        {
            matrix[k][p] = larr[i];
            i += 1;
            p += 1;
        }
        while j < fb
        {
            matrix[k][p] = rarr[j] as u32;
            j += 1;
            p += 1;
        }
    }
 
    for k in start_col..(end_col + 1)
    {
        let la = mid_row - start_row + 1;
        let lb = end_row - mid_row;
 
        let mut larrc = vec![0; la];
        let mut rarrc = vec![0, lb];
 
        for i in 0..la
        {
            larrc[i] = matrix[start_row + i][k];
        }
        for j in 0..lb
        {
            rarrc[j] = matrix[mid_row + 1 + j][k] as usize;
        }
 
        let mut i = 0;
        let mut j = 0;
        let mut p = start_row;
 
        while i < la && j < lb
        {
            if larrc[i] <= rarrc[j] as u32
            {
                matrix[p][k] = larrc[i];
                i += 1;
            } else {
                matrix[p][k] = rarrc[j] as u32;
                j += 1;
            }
            p += 1;
        }
        while i < la
        {
            matrix[p][k] = larrc[i];
            i += 1;
            p += 1;
        }
        while j < lb
        {
            matrix[p][k] = rarrc[j] as u32;
            j += 1;
            p += 1;
        }
    }
}
 
fn sort_matrix(matrix: &mut Vec<Vec<u32>>,
               start_row: usize,
               end_row: usize,
               start_col: usize,
               end_col: usize) {
    if start_row >= end_row && start_col >= end_col {
        return;
    }
 
    let mid_x;
    let mid_y;
 
    if (end_row - start_row + 1) % 2 == 0 {
        mid_x = ((end_row - start_row) / 2) + start_row;
    } else {
        mid_x = ((end_row - start_row + 1) / 2) + start_row;
    }
 
    if (end_col - start_col + 1) % 2 == 0 {
        mid_y = ((end_col - start_col) / 2) + start_col;
    } else {
        mid_y = ((end_col - start_col + 1) / 2) + start_col;
    }
    sort_matrix(matrix, start_row, mid_x, start_col, mid_y);
    sort_matrix(matrix, start_row, mid_x, mid_y + 1, end_col);
    sort_matrix(matrix, mid_x + 1, end_row, start_col, mid_y);
    sort_matrix(matrix, mid_x + 1, end_row, mid_y + 1, end_col);
 
    merge_four_matrices(matrix,
                        start_row,
                        end_row,
                        start_col,
                        end_col,
                        mid_x,
                        mid_y);
}
 
 
fn main() {
    let inp = read_input_vec();
 
    let (m, n) = (inp[0], inp[1]);
 
    let mut matrix: Vec<Vec<u32>> = vec![vec![0; n as usize]; m as usize];
 
    let inp_mat_raw = read_input_vec();
 
    let mut current_index: usize = 0;
 
    for i in 0..m {
        for j in 0..n {
            matrix[i as usize][j as usize] = inp_mat_raw[current_index];
            current_index += 1;
        }
    }
    sort_matrix(&mut matrix, 0, (m - 1) as usize, 0, (n - 1) as usize);
 
    for row in matrix {
        print_vec(&row);
    }
    println!();

}

```