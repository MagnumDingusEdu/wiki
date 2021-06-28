---
title: String Matching
description: 
published: true
date: 2021-06-28T03:39:09.913Z
tags: 
editor: markdown
dateCreated: 2021-06-28T03:39:09.913Z
---

# Rabin-Karp Algorithm
```python
class Solution:

    def rabin_karp_hash(self, string, base, modulus):
        hash = 0
        for letter in string:
            hash *= base
            hash += ord(letter)*base
            hash %= modulus
        return hash

    def strStr(self, haystack: str, needle: str) -> int:
        base = 26
        sub_size = len(needle)
        modulus = 2**31

        if not needle:
            return 0

        if len(needle) > len(haystack):
            return -1

        # Calculate pattern hash
        sub_hash = self.rabin_karp_hash(needle, base, modulus)

        # Calculate initial hash
        current_hash = self.rabin_karp_hash(haystack[:sub_size], base, modulus)

        start_p = 0
        end_p = sub_size - 1

        highest_power = base**(sub_size)

        while True:
            if current_hash == sub_hash and haystack[start_p:end_p + 1] == needle:
                return start_p

            current_hash -= ord(haystack[start_p])*highest_power
            current_hash *= base
            start_p += 1
            end_p += 1

            if end_p >= len(haystack):
                break
            current_hash += ord(haystack[end_p])*base
            current_hash %= modulus

        # No match found
        return -1


a = Solution()

ans = a.strStr("llhaoeunthaueo", "ll")
print("Ans : ", ans)
```


