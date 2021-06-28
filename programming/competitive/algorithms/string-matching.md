---
title: String Matching
description: 
published: true
date: 2021-06-28T05:03:39.090Z
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

# KMP (Knuth-Morris-Pratt) Algorithm
> Reference : http://www.btechsmartclass.com/data_structures/knuth-morris-pratt-algorithm.html
{.is-info}

```python
from typing import List


class Solution:

    # Build the prefix table
    # LPS : Longest proper Prefix which is also Suffix
    def build_lps_table(self, pattern: str) -> List[int]:
        lps = [0 for _ in range(len(pattern))]

        # init
        i = 0
        j = 1
        lps[0] = 0

        while True:
            # Check if finished
            if j >= len(pattern) or i >= len(pattern):
                break

            if pattern[i] == pattern[j]:
                lps[j] = i + 1
                i += 1
                j += 1
                continue
            else:
                if i == 0:
                    lps[j] = 0
                    j += 1
                    continue
                else:
                    i = lps[i - 1]
                    continue

        return lps

    def strStr(self, inp: str, pattern: str) -> int:
        if not pattern:
            return 0

        lps = self.build_lps_table(pattern)

        inp_pointer = 0
        pattern_pointer = 0
        print(lps)
        while True:
            if inp_pointer >= len(inp):
                return -1
            print(inp_pointer, pattern_pointer)
            if inp[inp_pointer] == pattern[pattern_pointer]:
                # CHeck if we have arrived at solution
                if pattern_pointer == len(pattern) - 1:
                    return inp_pointer - len(pattern) + 1

                inp_pointer += 1
                pattern_pointer += 1
            else:
                if pattern_pointer != 0:
                    pattern_pointer = lps[pattern_pointer - 1]
                else:
                    inp_pointer += 1


if __name__ == "__main__":
    instance = Solution()
    ans = instance.strStr("mississippi", "issi")
    print(ans)
```



