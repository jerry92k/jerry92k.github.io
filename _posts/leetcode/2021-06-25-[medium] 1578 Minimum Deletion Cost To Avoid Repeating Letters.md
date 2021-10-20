---
layout: post
title: LeetCode - 1578 Minimum Deletion Cost To Avoid Repeating Letters
date: 2021-06-25 00:00:00
categories: leetcode
---

# 1578 Minimum Deletion Cost To Avoid Repeating Letters

### 문제정의
Given a string s and an array of integers cost where cost[i] is the cost of deleting the ith character in s.

Return the minimum cost of deletions such that there are no two identical letters next to each other.

Notice that you will delete the chosen characters at the same time, in other words, after deleting a character, the costs of deleting other characters will not change.

### 예시

Example 1:

Input: s = "abaac", cost = [1,2,3,4,5]
Output: 3

Explanation: Delete the letter "a" with cost 3 to get "abac" (String without two identical letters next to each other).

Example 2:

Input: s = "abc", cost = [1,2,3]
Output: 0

Explanation: You don't need to delete any character because there are no identical letters next to each other.

Example 3:

Input: s = "aabaa", cost = [1,2,3,4,1]
Output: 2

Explanation: Delete the first and the last character, getting the string ("aba").
 
### 제약사항
Constraints:

s.length == cost.length
1 <= s.length, cost.length <= 10^5
1 <= cost[i] <= 10^4
s contains only lowercase English letters.

### 풀이
[전략]
1. 입력문자열 s를 순환하며
2. 연속적인 문자의 cost 최대값을 구함
3. 연속적인 문자인데 최대 cost보다 크면, 이전 최대cost는 비용에 포함되므로 전체 cost에 추가하고, 최대 cost는 i번째 cost로 변경
4. 연속적인 문자인데 최대 cost보다 작으면, 전체 cost에 i번째 cost 추가(최대 cost는 변경 없음)
5. 다른 문자가 나오면 연속문자의 최대값을 i번째 새로운 문자의 cost로 변경

```java

class Solution {
    public int minCost(String s, int[] cost) {

        int costSum = 0;
        char preCh = s.charAt(0);
        int subMaxVal = cost[0];
        for (int i = 1; i < s.length(); i++) {
            char curCh = s.charAt(i);
            if (curCh != preCh) {
                subMaxVal = cost[i];
                preCh = curCh;
            } else {
                if (subMaxVal < cost[i]) {
                    costSum += subMaxVal;
                    subMaxVal = cost[i];
                } else {
                    costSum += cost[i];
                }
            }

        }
        return costSum;
    }
}
```

