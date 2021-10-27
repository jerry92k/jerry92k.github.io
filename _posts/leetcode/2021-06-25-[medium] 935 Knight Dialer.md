---
layout: post
title: LeetCode - 935 Knight Dialer
date: 2021-06-25 00:00:00
categories: leetcode
---

# 935 Knight Dialer

### 문제정의
The chess knight has a unique movement, it may move two squares vertically and one square horizontally, or two squares horizontally and one square vertically (with both forming the shape of an L). The possible movements of chess knight are shown in this diagaram:

A chess knight can move as indicated in the chess diagram below:

![alt text](/public/img/2021-10-27-knight.png)

We have a chess knight and a phone pad as shown below, the knight can only stand on a numeric cell (i.e. blue cell).

![alt text](/public/img/2021-10-27-knight-dialer.png)

Given an integer n, return how many distinct phone numbers of length n we can dial.

You are allowed to place the knight on any numeric cell initially and then you should perform n - 1 jumps to dial a number of length n. All jumps should be valid knight jumps.

As the answer may be very large, return the answer modulo 109 + 7.

### 예시

Example 1:

Input: n = 1
Output: 10

Explanation: We need to dial a number of length 1, so placing the knight over any numeric cell of the 10 cells is sufficient.

Example 2:

Input: n = 2
Output: 20

Explanation: All the valid number we can dial are [04, 06, 16, 18, 27, 29, 34, 38, 40, 43, 49, 60, 61, 67, 72, 76, 81, 83, 92, 94]

Example 3:

Input: n = 3
Output: 46

Example 4:

Input: n = 4
Output: 104

Example 5:

Input: n = 3131
Output: 136006598

Explanation: Please take care of the mod.
 
### 제약사항
Constraints:

1 <= n <= 5000

### 풀이
[전략]
1. 가로 기준, 세로 기준으로 바운더리에서 부터 각각의 선 까지 크기를 구한다.
2. 가로의 최대값과 세로의 최대값을 곱한 것이 최대 area가 된다.

```java
import java.util.*;

class Solution {

    public int knightDialer(int n) {

        if (n == 1) {
            return 10;
        }
        int[][] dials = { { 1, 2, 3 }, { 4, 5, 6, }, { 7, 8, 9 }, { -1, 0, -1 } };

        HashMap<Integer, ArrayList<Integer>> dialstoNextDial = new HashMap<>();
        Queue<Integer> nextDialqueue = new LinkedList<>();

        for (int i = 0; i < dials.length; i++) {
            for (int j = 0; j < dials[i].length; j++) {
                if (dials[i][j] < 0) {
                    continue;
                }

                ArrayList<Integer> nextDials = checkAllNextDialPossible(dials, i, j);

                nextDialqueue.addAll(nextDials);

                dialstoNextDial.put(dials[i][j], nextDials);

            }
        }

        for (int k = 3; k <= n; k++) {
            int queueSize = nextDialqueue.size();
            for (int i = 0; i < queueSize; i++) {
                nextDialqueue.addAll(dialstoNextDial.get(nextDialqueue.remove()));
            }
        }
        int divideVal = 1000000007;
        return nextDialqueue.size() % divideVal;
    }

    public ArrayList<Integer> checkAllNextDialPossible(int[][] dials, int i, int j) {

        ArrayList<Integer> nextDials = new ArrayList<Integer>();

        checkNextDialPossible(dials, i - 2, j + 1, nextDials);
        // -1 up + 2 right
        checkNextDialPossible(dials, i - 1, j + 2, nextDials);

        // 2 up + 1 right
        checkNextDialPossible(dials, i + 2, j + 1, nextDials);
        // 1 up + 2 right
        checkNextDialPossible(dials, i + 1, j + 2, nextDials);

        // -2 up - 1 right
        checkNextDialPossible(dials, i - 2, j - 1, nextDials);
        // -1 up - 2 right
        checkNextDialPossible(dials, i - 1, j - 2, nextDials);
        // 2 up - 1 right
        checkNextDialPossible(dials, i + 2, j - 1, nextDials);
        // 1 up - 2 right
        checkNextDialPossible(dials, i + 1, j - 2, nextDials);

        return nextDials;

    }

    public void checkNextDialPossible(int[][] dials, int horizonIdx, int verticalIdx,
            ArrayList<Integer> nextPossibleDials) {
        if (horizonIdx >= 0 && horizonIdx < dials.length && verticalIdx >= 0 && verticalIdx < dials[0].length) {
            int nextDial = dials[horizonIdx][verticalIdx];
            if (nextDial >= 0) {
                nextPossibleDials.add(nextDial);
            }
        }
    }

    public int knightDialer2(int n) {

        if (n == 1) {
            return 10;
        }
        int[][] dials = { { 1, 2, 3 }, { 4, 5, 6, }, { 7, 8, 9 }, { -1, 0, -1 } };

        // HashMap<Integer, ArrayList<Integer>> dialstoNextDial = new HashMap<>();
        // Queue<Integer> nextDialqueue = new LinkedList<>();

        int count = 0;
        int divideVal = 1000000007;
        for (int i = 0; i < dials.length; i++) {
            for (int j = 0; j < dials[i].length; j++) {
                if (dials[i][j] < 0) {
                    continue;
                }
                count += dfsNextDial(dials, i, j, n, 1) % divideVal;
            }
        }

        return count;
    }

    public int dfsNextDial(int[][] dials, int horizonIdx, int verticalIdx, int n, int curTime) {

        if (horizonIdx >= 0 && horizonIdx < dials.length && verticalIdx >= 0 && verticalIdx < dials[0].length
                && dials[horizonIdx][verticalIdx] >= 0) {
            if (curTime == n) {
                return 1;
            } else {
                return dfsNextDial(dials, horizonIdx - 2, verticalIdx + 1, n, curTime + 1)
                        + dfsNextDial(dials, horizonIdx - 1, verticalIdx + 2, n, curTime + 1)
                        + dfsNextDial(dials, horizonIdx + 2, verticalIdx + 1, n, curTime + 1)
                        + dfsNextDial(dials, horizonIdx + 1, verticalIdx + 2, n, curTime + 1)
                        + dfsNextDial(dials, horizonIdx - 2, verticalIdx - 1, n, curTime + 1)
                        + dfsNextDial(dials, horizonIdx - 1, verticalIdx - 2, n, curTime + 1)
                        + dfsNextDial(dials, horizonIdx + 2, verticalIdx - 1, n, curTime + 1)
                        + dfsNextDial(dials, horizonIdx + 1, verticalIdx - 2, n, curTime + 1);
            }
        }
        return 0;
    }

    public int knightDialer3(int n) {

        if (n == 1) {
            return 10;
        }
        int[][] dials = { { 1, 2, 3 }, { 4, 5, 6, }, { 7, 8, 9 }, { -1, 0, -1 } };

        HashMap<Integer, ArrayList<Integer>> dialstoNextDial = new HashMap<>();

        for (int i = 0; i < dials.length; i++) {
            for (int j = 0; j < dials[i].length; j++) {
                if (dials[i][j] < 0) {
                    continue;
                }
                ArrayList<Integer> nextDials = checkAllNextDialPossible(dials, i, j);
                dialstoNextDial.put(dials[i][j], nextDials);

            }
        }
        int divideVal = 1000000007;
        int[][] dp = new int[n][10];
        for (int i = 0; i < n; i++) {
            for (int j = 0; j < 10; j++) {
                if (i == 0) {
                    dp[i][j] = 1;
                } else {
                    if (dp[i - 1][j] > 0) {
                        ArrayList<Integer> nextDials = dialstoNextDial.get(j);
                        for (int t = 0; t < nextDials.size(); t++) {
                            int prevDialTime = nextDials.get(t);
                            dp[i][prevDialTime] = (dp[i][prevDialTime] + dp[i - 1][j]) % divideVal;
                        }
                    }
                }
            }

        }
        int sum = 0;
        for (int j = 0; j < 10; j++) {
            sum = (sum + dp[n - 1][j]) % divideVal;
        }

        return sum;
    }

}
```

