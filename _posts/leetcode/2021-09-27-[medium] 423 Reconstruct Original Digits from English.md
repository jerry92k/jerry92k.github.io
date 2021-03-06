---
layout: post
title:  LeetCode - 423. Reconstruct Original Digits from English
date:   2021-09-27 00:00:00
categories: leetcode
---

# 423. Reconstruct Original Digits from English

### 문제정의
Given a string s containing an out-of-order English representation of digits 0-9, return the digits in ascending order.


### 예시
Example 1:
Input: s = "owoztneoer"
Output: "012"

Example 2:
Input: s = "fviefuro"
Output: "45"
 
### 제약사항
Constraints:

1 <= s.length <= 105
s[i] is one of the characters ["e","g","f","i","h","o","n","s","r","u","t","w","v","x","z"].
s is guaranteed to be valid.

### 풀이
[전략]
- 숫자에 대한 영어단어를 정의해놓고
- 각 알파벳의 freq를 해쉬맵으로 관리하여
- 숫자단어를 순회하며 가능한 조합을 카운트해본다.
 => 이 때, LinkedHashMap을 사용하여 다음 단어에서 사용되지 않는 알파벳이 있는 단어부터 체크하도록 순서를 정함.

```java
import java.util.Arrays;
import java.util.HashMap;
import java.util.LinkedHashMap;
import java.util.Map;

class Solution {

    public String originalDigits(String s) {
        final int maxSLen = 100000;
        HashMap<String, Integer> numberStrs = getNumberStrs();
        HashMap<Character, Integer> alphabets = new HashMap<>();

        for (char ch : s.toCharArray()) {
            alphabets.compute(ch, (key, value) -> value == null ? 1 : value + 1);
        }

        StringBuffer sb = new StringBuffer();
        for (Map.Entry<String, Integer> numberStrPair : numberStrs.entrySet()) {
            boolean hasNumber = true;
            int minMakeNum = maxSLen;
            char[] numberToMakeStr = numberStrPair.getKey().toCharArray();

            for (char ch : numberToMakeStr) {

                if (alphabets.containsKey(ch)) {
                    minMakeNum = Math.min(minMakeNum, alphabets.get(ch));

                } else {
                    hasNumber = false;
                    break;
                }
            }

            if (!hasNumber) {
                continue;
            }

            int numberVal = numberStrPair.getValue();

            String str = String.valueOf(numberVal);
            for (int i = 0; i < minMakeNum; i++) {
                sb.append(str);
            }

            for (char ch : numberToMakeStr) {

                int freq = alphabets.get(ch);

                freq -= minMakeNum;
                if (freq == 0) {
                    alphabets.remove(ch);
                } else {
                    alphabets.put(ch, freq);
                }
            }
        }

        char[] resultArr = sb.toString().toCharArray();
        Arrays.sort(resultArr);
        return String.valueOf(resultArr);
    }

    public HashMap<String, Integer> getNumberStrs() {
        HashMap<String, Integer> numberStrs = new LinkedHashMap<>();
        // 다음 단어에서 사용되지 않는 알파벳이 있는 단어부터 체크하도록 순서를 정하고 LinkedHashMap 사용 
        numberStrs.put("zero", 0);
        numberStrs.put("two", 2);
        numberStrs.put("four", 4);
        numberStrs.put("five", 5);
        numberStrs.put("six", 6);
        numberStrs.put("eight", 8);
        numberStrs.put("one", 1);
        numberStrs.put("seven", 7);
        numberStrs.put("nine", 9);
        numberStrs.put("three", 3);
        return numberStrs;
    }
}
```