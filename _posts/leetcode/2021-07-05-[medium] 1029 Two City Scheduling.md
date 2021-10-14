---
layout: post
title: LeetCode - 1029 Two City Scheduling
date: 2021-07-05 00:00:00
categories: leetcode
---

# 1029 Two City Scheduling

### 문제정의
A company is planning to interview 2n people. Given the array costs where costs[i] = [aCosti, bCosti], the cost of flying the ith person to city a is aCosti, and the cost of flying the ith person to city b is bCosti.

Return the minimum cost to fly every person to a city such that exactly n people arrive in each city.

### 예시

Example 1:

Input: costs = [[10,20],[30,200],[400,50],[30,20]]
Output: 110

Explanation: 
The first person goes to city A for a cost of 10.
The second person goes to city A for a cost of 30.
The third person goes to city B for a cost of 50.
The fourth person goes to city B for a cost of 20.

The total minimum cost is 10 + 30 + 50 + 20 = 110 to have half the people interviewing in each city.

Example 2:

Input: costs = [[259,770],[448,54],[926,667],[184,139],[840,118],[577,469]]
Output: 1859

Example 3:

Input: costs = [[515,563],[451,713],[537,709],[343,819],[855,779],[457,60],[650,359],[631,42]]
Output: 3086
 
### 제약사항
Constraints:

2 * n == costs.length
2 <= costs.length <= 100
costs.length is even.
1 <= aCosti, bCosti <= 1000

### 풀이
[전략] -1
1. 2n명의 사람이 모두 city B로 갔을때의 합을 구한다.
2. (A를 선택해서 추가되는 비용 - B를 선택하지 않아서 감소되는 비용)으로 priorityqueue를 구성한다.
3. 전체 합이 최소화 되는 방향으로 n명이 city A를 선택하도록 변경한다.

```java
import java.util.PriorityQueue;

class Solution {
    public int twoCitySchedCost(int[][] costs) {

        int minSumB = 0;
        for (int i = 0; i < costs.length; i++) {
            minSumB += costs[i][1];
        }

        PriorityQueue<Integer> abDiff = new PriorityQueue<>();

        for (int i = 0; i < costs.length; i++) {
            abDiff.add(costs[i][0] - costs[i][1]);
        }

        for (int i = 0; i < costs.length / 2; i++) {
            minSumB += abDiff.poll();
        }
        return minSumB;
    }
}
```

[전략] -2
- 전체 합을 구해서 다시 차를 뺄 필요 없이, 
- A를 선택하는 비용 - B를 선택하는 비용 으로 정렬하여
- 앞의 n개는 A를 선택하고 뒤의 n개는 B를 선택하도록 함

```java


class Solution {
    public int twoCitySchedCost(int[][] costs) {

        int n = costs.length/2;
        int minSum = 0;

        Arrays.sort(costs,(costA,costB)-> (costA[0]-costA[1]) - (costB[0]-costB[1]));

       for(int i=0; i<n; i++) {
           minSum +=costs[i][0]+costs[n+i][1];
       }
        return minSum;
    }
}
```