---
title: Number of digits
description: 
published: true
date: 2021-05-30T15:20:37.464Z
tags: 
editor: markdown
dateCreated: 2021-05-30T15:20:37.464Z
---

# Print number of digits in a number
## Java
```java

import java.util.*;


public class Main {

    public static void main(String[] args) {
        // check whether a number is prime or not
        Scanner scn = new Scanner(System.in);
        int n = scn.nextInt();
        int counter = 0;
        if(n == 0) {
            System.out.println(1);
            return;
        }
        while (n > 0) {
            counter++;
            n /= 10;
        }
        System.out.println(counter);


    }
}
```