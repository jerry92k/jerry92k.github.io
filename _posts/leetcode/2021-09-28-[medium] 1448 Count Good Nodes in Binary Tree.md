---
layout: post
title:  LeetCode - 1448. Count Good Nodes in Binary Tree
date:   2021-09-28 00:00:00
categories: leetcode
---

# 1448. Count Good Nodes in Binary Tree

### 문제정의
Given a binary tree root, a node X in the tree is named good if in the path from root to X there are no nodes with a value greater than X.

Return the number of good nodes in the binary tree.

### 예시
Example 1:

Input: root = [3,1,4,3,null,1,5]
Output: 4
Explanation: Nodes in blue are good.
Root Node (3) is always a good node.
Node 4 -> (3,4) is the maximum value in the path starting from the root.
Node 5 -> (3,4,5) is the maximum value in the path
Node 3 -> (3,1,3) is the maximum value in the path.

Example 2:

Input: root = [3,3,null,4,2]
Output: 3
Explanation: Node 2 -> (3, 3, 2) is not good, because "3" is higher than it.

Example 3:

Input: root = [1]
Output: 1
Explanation: Root is considered as good.
 
### 제약사항
Constraints:

The number of nodes in the binary tree is in the range [1, 10^5].
Each node's value is between [-10^4, 10^4].

### 풀이
[전략]
- 바이너르 트리이므로 노드는 최대 2개의 자식 노드를 가질수 있다. 
- 왼쪽 자식 노드와 오른쪽 자식 노드를 각각 재귀적으로 탐색한다. 
- 재귀 탐색시 이전 노드까지의 maximum value를 함수로 같이 전달해주어 good node인지 비교하도록 한다.
  - maximum 보다 현재 노드의 값이 작은 경우에는 good node가 아니므로 count는 증가하지 않고 다음 자식 노드를 탐색한다.
  - maximum 보다 현재 노드의 값이 큰 경우에는 maximum 값을 현재 노드의 값으로 변경하고 count 1 증가후 다음 자식노드를 탐색한다.

```java
public class TreeNode {
    int val;
    TreeNode left;
    TreeNode right;

    TreeNode() {
    }

    TreeNode(int val) {
        this.val = val;
    }

    TreeNode(int val, TreeNode left, TreeNode right) {
        this.val = val;
        this.left = left;
        this.right = right;
    }
}

class Solution {
    public int goodNodes(TreeNode root) {

        return 1+ getNumberOfGoodNodes(root.val, root.left)+ getNumberOfGoodNodes(root.val, root.right);
    }

    public int getNumberOfGoodNodes(int maxVal, TreeNode node) {
        if (node == null) {
            return 0;
        }
        if (node.val >= maxVal) {
            maxVal = node.val;
            return 1 + getNumberOfGoodNodes(maxVal, node.left) + getNumberOfGoodNodes(maxVal, node.right);
        }
        return 0 + getNumberOfGoodNodes(maxVal, node.left) + getNumberOfGoodNodes(maxVal, node.right);
    }
}
```