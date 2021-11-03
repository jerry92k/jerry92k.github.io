---
layout: post
title:  LeetCode - 54 Spiral Matrix
date:   2021-11-03 00:00:00
categories: leetcode
---

# 54 Spiral Matrix

### 문제정의
Given an m x n matrix, return all elements of the matrix in spiral order.

### 예시
Example 1:

Input: matrix = [[1,2,3],[4,5,6],[7,8,9]]
Output: [1,2,3,6,9,8,7,4,5]

Example 2:

![alt text](/public/img/2021-11-03-sprialmtrix.png)

Input: matrix = [[1,2,3,4],[5,6,7,8],[9,10,11,12]]
Output: [1,2,3,4,8,12,11,10,9,5,6,7]
 
### 제약사항
Constraints:

m == matrix.length
n == matrix[i].length
1 <= m, n <= 10
-100 <= matrix[i][j] <= 100

### 풀이
[전략]
- matrix의 row와 column 바운더리를 설정한다.
- 가로, 세로를 한줄씩 (리버스도) 읽어가면서 바운더리 계수를 좁힌다.


```java

import java.util.*;

class Solution {
    public List<Integer> spiralOrder(int[][] matrix) {
        List<Integer> spiralOrders = new ArrayList<>();

        int lowBoundRowIdx = 0;
        int hightBoundRowIdx = matrix.length-1;

        int lowBoundColumnIdx = 0;
        int hightBoundColumnIdx = matrix[0].length-1;
        
        while (lowBoundRowIdx <= hightBoundRowIdx && lowBoundColumnIdx <= hightBoundColumnIdx) {
            spiralOrders.addAll(readHorizontal(matrix, lowBoundColumnIdx, hightBoundColumnIdx, lowBoundRowIdx, false));
            lowBoundRowIdx++;

            if (lowBoundRowIdx > hightBoundRowIdx) {
                break;
            }

            spiralOrders.addAll(readVertical(matrix, lowBoundRowIdx, hightBoundRowIdx, hightBoundColumnIdx, false));
            hightBoundColumnIdx--;

            if (lowBoundColumnIdx > hightBoundColumnIdx) {
                break;
            }

            spiralOrders
                    .addAll(readHorizontal(matrix, hightBoundColumnIdx, lowBoundColumnIdx, hightBoundRowIdx, true));
            hightBoundRowIdx--;
            
            spiralOrders
                    .addAll(readVertical(matrix, hightBoundRowIdx, lowBoundRowIdx, lowBoundColumnIdx, true));

            lowBoundColumnIdx++;
            
        }
        return spiralOrders;

    }
    
    private List<Integer> readHorizontal(int[][] matrix, int startIdx, int endIdx, int fixedRow, boolean reverseWay) {
        List<Integer> readValues = new ArrayList<>();

        if (reverseWay) {
            for (int i = startIdx; i >= endIdx; i--) {
                readValues.add(matrix[fixedRow][i]);
            }
        } else {
            for (int i = startIdx; i <= endIdx; i++) {
                readValues.add(matrix[fixedRow][i]);
            }
        }
        return readValues;
    }
    
    private List<Integer> readVertical(int[][] matrix, int startIdx, int endIdx, int fixedColumn, boolean reverseWay) {
        List<Integer> readValues = new ArrayList<>();

        if (reverseWay) {
            for (int i = startIdx; i >= endIdx; i--) {
                readValues.add(matrix[i][fixedColumn]);
            }
        } else {
            for (int i = startIdx; i <= endIdx; i++) {
                readValues.add(matrix[i][fixedColumn]);
            }
        }

        return readValues;
    }

}
```