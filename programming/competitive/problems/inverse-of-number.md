---
title: Inverse of a number
description: 
published: true
date: 2021-05-30T15:52:46.925Z
tags: 
editor: markdown
dateCreated: 2021-05-30T15:52:46.925Z
---

# Inverse Of A Number
You are given a number following certain constraints.
* The key constraint is if the number is 5 digits long, it'll contain all the digits from 1 to 5 without missing any and without repeating any. e.g. `23415` is a 5 digit long number containing all digits from 1 to 5 without missing and repeating any digit from 1 to 5. Take a look at few other valid numbers : `624135`, `81456273`, etc. Here are a few invalid numbers : `139`, `7421357`, etc.
* The inverse of a number is defined as the number created by interchanging the face value and index of digits of the number. e.g. for `426135` (reading from right to left, 5 is in place 1, 3 is in place 2, 1 is in place 3, 6 is in place 4, 2 is in place 5 and 4 is in place 6), the inverse will be `416253` (reading from right to left, 3 is in place 1, 5 is in place 2,2 is in place 3, 6 is in place 4, 1 is in place 5 and 4 is in place 6) 
* More examples - the inverse of `2134` is `1243` and inverse of `24153` is `24153`

## Input
```cpp
"n" where n is an integer, following constraints defined above.
```
## Output
```cpp
"i", the inverted number
```
## Examples
```cpp
Input : 28346751
Output : 73425681
```

## Solution
```java
import java.util.*;


public class Main {

    public static void main(String[] args) {
        Scanner scn = new Scanner(System.in);
        int n = scn.nextInt();

        int counter = 0;
        ArrayList<Integer> number = new ArrayList<>();

        // populate the arraylist in reverse order
        while (n > 0) {
            number.add(0, n % 10);
            n /= 10;
        }
        int inverted = 0;

        // replace the number's face value with it's position
        for (int i = 0; i < number.size(); i++) {
            inverted += (number.size() - i) * (Math.pow(10, number.get(i) - 1));
        }
        System.out.println(inverted);

    }
}
```