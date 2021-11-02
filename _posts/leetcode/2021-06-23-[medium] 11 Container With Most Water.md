---
layout: post
title: LeetCode - 11 Container With Most Water
date: 2021-06-23 00:00:00
categories: leetcode
---

# 11 Container With Most Water

### 문제정의
Given n non-negative integers a1, a2, ..., an , where each represents a point at coordinate (i, ai). n vertical lines are drawn such that the two endpoints of the line i is at (i, ai) and (i, 0). Find two lines, which, together with the x-axis forms a container, such that the container contains the most water.

Notice that you may not slant the container.

### 예시

Example 1:

![alt text](/public/img/2021-11-02-maxArea.png)

Input: height = [1,8,6,2,5,4,8,3,7]
Output: 49
Explanation: The above vertical lines are represented by array [1,8,6,2,5,4,8,3,7]. In this case, the max area of water (blue section) the container can contain is 49.

Example 2:

Input: height = [1,1]
Output: 1

Example 3:

Input: height = [4,3,2,1,4]
Output: 16

Example 4:

Input: height = [1,2,1]
Output: 2
 
### 제약사항
Constraints:

n == height.length
2 <= n <= 105
0 <= height[i] <= 104

### 풀이

[전략]
- 각 라인을 양쪽 끝에서부터 시작하여, 최대 넓이를 찾아간다.
- 두 라인중 높이가 낮은 라인의 index를 안쪽으로 옮긴다.
why?
높이가 높은 라인의 인덱스를 옮길경우,
낮은 라인의 높이보다 낮다면 넓이 산정의 높이도 낮아지고 index를 옮겨 가로길이도 작아진다.
낮은 라인의 높이보다 같거나 크면 넓이의 높이는 낮은 라인 기준이므로 그대로지만 index를 옮겨 가로 길이가 짧아진다.
==> 높은 라인의 인덱스를 옮겨봤자 항상 넓이는 작아질 일만 있다.
낮은 라인의 인덱스를 옮길경우,
인덱스는 줄어들어 가로길이는 짧아져도 높이 상승의 가능성은 있어서 큰 넓이가 나올 수도 있다.


```java
class Solution {

    public int maxArea(int[] height) {

        int inputLength = height.length;
        
        int maxProduct = 0;

        int i = 0;
        int j = inputLength - 1;
        while (i<j) {

           int productBetweenTwo = Math.min(height[i], height[j]) * (j - i);
           maxProduct = Math.max(maxProduct, productBetweenTwo);
           // 짧은 길이의 점을 그대로 두고 긴 길이의 점을 옮겨 더 긴 길이를 만나더라도 h의 기준은 짧은 길이이고 너비만 줄어들 뿐이므로
           // h가 커질수있는 후보를 찾기위해 짧은 길이의 점을 움직임
           if(height[i]<height[j]){
               i++;
           }else{
               j--;
           }
            
        }
        return maxProduct;
    }
}
```

