# [Longest Palindromic Substring](https://leetcode.com/explore/interview/card/microsoft/30/array-and-strings/180/)

![Longest Palindromic Substring](img/longest-palindromic-substring.png)

```python
# Naive Solution: O(nˆ3) ... OMG... Oh Noooo!!!!
class Solution:
    def longestPalindrome(self, s: str) -> str:
        max_length = 0
        max_subs = None
        for i, _ in enumerate(s):
            for j in range(i, len(s) + 1):
                curr_subs = s[i:j]
                if curr_subs == curr_subs[::-1] and len(curr_subs) > max_length:
                    max_length = len(curr_subs)
                    max_subs = curr_subs
                
        return max_subs
```



