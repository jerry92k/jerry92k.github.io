---
layout: post
title:  LeetCode - 31. Next Permutation
date:   2021-09-28 00:00:00
categories: leetcode
---

# 31 Next Permutation

### 문제정의
Implement next permutation, which rearranges numbers into the lexicographically next greater permutation of numbers.

If such an arrangement is not possible, it must rearrange it as the lowest possible order (i.e., sorted in ascending order).

The replacement must be in place and use only constant extra memory.


### 예시
Example 1:

Input: nums = [1,2,3]
Output: [1,3,2]

Example 2:

Input: nums = [3,2,1]
Output: [1,2,3]

Example 3:

Input: nums = [1,1,5]
Output: [1,5,1]

Example 4:

Input: nums = [1]
Output: [1]
 
### 제약사항
Constraints:

1 <= nums.length <= 100
0 <= nums[i] <= 100

### 풀이
[전략]
- 숫자의 뒷자리로 갈수록 영향이 작아진다.
- 가장 뒷자리 숫자부터 체크해나간다.
- 앞자리 숫자가 뒷자리 숫자보다 큰 경우에는 swap하면 숫자가 작아지니 skip
- 앞자리 숫자가 뒷자리 숫자보다 작은 경우에는, 뒷자리 숫자들 중 앞자리 숫자 보다는 크지만 그중에 가장 작은 숫자를 선택하여 두 숫자를 swap한다.
   => 이전까지 swap이 발생하지 않았다면 내림차순으로 정렬되어있는것이므로 내부 for문으로 바로 뒷자리부터 순회하여 값을 찾아낸다.
- 자릿수의 숫자 swap후, 뒷자리들은 오름차순으로 정렬하여 return 한다.
- 다음 큰 숫자를 찾지 못해 return 하지 못한 로직은 오름차순으로 정렬하여 리턴한다. 
    => 이 경우, 내림차순으로 정렬되어있기 때문에 정렬 알고리즘을 사용하지 않고 양쪽을 swap하며 n/2 시간에 끝낸다. 

```java
import java.util.Arrays;

class Solution {

    public void nextPermutation(int[] nums) {

        int numsLen = nums.length;

        for (int i = numsLen - 2; i >= 0; i--) {
            int j=i+1;
            if(nums[i]<nums[j]){
                
                while (j < numsLen) {
                    if (nums[j] <= nums[i]) {
                        break;
                    }
                    j++;
                }
                j--;
                
                 int temp = nums[i];
                 nums[i]=nums[j];
                 nums[j]=temp;

                 Arrays.sort(nums, i + 1, numsLen);
                 return;
            }
            // 이번값이 뒤에 값보다 크거나 같으면 바꿔봤자 작아지니 skip
        }

        // 하나 큰 수를 만들수 없어서 밑으로 로직이 빠진 애들은, 내림차순으로 정렬되어있는 경우일것이므로, 반대로 뒤집으면 오름차순 정렬
        for (int i = 0; i < (int) Math.ceil((double) numsLen / 2); i++) {

            int oppositIdx = numsLen - 1 - i;
            if (i == oppositIdx) {
                break;
            }
            int temp = nums[i];
            nums[i] = nums[oppositIdx];
            nums[oppositIdx] = temp;

        }
    }
}
```