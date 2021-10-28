---
layout: post
title: LeetCode - 621 Task Scheduler
date: 2021-06-25 00:00:00
categories: leetcode
---

# 621 Task Scheduler

### 문제정의
Given a characters array tasks, representing the tasks a CPU needs to do, where each letter represents a different task. Tasks could be done in any order. Each task is done in one unit of time. For each unit of time, the CPU could complete either one task or just be idle.

However, there is a non-negative integer n that represents the cooldown period between two same tasks (the same letter in the array), that is that there must be at least n units of time between any two same tasks.

Return the least number of units of times that the CPU will take to finish all the given tasks.

### 예시

Example 1:

Input: tasks = ["A","A","A","B","B","B"], n = 2
Output: 8

Explanation: 
A -> B -> idle -> A -> B -> idle -> A -> B
There is at least 2 units of time between any two same tasks.

Example 2:

Input: tasks = ["A","A","A","B","B","B"], n = 0
Output: 6

Explanation: On this case any permutation of size 6 would work since n = 0.
["A","A","A","B","B","B"]
["A","B","A","B","A","B"]
["B","B","B","A","A","A"]
...
And so on.

Example 3:

Input: tasks = ["A","A","A","A","A","A","B","C","D","E","F","G"], n = 2
Output: 16

Explanation: 
One possible solution is
A -> B -> C -> A -> D -> E -> A -> F -> G -> A -> idle -> idle -> A -> idle -> idle -> A
 
### 제약사항
Constraints:

1 <= task.length <= 104
tasks[i] is upper-case English letter.
The integer n is in the range [0, 100].

### 풀이
[전략] -1
1. 각다이얼의 next dial을 계산해서 HashMap으로 저장
2. 다이얼의 길이만큼 순환하며 이전 dial을 queue에 넣고 하나씩 빼어 가능한 조합을 다시 queue로 넣길 반복하여 모든 조합을 구함


[전략]
1. 알파벳별 빈도수를 카운트 하여 정렬하고
2. 최대 빈도수를 기준으로, n만큼의 idle을 가정하고
3. 그 다음 알파벳들은 사이사이 끼워넣는 다고 생각하고 전체 idle에서 차감
4. 3과정을 하면 idle이 마이너스값이 될 수도 있음 => n보다 많은 종류의 알파벳들이 idle이 생길 틈 없이 연속적으로 이어짐

```java

import java.util.*;

class Solution {
    public int leastInterval(char[] tasks, int n) {

        int[] frequencies = new int[26];
        for (int t : tasks) {
            frequencies[t - 'A']++;
        }

        Arrays.sort(frequencies);

        int f_max = frequencies[25];
        int idle_time = (f_max - 1) * n;

        for (int i = frequencies.length - 2; i >= 0 && idle_time > 0; --i) {
            // maxFreq와 frequencies[i]이 같은 경우 ex) A A A B B B ,
            // Math.min(f_max - 1, frequencies[i]) 로 하지 않고 frequencies[i]로 하면 A 간 idle에 B를 연속으로 넣는 일이 카운트됨.
            idle_time -= Math.min(f_max - 1, frequencies[i]);
        }
        idle_time = Math.max(0, idle_time);

        return idle_time + tasks.length;
    }
}

```

