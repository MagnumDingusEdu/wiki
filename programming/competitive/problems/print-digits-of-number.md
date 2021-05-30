---
title: Print digits of a number
description: 
published: true
date: 2021-05-30T15:27:47.342Z
tags: 
editor: markdown
dateCreated: 2021-05-30T15:27:47.342Z
---

# Print all the digits of the number from L to R
## Java
```java
import java.util.*;


public class Main {

    public static void main(String[] args) {
        // check whether a number is prime or not
        Scanner scn = new Scanner(System.in);
        int n = scn.nextInt();

        int counter = 0;
        int temp = n;
        if (temp == 0) {
            System.out.println(1);
            return;
        }
        // Note : previous input is destroyed. ensure to make a copy
        // before doing further stuff
        while (temp > 0) {
            counter++;
            temp /= 10;
        }


        for (int i = counter; i > 0; i--) {

            System.out.println((int) (n / Math.pow(10, i - 1)) % 10);
        }


    }
}

```