---
layout: post
title: LeetCode - 767 Reorganize String
date: 2021-06-21 00:00:00
categories: leetcode
---

# 767 Reorganize String

### 문제정의
Given a string s, rearrange the characters of s so that any two adjacent characters are not the same.

Return any possible rearrangement of s or return "" if not possible.

### 예시

Example 1:

Input: s = "aab"
Output: "aba"
Example 2:

Input: s = "aaab"
Output: ""
 
### 제약사항
Constraints:

1 <= s.length <= 500
s consists of lowercase English letters.

### 풀이

[전략]
- 입력 String을 순회하며 각 알파벳별 빈도수를 HasMap으로 저장한다.
- Priority Queue를 이용해서, 가장 우선순위 2개의 Entry를 poll하며 rearrange한 문자열에 추가한다.
- 각 Entry의 빈도수를 1 차감하여 다시 queue에 넣는다.
- queue에 Entry가 하나만 존재하는 경우, 빈도수가 1이면 rearrange한 문자열에 추가하여 return하고,
  빈도수가 1 초과이면 연속되어 나타날 수 밖에 없으므로 ""를 리턴한다.
  
```java
import java.util.*;

class Solution {
   
    public String reorganizeString(String s) {
 
        HashMap<Character, Integer> alphaMap = new HashMap<>();

        for (int i = 0; i < s.length(); i++) {
            alphaMap.compute(s.charAt(i), (key, value) -> value == null ? 1 : value + 1);
        }

        PriorityQueue<Map.Entry<Character, Integer>> sq = new PriorityQueue<>(
                (a, b) -> b.getValue() - a.getValue());

        for (Map.Entry<Character, Integer> entry : alphaMap.entrySet()) {
            sq.add(entry);
        }

        StringBuilder sb = new StringBuilder();
        while (!sq.isEmpty()) {
            Map.Entry<Character, Integer> firstEntry = sq.poll();
            Map.Entry<Character, Integer> secondEntry = sq.poll();

            int firstEntryFreq = firstEntry.getValue();
            // 한가지 문자열이 남은 경우
            if (secondEntry == null) { // 한개이상 
                if (firstEntryFreq > 1) {
                    return "";
                }
                sb.append(firstEntry.getKey());
                break;
            }
            sb.append(firstEntry.getKey());
            sb.append(secondEntry.getKey());

            if (firstEntryFreq > 1) {
                firstEntry.setValue(firstEntryFreq - 1);
                sq.add(firstEntry);

                int secondEntryFreq = secondEntry.getValue();
                if (secondEntryFreq > 1) {
                    secondEntry.setValue(secondEntryFreq - 1);
                    sq.add(secondEntry);
                }
            }
        }
        return sb.toString();
    }
}



```

