---
layout: post
title: LeetCode - 49 Group Anagrams
date: 2021-09-17 00:00:00
categories: leetcode
---

# 49 Group Anagrams

### 문제정의
Given an array of strings strs, group the anagrams together. You can return the answer in any order.

An Anagram is a word or phrase formed by rearranging the letters of a different word or phrase, typically using all the original letters exactly once.

### 예시
Example 1:

Input: strs = ["eat","tea","tan","ate","nat","bat"]
Output: [["bat"],["nat","tan"],["ate","eat","tea"]]

Example 2:

Input: strs = [""]
Output: [[""]]

Example 3:

Input: strs = ["a"]
Output: [["a"]]
 
### 제약사항
Constraints:

1 <= strs.length <= 104
0 <= strs[i].length <= 100
strs[i] consists of lowercase English letters.

### 풀이
[전략]
- input String을 순회하며,
- HashMap을 이용해 정렬한 String을 key로 하고
- 정렬 전 String들 key에 해당하는 List에 담아 group anagrams 생성

```java
import java.util.*;

class Solution {
    public List<List<String>> groupAnagrams(String[] strs) {
        Map<String, List<String>> anagGroups = new HashMap<>();

        for (String str : strs) {
            char[] strArr = str.toCharArray();
            Arrays.sort(strArr);
            String sortedStr = String.valueOf(strArr);
            if(anagGroups.containsKey(sortedStr)){
                List<String> anags=anagGroups.get(sortedStr);
                anags.add(str);
            }
            else{
                List<String> anags=new ArrayList<>();
                anags.add(str);
                anagGroups.put(sortedStr, anags);
            }
        }
        
        List<List<String>> result = new ArrayList<>();

        for (Map.Entry<String, List<String>> entry : anagGroups.entrySet()) {
            result.add(entry.getValue());
        }
        return result;
    }
}
```

