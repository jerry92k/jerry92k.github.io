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
[전략] -1
1. 각다이얼의 next dial을 계산해서 HashMap으로 저장
2. 다이얼의 길이만큼 순환하며 이전 dial을 queue에 넣고 하나씩 빼어 가능한 조합을 다시 queue로 넣길 반복하여 모든 조합을 구함

```java
import java.util.*;

class Solution{

    public int knightDialer(int n) {

        if (n == 1) {
            return 10;
        }
        int[][] dials = new int[4][];
        dials[0] = new int[]{1,2,3};
        dials[1] = new int[]{4,5,6};
        dials[2] = new int[]{7,8,9};
        dials[3] = new int[]{-1,0,-1};

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
}

```

[전략] -2
1. dfs로 풀기
2. 각 다이얼을 시작점으로 길이 n만큼 가능한 조합을 탐색

```java
    public int knightDialer2(int n) {

        if (n == 1) {
            return 10;
        }
        int[][] dials = { { 1, 2, 3 }, { 4, 5, 6, }, { 7, 8, 9 }, { -1, 0, -1 } };

        long count = 0;
        int divideVal = 1000000007;
        for (int i = 0; i < dials.length; i++) {
            for (int j = 0; j < dials[i].length; j++) {
                if (dials[i][j] < 0) {
                    continue;
                }
                count += dfsNextDial(dials, i, j, n, 1);
            }
        }

        return count% divideVal;
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
```

[전략] -3
1. 1번 방법과 유사하게, 특정 dial을 시작으로 가능한 next dial을 HashMap으로 저장하여
2. DP 용으로 2차원 배열을 생성한다. row는 다이얼의 길이, column은 각 길이의 마지막 dial이다.
   => dp[row][column] 이면 길이 row인 모든 dial 조합중, 마지막 dial column의 빈도수

```java
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

        for(int j=0; j<10; j++){
             dp[0][j] = 1;
        }

        for (int i = 1; i < n; i++) {
            for (int j = 0; j < 10; j++) {

                if (dp[i - 1][j] > 0) {
                    ArrayList<Integer> nextDials = dialstoNextDial.get(j);
                    for(int nextDial : nextDials){
                        dp[i][nextDial] = (dp[i][nextDial]+dp[i - 1][j])% divideVal;
                    }
                }
                
            }
        }
        long sum = 0;
        for (int j = 0; j < 10; j++) {
            sum = (sum + dp[n - 1][j]);
        }

        return sum % divideVal;
    }

```

