---
layout: post
title: LeetCode - 945 Minimum Increment To Make Array Unique
date: 2021-06-28 00:00:00
categories: leetcode
---

# 945 Minimum Increment To Make Array Unique

### 문제정의
You are given an integer array nums. In one move, you can pick an index i where 0 <= i < nums.length and increment nums[i] by 1.

Return the minimum number of moves to make every value in nums unique.

### 예시

Example 1:

Input: nums = [1,2,2]
Output: 1

Explanation: After 1 move, the array could be [1, 2, 3].

Example 2:

Input: nums = [3,2,1,2,1,7]
Output: 6

Explanation: After 6 moves, the array could be [3, 4, 1, 2, 5, 7].

It can be shown with 5 or less moves that it is impossible for the array to have all unique values.
 
### 제약사항
Constraints:

1 <= nums.length <= 105
0 <= nums[i] <= 105

### 풀이
[전략]
 1. 각 원소 값의 frequency를 계산하여, 값을 index로 frequency 값을 저장하는 배열을 생성하여
 2. 원소별 하나만 남기고 나머지 freqency는 1씩 더하여 다음 index의 값으로 중첩하며 배열을 순환.
    => 각 단계에서 나머지 frequency들 만큼 increase count에 산정함


```java
class Solution {
    public int minIncrementForUnique(int[] nums) {


        if (nums.length < 2) {
            return 0;
        }
        int maxVal = nums[0];
        for (int i = 1; i < nums.length; i++) {
            maxVal = Math.max(maxVal, nums[i]);
        }

        int[] freqNum = new int[maxVal + 2];

        for (int i = 0; i < nums.length; i++) {
            freqNum[nums[i]]++;
        }

        int increaseCount = 0;
        for (int i = 0; i <= maxVal; i++) {
            if (freqNum[i] > 1) {
                int increaseVal = freqNum[i] - 1;
                increaseCount += increaseVal;
                freqNum[i + 1] += increaseVal;
            }
        }
        for (int i = 1; i < freqNum[maxVal + 1]; i++) {
            increaseCount += i;
        }
        return increaseCount;
    }
}
```

