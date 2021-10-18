---
layout: post
title: LeetCode - 1647 Minimum Deletions To Make Character Frequencies Unique
date: 2021-06-28 00:00:00
categories: leetcode
---

# 1647 Minimum Deletions To Make Character Frequencies Unique

### 문제정의
A string s is called good if there are no two different characters in s that have the same frequency.

Given a string s, return the minimum number of characters you need to delete to make s good.

The frequency of a character in a string is the number of times it appears in the string. For example, in the string "aab", the frequency of 'a' is 2, while the frequency of 'b' is 1.

### 예시

Example 1:

Input: s = "aab"
Output: 0
Explanation: s is already good.

Example 2:

Input: s = "aaabbbcc"
Output: 2

Explanation: You can delete two 'b's resulting in the good string "aaabcc".
Another way it to delete one 'b' and one 'c' resulting in the good string "aaabbc".

Example 3:

Input: s = "ceabaacb"
Output: 2

Explanation: You can delete both 'c's resulting in the good string "eabaab".

Note that we only care about characters that are still in the string at the end (i.e. frequency of 0 is ignored).
 
### 제약사항
Constraints:

1 <= s.length <= 105
s contains only lowercase English letters.

### 풀이
[전략]
1. 입력 문자열 s의 각 문자의 frequency를 HashMap으로 구성한다.
2. frequency의 빈도를 배열로 나타낸다.(Max 빈도만큼의 배열을 할당한다.)
3. frequency의 빈도 배열을 순회하며 빈도의 값을 1만 남기고 이전 frequency로 나머지를 이관하면서 deletion을 중첩해나간다.


```java
import java.util.HashMap;

class Solution {
    public int minDeletions(String s) {
        HashMap<Character, Integer> alphabetFreq = new HashMap<>();

        int maxFreq = 1;
        for (int i = 0; i < s.length(); i++) {
            alphabetFreq.compute(s.charAt(i), (key, value) -> value == null ? 1 : value + 1);
            maxFreq = Math.max(maxFreq, alphabetFreq.get(s.charAt(i)));
        }

        int[] freqArr = new int[maxFreq + 1];

        for (int freq : alphabetFreq.values()) {
            freqArr[freq]++;
        }

        int count = 0;
        for (int i = freqArr.length - 1; i >= 1; i--) {
            if (freqArr[i] > 1) {
                int deleteCount = freqArr[i] - 1;
                count += deleteCount;
                freqArr[i - 1] += deleteCount;
            }
        }
        return count;
    }
}
```

