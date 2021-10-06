---
layout: post
title: LeetCode - 1386 Cinema Seat Allocation
date: 2021-07-05 00:00:00
categories: leetcode
---

# 1386 Cinema Seat Allocation

### 문제정의
A cinema has n rows of seats, numbered from 1 to n and there are ten seats in each row, labelled from 1 to 10 as shown in the figure above.

Given the array reservedSeats containing the numbers of seats already reserved, for example, reservedSeats[i] = [3,8] means the seat located in row 3 and labelled with 8 is already reserved.

Return the maximum number of four-person groups you can assign on the cinema seats. A four-person group occupies four adjacent seats in one single row. Seats across an aisle (such as [3,3] and [3,4]) are not considered to be adjacent, but there is an exceptional case on which an aisle split a four-person group, in that case, the aisle split a four-person group in the middle, which means to have two people on each side.

  ![alt text](/public/img/2021-07-05-cinema-seats.png)

### 예시
Example 1:

Input: n = 3, reservedSeats = [[1,2],[1,3],[1,8],[2,6],[3,1],[3,10]]
Output: 4

Explanation: The figure above shows the optimal allocation for four groups, where seats mark with blue are already reserved and contiguous seats mark with orange are for one group.

Example 2:

Input: n = 2, reservedSeats = [[2,1],[1,8],[2,6]]
Output: 2

Example 3:

Input: n = 4, reservedSeats = [[4,3],[1,4],[4,6],[1,7]]
Output: 4
 
### 제약사항
Constraints:

1 <= n <= 10^9
1 <= reservedSeats.length <= min(10*n, 10^4)
reservedSeats[i].length == 2
1 <= reservedSeats[i][0] <= n
1 <= reservedSeats[i][1] <= 10
All reservedSeats[i] are distinct.

### 풀이
[전략]
1. 배열을 row, column 순으로 오름차순 정렬하여
2. 각각의 reservedSeat 사이가 4명이 앉을수있는 곳인지 판단한다

```java


import java.util.*;

class Solution {
    public int maxNumberOfFamilies(int n, int[][] reservedSeats) {

        Arrays.sort(reservedSeats, (a, b) -> a[0] == b[0] ? a[1] - b[1] : a[0] - b[0]);
        int rowNum = 1;
        int lastOccupied = 0;
        int count = 0;
        for (int i = 0; i < reservedSeats.length; i++) {

            if (reservedSeats[i][0] > rowNum) { // row 바뀜

                int diffRow = reservedSeats[i][0] - rowNum;
                // 한줄이 전체 비었을때 최대 2 그룹이 앉을 수 있음
                if (diffRow > 1) {
                    count += (diffRow - 1) * 2;
                }
                // 이전 줄의 마지막 자리가 어디냐에 따라 달라짐
                if (lastOccupied < 2) {
                    count += 2;
                } else if (lastOccupied < 6) {
                    count++;
                }
                rowNum = reservedSeats[i][0];
                lastOccupied = 0;
            }

            if (reservedSeats[i][1] >= 6 && reservedSeats[i][1] <= 7) {
                if (lastOccupied < 2) {
                    count++;
                }
            } else if (reservedSeats[i][1] >= 8 && reservedSeats[i][1] <= 9) {
                if (lastOccupied < 4) {
                    count++;
                }
            } else if (reservedSeats[i][1] == 10) {
                if (lastOccupied < 2) {
                    count += 2;
                } else if (lastOccupied < 6) {
                    count++;
                }
            }

            lastOccupied = reservedSeats[i][1];
        }

        if (lastOccupied < 2) {
            count += 2;
        } else if (lastOccupied < 6) {
            count++;
        }
        if (rowNum < n) {
            count += (n - rowNum) * 2;
        }
        return count;
    }
}
```

