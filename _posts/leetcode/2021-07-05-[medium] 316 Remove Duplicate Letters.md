---
layout: post
title: LeetCode - 316 Remove Duplicate Letters
date: 2021-07-05 00:00:00
categories: leetcode
---

# 316 Remove Duplicate Letters

### 문제정의
Given a string s, 
remove duplicate letters so that every letter appears once and only once. 
You must make sure your result is the smallest in lexicographical order among all possible results.

### 예시

Example 1:

Input: s = "bcabc"
Output: "abc"

Example 2:

Input: s = "cbacdcbc"
Output: "acdb"
 
### 제약사항
Constraints:

1 <= s.length <= 104
s consists of lowercase English letters.


### 풀이
[전략]
1. 각 알파벳별 마지막 position을 hashMap에 저장
2. 입력 문자열 s를 하나의 char씩 순회하며
3-1. stack이 비어있으면 push
3-2. stack의 peek의 원소가 현재 알파벳보다 크고, peek원소가 해당 알파벳의 마지막이 아닐때 pop
3-3. stack의 peek의 원소가 현재 작으면, push

```java
import java.util.*;

class Solution {
    public String removeDuplicateLetters(String s) {
        HashMap<Character, Integer> alphaHash = new HashMap<>();
        Set<Character> posAlphas = new HashSet<Character>();
        Stack<Character> stacks = new Stack<>();

        for (int i = 0; i < s.length(); i++) {
            // 알파벳별 마지막 index를 저장
            alphaHash.put(s.charAt(i), i);
        }

        StringBuilder sb = new StringBuilder();
        for (int i = 0; i < s.length(); i++) {
            char ch = s.charAt(i);
            
            if (!posAlphas.contains(ch)) {
                // alphaHash.get(stacks.peek()) >= 1 
                // -> alphaHash.get(stacks.peek()) > i. i 이후에 동일 알파벳이 뒤에 남아있는지 확인
                while (!stacks.empty() && stacks.peek() > ch && alphaHash.get(stacks.peek()) > i) {
                    char peekCh = stacks.pop();
                    posAlphas.remove(peekCh);
                }
                stacks.push(ch);
                posAlphas.add(ch);
            }
           // alphaHash.compute(ch, (key, value) -> value - 1);   
        }

        while (!stacks.isEmpty()) {
            sb.insert(0,stacks.pop()));
        }
        return String.valueOf(sb);
    }

}
```

