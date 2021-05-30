---
title: Reverse a number
description: 
published: true
date: 2021-05-30T17:07:57.791Z
tags: 
editor: markdown
dateCreated: 2021-05-30T17:07:57.791Z
---

# Reverse a 32-bit number
## Rust
```rust
fn reverse(x: i32) -> i32 {
    let is_negative = (x < 0);
    if x == 0 {
        return x;
    }

    let mut temp = x.abs();
    let mut n_of_digits: i32 = 0;
    while temp > 0 {
        temp /= 10;
        n_of_digits += 1;
    }


    let mut current = x.abs();


    let mut new = 0;
    let mut multiplier = 1;
    while current > 0 {
        let current_digit = current % 10;
        let number_to_add = current_digit as i64 * i64::pow(10, (n_of_digits - multiplier) as u32);
        if new + number_to_add > i32::MAX as i64 {
            return 0;
        }
        new += number_to_add;



        current /= 10;
        multiplier += 1;
    }
    if is_negative {
        return -new as i32;
    }else {
        return new as i32;
    }

}

fn main() {
    let ans = reverse(-123);
    println!("{}", ans)
    
}
```