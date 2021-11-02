---
layout: post
title: LeetCode - 134 Gas Station
date: 2021-06-25 00:00:00
categories: leetcode
---

# 134 Gas Station

### 문제정의
There are n gas stations along a circular route, where the amount of gas at the ith station is gas[i].

You have a car with an unlimited gas tank and it costs cost[i] of gas to travel from the ith station to its next (i + 1)th station. You begin the journey with an empty tank at one of the gas stations.

Given two integer arrays gas and cost, return the starting gas station's index if you can travel around the circuit once in the clockwise direction, otherwise return -1. If there exists a solution, it is guaranteed to be unique

### 예시

Example 1:

Input: gas = [1,2,3,4,5], cost = [3,4,5,1,2]
Output: 3

Explanation:
Start at station 3 (index 3) and fill up with 4 unit of gas. Your tank = 0 + 4 = 4
Travel to station 4. Your tank = 4 - 1 + 5 = 8
Travel to station 0. Your tank = 8 - 2 + 1 = 7
Travel to station 1. Your tank = 7 - 3 + 2 = 6
Travel to station 2. Your tank = 6 - 4 + 3 = 5
Travel to station 3. The cost is 5. Your gas is just enough to travel back to station 3.
Therefore, return 3 as the starting index.

Example 2:

Input: gas = [2,3,4], cost = [3,4,3]
Output: -1

Explanation:
You can't start at station 0 or 1, as there is not enough gas to travel to the next station.
Let's start at station 2 and fill up with 4 unit of gas. Your tank = 0 + 4 = 4
Travel to station 0. Your tank = 4 - 3 + 2 = 3
Travel to station 1. Your tank = 3 - 3 + 3 = 3
You cannot travel back to station 2, as it requires 4 unit of gas but you only have 3.
Therefore, you can't travel around the circuit once no matter where you start.
 
### 제약사항
Constraints:

gas.length == n
cost.length == n
1 <= n <= 105
0 <= gas[i], cost[i] <= 104

### 풀이

[전략]-1
- 각 station을 시작점으로 해서 가능한지 brute force로 풀어본다.
  
  
```java
class Solution {
      public int canCompleteCircuit(int[] gas, int[] cost) {

        int stationLength = gas.length;

        for (int i = 0; i < stationLength; i++) {
            int remainGas = gas[i] - cost[i];
            if (remainGas >= 0) {
                int j = (i + 1) % stationLength;
                while (j != i) {
                    remainGas = remainGas + gas[j] - cost[j];
                    if (remainGas < 0) {
                        break;
                    }
                    j = (j + 1) % stationLength;
                }
                if (i == j) {
                    return i;
                }
            }
        }
        return -1;
    }
}
```

[전략]-2
1. 순회를 했을 때 전체 gas-cost 값은 >=0 이면 순회가 성공한다는것이다.
2. 1이 만족하면 어느 지점에서 start 하는 지가 포인트다.
=> 이를 구하는 방법은, 연속적인 합이 마이너스가 되지 않는 시작점을 찾는 것

```java
class Solution {

    public int canCompleteCircuit(int[] gas, int[] cost) {

        int totalSum=0;
        int curSum=0;

        int startStation=0;
        for(int i=0; i<gas.length; i++){
            int nextGasCost=gas[i]-cost[i];
            totalSum+=nextGasCost;
            curSum+=nextGasCost;

            if(curSum<0){
                curSum=0;
                startStation=(i+1)%gas.length;
            }
        }

        return totalSum>=0 ? startStation : -1;
    }
}
```

