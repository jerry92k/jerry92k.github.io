---
layout: post
title: LeetCode - 3 Longest Substring Without Repeating Characters
date: 2021-08-19 00:00:00
categories: leetcode
---

# 3 Longest Substring Without Repeating Characters

### 문제정의
Given a string s, find the length of the longest substring without repeating characters.

### 예시
Example 1:

Input: s = "abcabcbb"
Output: 3
Explanation: The answer is "abc", with the length of 3.

Example 2:

Input: s = "bbbbb"
Output: 1
Explanation: The answer is "b", with the length of 1.

Example 3:

Input: s = "pwwkew"
Output: 3
Explanation: The answer is "wke", with the length of 3.

Notice that the answer must be a substring, "pwke" is a subsequence and not a substring.

Example 4:

Input: s = ""
Output: 0
 
### 제약사항
Constraints:

0 <= s.length <= 5 * 104
s consists of English letters, digits, symbols and spaces.

### 풀이
[전략]
- 문자열을 순회하며 연속적인 substring의 시작점 index(startIdx)과 각 문자에 해당하는 index를 HashMap으로 기록
- 이미 HashMap에 있는 문자가 발생한 경우에는 HashMap에 저장된 index를 읽어와서,
- startIdx보다 이후인 경우(큰 경우), 이전의 동일한 문자는 포함되지 않도록 startIdx를 이전 index+1로 변경 후 substring 계산 
  (startIdx 보다 이전인 경우는 이미 substring으로 카운트하지 않는 이전 문자이므로 상관없음)

```java
import java.util.*;

class Solution {
    public int lengthOfLongestSubstring(String s) {

        if (s.length() == 0) {
            return 0;
        }

        Map<Character,Integer> checkChars = new HashMap<>();
        
        int maxNonRepeatedLen = 0;

        int startIdx=0;
        int endIdx=0;
        for (int i=0; i<s.length(); i++) {
            char ch= s.charAt(i);
            if (checkChars.containsKey(ch)) {

                int preChIdx = checkChars.get(ch);
                if (preChIdx >= startIdx) {
                    startIdx = preChIdx + 1;    
                }
            }
            checkChars.put(ch,i);
            endIdx=i;   
            int nonRepeatedLen = endIdx - startIdx;
            maxNonRepeatedLen = Math.max(maxNonRepeatedLen, nonRepeatedLen);
        }
        maxNonRepeatedLen = Math.max(maxNonRepeatedLen, endIdx-startIdx);
        return maxNonRepeatedLen+1;
    }
}
```

