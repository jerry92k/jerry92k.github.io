---
layout: post
title: LeetCode - 79 Word Search
date: 2021-07-08 00:00:00
categories: leetcode
---

# 79 Word Search

### 문제정의
Given an m x n grid of characters board and a string word, return true if word exists in the grid.

The word can be constructed from letters of sequentially adjacent cells, where adjacent cells are horizontally or vertically neighboring. The same letter cell may not be used more than once.

### 예시
Example 1:

Input: board = [["A","B","C","E"],["S","F","C","S"],["A","D","E","E"]], word = "ABCCED"
Output: true

Example 2:

Input: board = [["A","B","C","E"],["S","F","C","S"],["A","D","E","E"]], word = "SEE"
Output: true

Example 3:

Input: board = [["A","B","C","E"],["S","F","C","S"],["A","D","E","E"]], word = "ABCB"
Output: false
 
### 제약사항
Constraints:

m == board.length
n = board[i].length
1 <= m, n <= 6
1 <= word.length <= 15
board and word consists of only lowercase and uppercase English letters.

### 풀이
[전략] -1
- 각 보드를 시작으로 주어진 문자열이 될수있는 연속점이 있는지 확인
- 2번 방문하는 것을 방지하지 위해 방문 기록 관리.


```java
import java.util.Arrays;

class Solution {
    public boolean exist(char[][] board, String word) {

        boolean[][] visits = new boolean[board.length][board[0].length];

        for (boolean[] visit : visits) {
            Arrays.fill(visit, false);
        }

        for (int i = 0; i < board.length; i++) {
            for (int j = 0; j < board[i].length; j++) {
                // System.out.println("i , j :"+ i+ " : "+j);
                if (backtracking(board, visits, i, j, word, 0)) {
                    return true;
                }
            }
        }
        return false;
    }

    public boolean backtracking(char[][] board, boolean[][] visits, int i, int j, String word, int len) {

        if (board[i][j] == word.charAt(len)) {

            if (word.length() == len + 1) {
                return true;
            }
            visits[i][j] = true;
            boolean up = i <= 0 || visits[i - 1][j] ? false : backtracking(board, visits, i - 1, j, word, len + 1);
            boolean down = i >= board.length - 1 || visits[i + 1][j] ? false
                    : backtracking(board, visits, i + 1, j, word, len + 1);
            boolean left = j <= 0 || visits[i][j - 1] ? false : backtracking(board, visits, i, j - 1, word, len + 1);
            boolean right = j >= board[0].length - 1 || visits[i][j + 1] ? false
                    : backtracking(board, visits, i, j + 1, word, len + 1);

            visits[i][j] = false;
            return up || down || left || right;
        }
        return false;
    }
}
```

### 풀이
[전략] -2
조금 더 성능 개선이 된 알고리즘
- board 정보를 인스턴스 변수로 사용하여 매소드 호출시마다 지역변수를 계속 생성하지 않음
- 방문 루트를 체크하는 것을 별도의 2차원 배열을 사용하지 않고, 다른 문자열로 마킹함으로서 공간 비용 줄임

```java

class Solution {
  // board 정보를 인스턴스 변수로 선언
  private char[][] board;
  private int ROWS;
  private int COLS;

public boolean exist(char[][] board, String word) {
    this.board = board;
    this.ROWS = board.length;
    this.COLS = board[0].length;

    for (int row = 0; row < this.ROWS; ++row)
      for (int col = 0; col < this.COLS; ++col)
        if (this.backtrack(row, col, word, 0))
          return true;
    return false;
  }

  protected boolean backtrack(int row, int col, String word, int index) {
    // 탐색 문자열 길이만큼 정상 탐색 하였으면 true 리턴
    if (index >= word.length())
      return true;

    // 바운더리 체크 , 정삭탐색 루트가 아닌경우 return false
    if (row < 0 || row == this.ROWS || col < 0 || col == this.COLS
        || this.board[row][col] != word.charAt(index))
      return false;

    boolean ret = false;

    // 탐색한 곳을 재방문 하지 않기 위해, 지나온 길을 마킹
    this.board[row][col] = '#';

    int[] rowOffsets = {0, 1, 0, -1};
    int[] colOffsets = {1, 0, -1, 0};
    for (int d = 0; d < 4; ++d) {
      ret = this.backtrack(row + rowOffsets[d], col + colOffsets[d], word, index + 1);
      if (ret)
        break;
    }

    // 방문을 완료 하면서 마킹한 값을 다시 원래의 값으로 변경
    this.board[row][col] = word.charAt(index);
    return ret;
  }
}


```