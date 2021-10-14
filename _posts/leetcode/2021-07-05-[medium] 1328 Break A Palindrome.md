---
layout: post
title: LeetCode - 1328 Break A Palindrome
date: 2021-07-05 00:00:00
categories: leetcode
---

# 1328 Break A Palindrome

### 문제정의
Given a palindromic string of lowercase English letters palindrome, replace exactly one character with any lowercase English letter so that the resulting string is not a palindrome and that it is the lexicographically smallest one possible.

Return the resulting string. If there is no way to replace a character to make it not a palindrome, return an empty string.

A string a is lexicographically smaller than a string b (of the same length) if in the first position where a and b differ, a has a character strictly smaller than the corresponding character in b. For example, "abcc" is lexicographically smaller than "abcd" because the first position they differ is at the fourth character, and 'c' is smaller than 'd'.

### 예시

Example 1:

Input: palindrome = "abccba"
Output: "aaccba"

Explanation: There are many ways to make "abccba" not a palindrome, such as "zbccba", "aaccba", and "abacba".
Of all the ways, "aaccba" is the lexicographically smallest.

Example 2:

Input: palindrome = "a"
Output: ""

Explanation: There is no way to replace a single character to make "a" not a palindrome, so return an empty string.

Example 3:

Input: palindrome = "aa"
Output: "ab"

Example 4:

Input: palindrome = "aba"
Output: "abb"
 
### 제약사항
Constraints:

1 <= palindrome.length <= 1000
palindrome consists of only lowercase English letters.

### 풀이
[전략]
1. smallest를 만들기 위해선 앞자리에서 부터 'a'로 변경을 시도해본다.
 - 원래 'a'인 경우는 'a'로 변경할수 없고
 - 이 경우에는 이 자리를 다른 어떤 알파벳을 넣어도 뒷자를 바꾸는 거보다 큰 값이 되니 변경해도 smallest가 되지 않는다.
2. String이 모두 a로 구성된 경우에는 for문을 빠져나오므로 값을 변경하여 리턴되지 못하므로
3. 제일 마지막 자리를 'a' 다음인 'b'로 변경해주면 대칭이 아닌 가장 작은 문자열을 구할 수 있다.
4. 길이가 1인 문자열인 경우에는 항상 대칭이므로 empty string을 리턴한다.

```java
class Solution {

    public String breakPalindrome(String palindrome) {

        char[] strArr = palindrome.toCharArray();
        int strLen = strArr.length;
        if (strLen < 2) {
            return "";
        }

        for (int i = 0; i < strLen; i++) {
            if ((strLen & 1) == 1 && i == strLen / 2) { // 홀수
                continue;
            }
            if (strArr[i] != 'a') {
                strArr[i] = 'a';
                return String.valueOf(strArr);
            }
        }

        strArr[strLen - 1] = 'b';
        return String.valueOf(strArr);
    }
}
```

