---
layout: post
title: LeetCode - 1395 Count Number Of Teams
date: 2021-07-17 00:00:00
categories: leetcode
---

# 1395 Count Number Of Teams

### 문제정의
There are n soldiers standing in a line. Each soldier is assigned a unique rating value.

You have to form a team of 3 soldiers amongst them under the following rules:

Choose 3 soldiers with index (i, j, k) with rating (rating[i], rating[j], rating[k]).
A team is valid if: (rating[i] < rating[j] < rating[k]) or (rating[i] > rating[j] > rating[k]) where (0 <= i < j < k < n).
Return the number of teams you can form given the conditions. (soldiers can be part of multiple teams).

### 예시
Example 1:

Input: rating = [2,5,3,4,1]
Output: 3

Explanation: We can form three teams given the conditions. (2,3,4), (5,4,1), (5,3,1). 

Example 2:

Input: rating = [2,1,3]
Output: 0

Explanation: We can't form any team given the conditions.

Example 3:

Input: rating = [1,2,3,4]
Output: 4
 
### 제약사항
Constraints:

n == rating.length
3 <= n <= 1000
1 <= rating[i] <= 105
All the integers in rating are unique.

### 풀이
[전략] -1
- 배열을 순회하며, index i 기준으로, 이전에 index i의 값보다 작은값, 큰 값의 갯수를 각각 별도의 배열로 저장한다.
- 동시에 i 이전의 작은값(혹은 큰 값)이 그 index를 기준으로 관리한 이전의 작은 값(혹은 큰 값)을 전체 카운트에 포함한다. 

오름차순 체크를 예시로 들었을 때,
```
index : k < j < i , 
값 :   rating[j] < rating[i]
```
이라면,

```
index : k1,k2,k3.... < j 일 때,
값 : rating[k?] < rating[j]
```
에 해당하는 갯수를 ratingAES[j]에 저장해놓았을 것이므로, rating[i], rating[j], rating[k?]의 유효한 오름차순 쌍의 갯수는 ratingAES[j]이 된다.


```java

class Solution {
    public int numTeamsDP(int[] rating) {

        int inputLen = rating.length;
        int[] ratingAES = new int[inputLen];
        int[] ratingDESC = new int[inputLen];

        int count = 0;
        for (int i = 1; i < rating.length; i++) {
            for (int j = 0; j < i; j++) {
                if (rating[i] > rating[j]) {
                    ratingAES[i]++;
                    // rating[i]가 rating[j] 보다 커서 if문 안으로 들어온 것이므로, j를 기준으로 j보다 작은 애들의 갯수 만큼 생성
                    count += ratingAES[j];
            
                }
                if (rating[i] < rating[j]) {
                    ratingDESC[i]++;

                    count += ratingDESC[j];
                }
            }
        }

        return count;
    }
```

[전략] -2
- 3중 for문으로 모든 경우의 수를 체크한다.

```java    

    public int numTeamsBruteForce(int[] rating) {
        int count = 0;
        for (int i = 0; i < rating.length - 2; i++) {
            for (int j = i + 1; j < rating.length - 1; j++) {
                if (rating[j] <= rating[i]) {
                    continue;
                }
                for (int k = j + 1; k < rating.length; k++) {
                    if (rating[k] > rating[j]) {
                        count++;
                    }
                }
            }
        }

        for (int i = 0; i < rating.length - 2; i++) {
            for (int j = i + 1; j < rating.length - 1; j++) {
                if (rating[j] >= rating[i]) {
                    continue;
                }
                for (int k = j + 1; k < rating.length; k++) {
                    if (rating[k] < rating[j]) {
                        count++;
                    }
                }
            }
        }
        return count;
    }
}
```

