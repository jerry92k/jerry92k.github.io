---
layout: post
title:  LeetCode - 809 Expressive Words
date:   2021-10-14 00:00:00
categories: leetcode
---

# 809 Expressive Words

### 문제정의
Sometimes people repeat letters to represent extra feeling. For example:

"hello" -> "heeellooo"
"hi" -> "hiiii"
In these strings like "heeellooo", we have groups of adjacent letters that are all the same: "h", "eee", "ll", "ooo".

You are given a string s and an array of query strings words. A query word is stretchy if it can be made to be equal to s by any number of applications of the following extension operation: choose a group consisting of characters c, and add some number of characters c to the group so that the size of the group is three or more.

For example, starting with "hello", we could do an extension on the group "o" to get "hellooo", but we cannot get "helloo" since the group "oo" has a size less than three. Also, we could do another extension like "ll" -> "lllll" to get "helllllooo". If s = "helllllooo", then the query word "hello" would be stretchy because of these two extension operations: query = "hello" -> "hellooo" -> "helllllooo" = s.
Return the number of query strings that are stretchy.

### 예시
Example 1:

Input: s = "heeellooo", words = ["hello", "hi", "helo"]
Output: 1
Explanation: 
We can extend "e" and "o" in the word "hello" to get "heeellooo".
We can't extend "helo" to get "heeellooo" because the group "ll" is not size 3 or more.
Example 2:

Input: s = "zzzzzyyyyy", words = ["zzyy","zy","zyy"]
Output: 3
 
### 제약사항
Constraints:

1 <= s.length, words.length <= 100
1 <= words[i].length <= 100
s and words[i] consist of lowercase letters.

### 풀이
[전략]
1. 입력 문자열 s를 연속적인 알파벳별 그룹으로 순서대로 나눠 String 배열로 저장
2. 입력 문자열 배열 words를 순회화며, 각 단어 word가 stretch 하여 s를 만들수 있을지 검사
    - s의 연속적 알파벳 그룹이 3보다 작은 경우는, stretch해서 만들 수 없기 때문에 반드시 같아야함
    - s의 연속적 알파벳 그룹이 3이상인 경우는, 단어 word의 그룹이 s의 그룹보다 작거나 같아야함
3. s에 없는 그룹이 word에 있거나 word에 없는 그룹이 s에 있으면 skip

```java
import java.util.ArrayList;
import java.util.List;

class Solution {
    public int expressiveWords(String s, String[] words) {
        List<String> strs = new ArrayList<>();

        // s의 연속적인 문자열들을 String 배열로 변환
        int startPosition = 0;
        char ch = s.charAt(0);
        for (int i = 1; i < s.length(); i++) {
            if (ch != s.charAt(i)) {
                strs.add(s.substring(startPosition, i));
                startPosition = i;
                ch = s.charAt(i);
            }
        }
        strs.add(s.substring(startPosition, s.length()));

        int stretchCount = 0;

        for (String word : words) {
            int sIndex = 0;
            String stdWord = strs.get(sIndex);

            int wordStartIdx = 0;
            if (!isSameGroup(stdWord.charAt(0), word.charAt(wordStartIdx))) {
                continue;
            }

            boolean isStretch = true;
            for (int k = 1; k < word.length(); k++) {

                if (word.charAt(wordStartIdx) != word.charAt(k)) { // 바로 전 꺼랑 문자가 다르면

                    if (sIndex == strs.size() - 1) {
                        isStretch = false;
                        break;
                    }
                    if (!isExpandable(stdWord, word.substring(wordStartIdx, k))) {
                        isStretch = false;
                        break;
                    }
                    stdWord = strs.get(++sIndex);
                    wordStartIdx = k;
                    if (!isSameGroup(stdWord.charAt(0), word.charAt(k))) {
                        isStretch = false;
                        break;
                    }
                }
            }
            if (isStretch && sIndex == strs.size() - 1) {
                if (isExpandable(stdWord, word.substring(wordStartIdx, word.length()))) {
                    stretchCount++;
                }
            }
        }
        return stretchCount;
    }
    public boolean isSameGroup(char stdCh, char candiCh) {
            return stdCh == candiCh ? true : false;
    }

    public boolean isExpandable(String stdWord, String tokenWord) {
        if(stdWord.length() >= 3){ // expandable
            if (tokenWord.length() <= stdWord.length()) { // token이 더 크면 안됨
                return true;
            }
            return false;
        } 
        if (tokenWord.length() == stdWord.length()) {
                return true;
        }
        return false;
    }
}
```