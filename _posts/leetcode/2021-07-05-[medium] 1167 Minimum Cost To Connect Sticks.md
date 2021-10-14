---
layout: post
title: LeetCode - 1167 Minimum Cost To Connect Sticks
date: 2021-07-05 00:00:00
categories: leetcode
---

# 1167 Minimum Cost To Connect Sticks

### 문제정의
You have some number of sticks with positive integer lengths. These lengths are given as an array sticks, where sticks[i] is the length of the ith stick.

You can connect any two sticks of lengths x and y into one stick by paying a cost of x + y. You must connect all the sticks until there is only one stick remaining.

Return the minimum cost of connecting all the given sticks into one stick in this way.

### 예시

Example 1:

Input: sticks = [2,4,3]
Output: 14

Explanation: You start with sticks = [2,4,3].
1. Combine sticks 2 and 3 for a cost of 2 + 3 = 5. Now you have sticks = [5,4].
2. Combine sticks 5 and 4 for a cost of 5 + 4 = 9. Now you have sticks = [9].
There is only one stick left, so you are done. The total cost is 5 + 9 = 14.

Example 2:

Input: sticks = [1,8,3,5]
Output: 30

Explanation: You start with sticks = [1,8,3,5].
1. Combine sticks 1 and 3 for a cost of 1 + 3 = 4. Now you have sticks = [4,8,5].
2. Combine sticks 4 and 5 for a cost of 4 + 5 = 9. Now you have sticks = [9,8].
3. Combine sticks 9 and 8 for a cost of 9 + 8 = 17. Now you have sticks = [17].
There is only one stick left, so you are done. The total cost is 4 + 9 + 17 = 30.

Example 3:

Input: sticks = [5]
Output: 0

Explanation: There is only one stick, so you don't need to do anything. The total cost is 0.
 
### 제약사항
Constraints:

1 <= sticks.length <= 104
1 <= sticks[i] <= 104

### 풀이
[전략]
- 전체 합은 같다
- 중간 과정에서 발생하는 별도 비용을 최소화 하기 위해서는, 가장 작은 두 수를 더하면 된다.
- 우선순위 큐를 이용한다.
- 원소가 하나가 될 때 까지, 가장 작은 두개의 수를 꺼내 더하고 다시 집어넣기를 반복한다. 


```java
import java.util.PriorityQueue;

class Solution {
    public int connectSticks(int[] sticks) {
        PriorityQueue<Integer> stickCosts = new PriorityQueue<>();

        for (int i = 0; i < sticks.length; i++) {
            stickCosts.add(sticks[i]);
        }

        int sum = 0;
        while (!stickCosts.isEmpty()) {
            int minVal = stickCosts.poll();

            if (stickCosts.isEmpty()) {
                return sum;
            }
            int secMinVal = stickCosts.poll();

            int subSum = minVal + secMinVal;
            sum += subSum;
            stickCosts.add(subSum);
        }
        return sum;
    }
}
```

