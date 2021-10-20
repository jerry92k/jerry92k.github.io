---
layout: post
title: LeetCode - 1465 Maximum Area Of A Piece Of Cake After Horizontal And Vertical Cuts
date: 2021-06-25 00:00:00
categories: leetcode
---

# 1465 Maximum Area Of A Piece Of Cake After Horizontal And Vertical Cuts

### 문제정의
You are given a rectangular cake of size h x w and two arrays of integers horizontalCuts and verticalCuts where:

horizontalCuts[i] is the distance from the top of the rectangular cake to the ith horizontal cut and similarly, and
verticalCuts[j] is the distance from the left of the rectangular cake to the jth vertical cut.
Return the maximum area of a piece of cake after you cut at each horizontal and vertical position provided in the arrays horizontalCuts and verticalCuts. Since the answer can be a large number, return this modulo 10^9 + 7.

### 예시

Example 1:

Input: h = 5, w = 4, horizontalCuts = [1,2,4], verticalCuts = [1,3]
Output: 4 

Explanation: The figure above represents the given rectangular cake. Red lines are the horizontal and vertical cuts. After you cut the cake, the green piece of cake has the maximum area.

Example 2:

Input: h = 5, w = 4, horizontalCuts = [3,1], verticalCuts = [1]
Output: 6

Explanation: The figure above represents the given rectangular cake. Red lines are the horizontal and vertical cuts. After you cut the cake, the green and yellow pieces of cake have the maximum area.

Example 3:

Input: h = 5, w = 4, horizontalCuts = [3], verticalCuts = [3]
Output: 9
 
### 제약사항
Constraints:

2 <= h, w <= 109
1 <= horizontalCuts.length <= min(h - 1, 105)
1 <= verticalCuts.length <= min(w - 1, 105)
1 <= horizontalCuts[i] < h
1 <= verticalCuts[i] < w
All the elements in horizontalCuts are distinct.
All the elements in verticalCuts are distinct.

### 풀이
[전략]
1. 가로 기준, 세로 기준으로 바운더리에서 부터 각각의 선 까지 크기를 구한다.
2. 가로의 최대값과 세로의 최대값을 곱한 것이 최대 area가 된다.

```java

import java.util.*;

class Solution {
    public int maxArea(int h, int w, int[] horizontalCuts, int[] verticalCuts) {

        int moduloVal = 1000000007;
        Arrays.sort(horizontalCuts);
        Arrays.sort(verticalCuts);

        int maxHeight = h - horizontalCuts[horizontalCuts.length - 1];
        maxHeight = Math.max(maxHeight, horizontalCuts[0]);
        for (int i = 0; i < horizontalCuts.length - 1; i++) {
            int diff = horizontalCuts[i + 1] - horizontalCuts[i];
            maxHeight = Math.max(maxHeight, diff);
        }

        int maxWidth = w - verticalCuts[verticalCuts.length - 1];
        maxWidth = Math.max(maxWidth, verticalCuts[0]);
        for (int i = 0; i < verticalCuts.length - 1; i++) {
            int diff = verticalCuts[i + 1] - verticalCuts[i];
            maxWidth = Math.max(maxWidth, diff);
        }

        long product=((long) maxWidth*(long) maxHeight)%(long)moduloVal;
        return (int) product;
    }
}
```

