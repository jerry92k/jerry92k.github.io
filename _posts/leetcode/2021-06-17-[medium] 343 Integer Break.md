---
layout: post
title: LeetCode - 343 Integer Break
date: 2021-06-17 00:00:00
categories: leetcode
---

# 343 Integer Break

### 문제정의
Given an integer n, break it into the sum of k positive integers, where k >= 2, and maximize the product of those integers.

Return the maximum product you can get.

### 예시

Example 1:

Input: n = 2
Output: 1

Explanation: 2 = 1 + 1, 1 × 1 = 1.

Example 2:

Input: n = 10
Output: 36

Explanation: 10 = 3 + 3 + 4, 3 × 3 × 4 = 36.
 
### 제약사항
Constraints:

2 <= n <= 58

### 풀이

[전략]
- 입력 String을 순회하며 각 알파벳별 빈도수를 HasMap으로 저장한다.
- Priority Queue를 이용해서, 가장 우선순위 2개의 Entry를 poll하며 rearrange한 문자열에 추가한다.
- 각 Entry의 빈도수를 1 차감하여 다시 queue에 넣는다.
- queue에 Entry가 하나만 존재하는 경우, 빈도수가 1이면 rearrange한 문자열에 추가하여 return하고,
  빈도수가 1 초과이면 연속되어 나타날 수 밖에 없으므로 ""를 리턴한다.

```java

class Solution {
    public int integerBreak(int n) {

        if (n == 2) {
            return 1;
        } else if (n == 3) {
            return 2;
        }
        int count = 0;
        while (n > 3) {
            n = n - 2;
            count++;
        }

        return n * (int) Math.pow(2, count);
    }
}
```

