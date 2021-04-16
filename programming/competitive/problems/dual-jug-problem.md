---
title: 2 Jugs Problem
description: 
published: true
date: 2021-04-16T14:46:52.312Z
tags: 
editor: markdown
dateCreated: 2021-04-16T14:46:05.021Z
---

# Question :
Given two jugs : A 4 liter and 3 liter capacity. Neither has any measurable markers on it. There is a pump which can be used to fill the jugs with water. Simulate the procedure to get exactly 2 liters of water into 4-liter jug.
```python
# !/usr/bin/env python
#######################################################################
#                         ＶＩＶＩＤＨ ＭＡＲＩＹＡ                       #
#                     １０１９０３５７２ | ２ＣＯＥ２２                    #
#                 ＡＲＴＩＦＩＣＡＬ ＩＮＴＥＬＬＩＧＥＮＣＥ                #
#                         ＡＳＳＩＧＮＭＥＮＴ ２                         #
#######################################################################
#                 This python file has type annotations               #
#                     Python 3.5 or above required                    #
#######################################################################

class Jug(object):
    def __init__(self, capacity: int = 0):
        self.capacity: int = capacity
        self.current_level: int = 0

    @property
    def level(self) -> int:
        return self.current_level

    @property
    def full(self) -> bool:
        return self.current_level == self.capacity

    def pump(self) -> None:
        self.current_level = self.capacity

    def pour_out(self) -> None:
        self.current_level = 0

    def pour_into(self, other: 'Jug', stop_if_full: bool = False) -> None:
        if stop_if_full:
            current_level: int = self.current_level
            available_capacity: int = other.capacity - other.current_level

            if available_capacity > 0 and current_level > 0:
                max_transfer_amount: int = min(available_capacity, current_level)
                self.current_level -= max_transfer_amount
                other.current_level += max_transfer_amount
        else:
            other.current_level = min(other.capacity, other.current_level + self.current_level)
            self.current_level = 0


if __name__ == '__main__':
    jug1 = Jug(capacity=4)
    jug2 = Jug(capacity=3)

    jug2.pump()
    jug2.pour_into(jug1)

    jug2.pump()
    jug2.pour_into(jug1, stop_if_full=True)

    jug1.pour_out()
    jug2.pour_into(jug1)

    print(jug1.level, jug2.level)

```