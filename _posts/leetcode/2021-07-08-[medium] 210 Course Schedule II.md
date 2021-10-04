---
layout: post
title: LeetCode - 210 Course Schedule II
date: 2021-07-08 00:00:00
categories: leetcode
---
# 210 Course Schedule II

### 문제정의
There are a total of numCourses courses you have to take, labeled from 0 to numCourses - 1. You are given an array prerequisites where prerequisites[i] = [ai, bi] indicates that you must take course bi first if you want to take course ai.

For example, the pair [0, 1], indicates that to take course 0 you have to first take course 1.
Return the ordering of courses you should take to finish all courses. If there are many valid answers, return any of them. If it is impossible to finish all courses, return an empty array.


### 예시
Example 1:

Input: numCourses = 2, prerequisites = [[1,0]]
Output: [0,1]

Explanation: There are a total of 2 courses to take. To take course 1 you should have finished course 0. So the correct course order is [0,1].

Example 2:

Input: numCourses = 4, prerequisites = [[1,0],[2,0],[3,1],[3,2]]
Output: [0,2,1,3]

Explanation: There are a total of 4 courses to take. To take course 3 you should have finished both courses 1 and 2. Both courses 1 and 2 should be taken after you finished course 0.
So one correct course order is [0,1,2,3]. Another correct ordering is [0,2,1,3].

Example 3:

Input: numCourses = 1, prerequisites = []
Output: [0]
 
### 제약사항
Constraints:

1 <= numCourses <= 2000
0 <= prerequisites.length <= numCourses * (numCourses - 1)
prerequisites[i].length == 2
0 <= ai, bi < numCourses
ai != bi
All the pairs [ai, bi] are distinct.

### 풀이
[전략]
1. key를 기준으로 다음 course, 이전 course 집합을 Map으로 관리
2. 초기에 이전 course가 없는 요소들을 queue에 넣고
3. queue에서 하나씩 빼면서 다음 코스들을 queue에 집어 넣음
4. 여러개의 선수과목이 존재하는 경우 queue에 여러번 들어가므로, Set으로 관리하여 중복된 course일 경우 다음으로 넘어감 

```java

import java.util.*;

class Solution {
    public int[] findOrderQueue(int numCourses, int[][] prerequisites) {

        HashMap<Integer, Set<Integer>> fromHash = new HashMap<>(); // key를 기준으로 다음 course
        HashMap<Integer, Set<Integer>> toHash = new HashMap<>(); // ket를 기준으로 이전 course

        for(int i = 0; i < prerequisites.length; i++) {
            Set<Integer> fromSet = toHash.get(prerequisites[i][0]);
            if (fromSet == null) {
                fromSet = new HashSet<>();
                toHash.put(prerequisites[i][0], fromSet);
            }
            fromSet.add(prerequisites[i][1]);
            
            Set<Integer> toSet = fromHash.get(prerequisites[i][1]);
            if (toSet == null) {
                toSet = new HashSet<>();
                fromHash.put(prerequisites[i][1], toSet);
            }
            toSet.add(prerequisites[i][0]);
        }

        Queue<Integer> courseCandidates = new LinkedList();
        for (int i = 0; i < numCourses; i++) {
            if (!toHash.containsKey(i)) {
                courseCandidates.add(i);
            }
        }

        if (courseCandidates.size() == 0) { // 시작점이 없이 순환됨
            return new int[0];
        }
        
        ArrayList<Integer> courseOrder = new ArrayList<>();

        Set<Integer> orderedSet = new HashSet<>();
        while (!courseCandidates.isEmpty()) {
            int orderCandidate = courseCandidates.poll();
            if (orderedSet.contains(orderCandidate)) {
                continue; // 이미 등록됨
            }
            if (toHash.containsKey(orderCandidate)) {
                //skip. 아직 선수과목이 남음
                continue;
            }
            courseOrder.add(orderCandidate);
            orderedSet.add(orderCandidate);

            if (fromHash.containsKey(orderCandidate)) {
                for (int nextVal : fromHash.get(orderCandidate)) {
                    Set<Integer> fromSet = toHash.get(nextVal);
                    if (fromSet == null || !fromSet.contains(orderCandidate)) {
                        // there is cycle;
                        return new int[0];
                    }
                    if (fromSet.size() == 1) {
                        toHash.remove(nextVal);
                    } else {
                        fromSet.remove(orderCandidate);
                    }
                    courseCandidates.add(nextVal);
                }
            }
        }
        if (courseOrder.size() < numCourses) {
            return new int[0];
        }
        return courseOrder.stream().mapToInt(Integer::intValue).toArray();
    }
}
```

