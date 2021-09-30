---
layout: post
title: LeetCode - 1249 Minimum Remove To Make Valid Parentheses
date: 2021-09-09 00:00:00
categories: leetcode
---

# 1249 Minimum Remove To Make Valid Parentheses

### 문제정의
Given a string s of '(' , ')' and lowercase English characters.

Your task is to remove the minimum number of parentheses ( '(' or ')', in any positions ) so that the resulting parentheses string is valid and return any valid string.

Formally, a parentheses string is valid if and only if:

It is the empty string, contains only lowercase characters, or
It can be written as AB (A concatenated with B), where A and B are valid strings, or
It can be written as (A), where A is a valid string.

### 예시
Example 1:

Input: s = "lee(t(c)o)de)"
Output: "lee(t(c)o)de"
Explanation: "lee(t(co)de)" , "lee(t(c)ode)" would also be accepted.

Example 2:

Input: s = "a)b(c)d"
Output: "ab(c)d"

Example 3:

Input: s = "))(("
Output: ""
Explanation: An empty string is also valid.

Example 4:

Input: s = "(a(b(c)d)"
Output: "a(b(c)d)"
 
### 제약사항
Constraints:

1 <= s.length <= 105
s[i] is either'(' , ')', or lowercase English letter.

### 풀이
[전략]
- bracket '('와 ')'의 짝이 맞도록 최소한의 괄호를 삭제해야함
- 입력 String을 한자리씩 읽으며 StringBuilder에 넣고, '(' 을 만날경우 위치를 기록한다.
  - 위치 기록은 Stack 자료구조를 사용한다. 
- ')'을 만날경우,
  -  Stack에 값이 존재하면 마지막 '('을 pop하고 => 해당 '(', ')' 은 쌍이 이루어지므로 삭제할 필요 없음
  -  Stack에 값이 존재하지 않으면 ')'은 StringBuilder에 포함시키지 않는다. => 앞에 '('이 없으므로 무조건 삭제해야함
- 입력 String 순회가 끝나면 생성된 StringBuilder에서 Stack에 남아있는 '('위치 값을 뒤에서 부터 삭제한다. => 짝이 없이 남은 '('이므로 삭제햐아함.
- 결과값을 String으로 변환한다.

```java
import java.util.*;

class Solution {
    public String minRemoveToMakeValid(String s) {
        Deque<Integer> openBracketIdxs = new LinkedList<>();
        StringBuilder sb = new StringBuilder();
        int sbIdx = 0;

        for(char ch : s.toCharArray()){
            if (ch == '(') {
                openBracketIdxs.push(sbIdx);
            } else if (ch == ')') {
                if (openBracketIdxs.isEmpty()) {
                    // 앞에 '(' 이 없어서 짝이 안맞는 ')'는 삭제해야하므로 sb에 append하지않고 sbIdx를 증가시키지도 않음
                    continue;
                } else {
                    openBracketIdxs.pop();
                }
            }
            sb.append(ch);
            sbIdx++;
        }
        while (!openBracketIdxs.isEmpty()) {
            int deleteIdx = openBracketIdxs.pop();
            sb.deleteCharAt(deleteIdx);
        }

        return sb.toString();
    }
    
}
```