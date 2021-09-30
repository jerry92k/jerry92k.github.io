---
layout: post
title:  LeetCode - 33. Search in Rotated Sorted Array
date:   2021-09-29 00:00:00
categories: leetcode
---

# 33. Search in Rotated Sorted Array

### 문제정의
There is an integer array nums sorted in ascending order (with distinct values).

Prior to being passed to your function, nums is possibly rotated at an unknown pivot index k (1 <= k < nums.length) such that the resulting array is [nums[k], nums[k+1], ..., nums[n-1], nums[0], nums[1], ..., nums[k-1]] (0-indexed). For example, [0,1,2,4,5,6,7] might be rotated at pivot index 3 and become [4,5,6,7,0,1,2].

Given the array nums after the possible rotation and an integer target, return the index of target if it is in nums, or -1 if it is not in nums.

You must write an algorithm with O(log n) runtime complexity.

### 예시
Example 1:

Input: nums = [4,5,6,7,0,1,2], target = 0
Output: 4

Example 2:

Input: nums = [4,5,6,7,0,1,2], target = 3
Output: -1

Example 3:

Input: nums = [1], target = 0
Output: -1
 
### 제약사항
Constraints:

1 <= nums.length <= 5000
-104 <= nums[i] <= 104
All values of nums are unique.
nums is an ascending array that is possibly rotated.
-104 <= target <= 104

### 풀이
[전략]
- 중간 값을 기준으로 target이 왼쪽반 오른쪽반 어디에 속할지 판단하여 재귀적으로 호출하도록 한다.

```java
class Solution {
    public int search(int[] nums, int target) {
        int startIdx = 0;
        int endIdx = nums.length - 1;

        return findTarget(nums, startIdx, endIdx, target);

    }

    public int findTarget(int[] nums, int startIdx, int endIdx, int target) {

        int middleIdx = (startIdx + endIdx) / 2;

        if (nums[middleIdx] == target) {
            return middleIdx;
        }
        if (startIdx >= endIdx) {
            return -1;
        }

        if (nums[middleIdx] < target) { // 타겟이 중간값보다 큰데
            if (nums[middleIdx] >= nums[startIdx]) // 앞에는 작은 애들만 있음
            {
                return findTarget(nums, middleIdx + 1, endIdx, target);
            } else { // distinct value 이므로 값이 같을 리는 없음
                     // 앞에 작은번호 + 큰번호 이어져있음
                if (nums[endIdx] == target) {
                    return endIdx;
                } else if (nums[endIdx] > target) {
                    return findTarget(nums, middleIdx + 1, endIdx - 1, target);
                } else {
                    return findTarget(nums, startIdx, middleIdx - 1, target);
                }
            }
        } else { // 타겟이 중간값보다 작을때

            if (nums[middleIdx] >= nums[startIdx]) // 앞에는 작은 애들만 있음
            {
                if (nums[startIdx] == target) {
                    return startIdx;
                } else if (nums[startIdx] < target) {
                    return findTarget(nums, startIdx + 1, middleIdx - 1, target);
                } else {
                    return findTarget(nums, middleIdx + 1, endIdx, target);
                }
            } else {
                // 앞에 큰애 + 작은애
                return findTarget(nums, startIdx, middleIdx - 1, target);
            }
        }

    }
}
```