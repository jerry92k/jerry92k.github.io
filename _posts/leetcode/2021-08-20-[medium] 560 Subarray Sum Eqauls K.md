---
layout: post
title: LeetCode - 560 Subarray Sum Eqauls K
date: 2021-08-20 00:00:00
categories: leetcode
---

# 560 Subarray Sum Eqauls K

### 문제정의
Given an array of integers nums and an integer k, return the total number of continuous subarrays whose sum equals to k.

### 예시
Example 1:

Input: nums = [1,1,1], k = 2
Output: 2

Example 2:

Input: nums = [1,2,3], k = 3
Output: 2
 
### 제약사항
Constraints:

1 <= nums.length <= 2 * 104
-1000 <= nums[i] <= 1000
-107 <= k <= 107

### 풀이
[전략] -1
- dp 방법으로 모든 축적 값을 계산해봄

```java

class Solution {
    public int subarraySum(int[] nums, int k) {

        int count=0;
        for (int i = 0; i < nums.length; i++) {
            int accSum = 0;
            for (int j = i; j < nums.length; j++) {
                if (i == j) {
                    accSum = nums[j];
                } else {
                    accSum = accSum + nums[j];
                }
                if (accSum == k) {
                    count++;
                }
            }
        }
        return count;
    }
}
```
[전략] -2
- dp 방법으로 모든 축적 값을 계산해봄

[전략]
- 배열을 순회하며 축적값을 계산하고, HashMap에 저장
- 해당 index에서의 축적값 - k 한 값이 HashMap에 존재하면, 이전의 특정 idex 까지의 sum을 제외하면    
  k와 같은 연속적인 합이 되므로 count증가. 
- index t와 index t+q 까지의 sum이 같을수 있고 이 경우에 각각의 case로 count 해야하므로 HashMap에 저장할때 개별 카운팅해줌
  
```java
public class Solution2 {
    public int subarraySum(int[] nums, int k) {
        int count = 0, sum = 0;
        HashMap < Integer, Integer > map = new HashMap<>();
        map.put(0, 1);
        for (int i = 0; i < nums.length; i++) {
            sum += nums[i];
            if (map.containsKey(sum - k))
                count += map.get(sum - k);
            map.put(sum, map.getOrDefault(sum, 0) + 1);
        }
        return count;
    }
}
```

