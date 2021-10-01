---
layout: post
title: LeetCode - 1048 Longest String Chain
date: 2021-07-17 00:00:00
categories: leetcode
---

# 1048 Longest String Chain

### 문제정의
You are given an array of words where each word consists of lowercase English letters.

wordA is a predecessor of wordB if and only if we can insert exactly one letter anywhere in wordA without changing the order of the other characters to make it equal to wordB.

For example, "abc" is a predecessor of "abac", while "cba" is not a predecessor of "bcad".
A word chain is a sequence of words [word1, word2, ..., wordk] with k >= 1, where word1 is a predecessor of word2, word2 is a predecessor of word3, and so on. A single word is trivially a word chain with k == 1.

Return the length of the longest possible word chain with words chosen from the given list of words.


### 예시
Example 1:

Input: words = ["a","b","ba","bca","bda","bdca"]
Output: 4

Explanation: One of the longest word chains is ["a","ba","bda","bdca"].

Example 2:

Input: words = ["xbc","pcxbcf","xb","cxbc","pcxbc"]
Output: 5

Explanation: All the words can be put in a word chain ["xb", "xbc", "cxbc", "pcxbc", "pcxbcf"].

Example 3:

Input: words = ["abcd","dbqca"]
Output: 1

Explanation: The trivial word chain ["abcd"] is one of the longest word chains.
["abcd","dbqca"] is not a valid word chain because the ordering of the letters is changed.
 
### 제약사항
Constraints:

1 <= words.length <= 1000
1 <= words[i].length <= 16
words[i] only consists of lowercase English letters.

### 풀이
[전략]
1. 입력 문자열 리스트를 오름차순으로 정렬하여 순회한다.
2. 각 문자열의 문자들을 순회하며, 문자를 하나 지운 문자열이 이전 문자열에 존재하는지 확인한다.
3. 이전 문자열에 존재하는지 확인하기 위해 HashMap을 사용하여 문자열을 기록하고, 해당 문자열까지의 연속적인 chain 길이를 기록한다. 

```java


import java.util.*;

class Solution {
    public int longestStrChain(String[] words) {

        HashMap<String, Integer> promiseStrs = new HashMap<>();

        Arrays.sort(words, (a, b) -> a.length() - b.length());

        int maxLen = 1
        for (String word : words) {
            int subMaxLen = 1;
            if(word.length()>1){
                for (int i = 0; i < word.length(); i++) {
                    StringBuilder sb = new StringBuilder(word);
                    sb.deleteCharAt(i);
                    String extractedStr = sb.toString();
                    if (promiseStrs.containsKey(extractedStr)) {
                        subMaxLen = Math.max(subMaxLen, promiseStrs.get(extractedStr) + 1);
                        maxLen = Math.max(maxLen, subMaxLen);
                    }
                }
            }
            promiseStrs.put(word, subMaxLen);
        }

        return maxLen;
    }
}
```

