---
title: Reading input from STDIN
description: 
published: true
date: 2021-01-08T03:42:07.846Z
tags: 
editor: markdown
dateCreated: 2021-01-08T03:42:07.846Z
---

# Macro based stdin input
```rust
use std::io;

#[allow(unused_macros)]
macro_rules! read {
    ($out:ident as $type:ty) => {
        let mut temp_input = String::new();
        io::stdin().read_line(&mut temp_input).expect("Unable to read input.");
        let $out = temp_input.trim().parse::<$type>().expect("Unable to parse");
    };
}

#[allow(unused_macros)]
macro_rules! read_str {
    ($out:ident) => {
        let mut temp_input = String::new();
        io::stdin().read_line(&mut temp_input).expect("Unable to read input.");
        let $out = temp_input.trim();
    };
}

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

fn main(){
    read!(x as u32);
    read!(y as f64);
    read!(z as char);
    println!("{} {} {}", x, y, z);

    read_vec!(v as u32);
    println!("{:?}", v);
}

```