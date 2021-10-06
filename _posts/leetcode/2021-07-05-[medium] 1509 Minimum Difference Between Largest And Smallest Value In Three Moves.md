---
layout: post
title: LeetCode - 1509 Minimum Difference Between Largest And Smallest Value In Three Moves
date: 2021-07-05 00:00:00
categories: leetcode
---

# 1509 Minimum Difference Between Largest And Smallest Value In Three Moves

### 문제정의
Given an array nums, you are allowed to choose one element of nums and change it by any value in one move.

Return the minimum difference between the largest and smallest value of nums after perfoming at most 3 moves.

### 예시
Example 1:

Input: nums = [5,3,2,4]
Output: 0

Explanation: Change the array [5,3,2,4] to [2,2,2,2].
The difference between the maximum and minimum is 2-2 = 0.

Example 2:

Input: nums = [1,5,0,10,14]
Output: 1

Explanation: Change the array [1,5,0,10,14] to [1,1,0,1,1]. 
The difference between the maximum and minimum is 1-0 = 1.

Example 3:

Input: nums = [6,6,0,1,1,4,6]
Output: 2
Example 4:

Input: nums = [1,5,6,14,15]
Output: 1
 
### 제약사항
Constraints:

1 <= nums.length <= 10^5
-10^9 <= nums[i] <= 10^9

### 풀이
[전략]
- 배열의 원소 갯수가 4개 이하이면 하나의 값으로 통일할수 있으므로 항상 최소 차는 0
- 배열을 정렬하여 3개의 값을 변경하였을때 최소 차 후보들을 구함
- 최소 차가 되기 위해서는 배열의 작은 값들을 증가시키거나 큰값을 감소시켜야함. 
 [배열 앞 뒤 변경 경우의 수]
 0 3 / 1 2 / 2 1 / 3 0 
  
```java


import java.util.*;

class Solution {
    public int minDifference(int[] nums) {

        int numsLen = nums.length;
        if (numsLen <= 4) {
            return 0;
        }

        Arrays.sort(nums);

        int min = nums[numsLen - 1] - nums[0];

        for (int i = 0; i <= 3; i++) {
            min = Math.min(min, nums[numsLen - 4 + i] - nums[i]);
        }
        return min;
    }
}
```

