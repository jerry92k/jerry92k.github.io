---
layout: post
title:  LeetCode - 394 Decode String
date:   2021-10-20 00:00:00
categories: leetcode
---

# 394 Decode String

### 문제정의
Given an encoded string, return its decoded string.

The encoding rule is: k[encoded_string], where the encoded_string inside the square brackets is being repeated exactly k times. Note that k is guaranteed to be a positive integer.

You may assume that the input string is always valid; No extra white spaces, square brackets are well-formed, etc.

Furthermore, you may assume that the original data does not contain any digits and that digits are only for those repeat numbers, k. For example, there won't be input like 3a or 2[4].

### 예시
Example 1:

Input: s = "3[a]2[bc]"
Output: "aaabcbc"

Example 2:

Input: s = "3[a2[c]]"
Output: "accaccacc"

Example 3:

Input: s = "2[abc]3[cd]ef"
Output: "abcabccdcdcdef"

Example 4:

Input: s = "abc3[cd]xyz"
Output: "abccdcdcdxyz"
 
### 제약사항
Constraints:

1 <= s.length <= 30
s consists of lowercase English letters, digits, and square brackets '[]'.
s is guaranteed to be a valid input.
All the integers in s are in the range [1, 300].

### 풀이
[전략] -1
1. 숫자, 알파벳, 괄호에 따라 로직을 분기
2. String을 담는 stack을 이용하여, 연속적은 숫자 String, 여는 괄호 String, 연속적인 문자 String을 담는다.
3. 닫는 괄호를 만나면 연속적인 문자 String을 pop하고 숫자 String만큼을 반복하여 다시 stack에 넣는다.

```java
import java.util.Stack;

class Solution {
    public String decodeString(String s) {

        Stack<String> decodeChars = new Stack<>();
        
        for (int i = 0; i < s.length(); i++) {
            char letter = s.charAt(i);

            if (decodeChars.isEmpty()) {
                decodeChars.push(Character.toString(letter));
                continue;
            }

            if (isEnglish(letter)) {
                if (isEnglish(decodeChars.peek().charAt(0))) {
                    decodeChars.push(decodeChars.pop() + letter);
                }
                else{
                   decodeChars.push(Character.toString(letter));
                }

            } else if (isNumber(letter)) {

                if (isNumber(decodeChars.peek().charAt(0))) {
                    decodeChars.push(decodeChars.pop() + letter);
                }
                else{
                    decodeChars.push(Character.toString(letter));
                }
            }
            else if (isCloseBracket(letter)) {
                String continuosStr = decodeChars.pop();
                StringBuilder multipliedSb = new StringBuilder();
                decodeChars.pop(); // pop open bracket
                int multipleTimes = Integer.parseInt(decodeChars.pop());
                for (int j = 0; j < multipleTimes; j++) {
                    multipliedSb.append(continuosStr);
                }

                if (!decodeChars.isEmpty() && isEnglish(decodeChars.peek().charAt(0))) {
                    decodeChars.push(decodeChars.pop() + multipliedSb.toString());
                } else {
                    decodeChars.push(multipliedSb.toString());
                }

            } else if (isOpenBracket(letter)) {
                decodeChars.push(Character.toString(letter));
            } 
        }

        String result=decodeChars.pop();

        while (!decodeChars.isEmpty()) {
            result+=decodeChars.pop();
        }
        return result;
    }

    private boolean isEnglish(char letter) {
        if (letter >= 'a' && letter <= 'z') {
            return true;
        }
        return false;
    }
    
    private boolean isCloseBracket(char letter) {
        if (letter == ']') {
            return true;
        }
        return false;
    }

    private boolean isOpenBracket(char letter) {
        if (letter == '[') {
            return true;
        }
        return false;
    }
    
    private boolean isNumber(char letter) {
        if (letter >= '0' && letter <= '9') {
            return true;
        }
        return false;
    }

   

}
```

[전략] -2
- 1번 전략의 변형
- String 대신 Character Stack을 만들어 로직을 간단하게 함 
- 로직은 간단하지만 반복해서 Stack에 넣을때 한 문자씩 반복하므로 String 대비 시간은 오래걸림
- String은 객체로 생성되서 힙에 쌓여 나중에 가비지 컬렉션할때 처리할 일이 늘어나지만, Character는 stack[메모리의 stack]에만 쌓이니 메모리 관리에는 더 효율적일 것으로 보임

```java
 public String simpleDecodeString(String s) {

        Stack<Character> decodeChars = new Stack<>();

        for (int i = 0; i < s.length(); i++) {
            char letter = s.charAt(i);

            if (isCloseBracket(letter)) {
                StringBuilder letterGroup = new StringBuilder();
                while (!isOpenBracket(decodeChars.peek())) {
                    letterGroup.insert(0,decodeChars.pop());
                }
                decodeChars.pop();

                StringBuilder numberGroup = new StringBuilder();
                while (!decodeChars.isEmpty() && isNumber(decodeChars.peek())) {
                    numberGroup.insert(0,decodeChars.pop());
                }
                int repeatTimes=Integer.parseInt(numberGroup,0,numberGroup.length(),10);
                
                for (int j = 0; j < repeatTimes; j++) {
                    for(int k=0; k<letterGroup.length(); k++){
                        decodeChars.push(letterGroup.charAt(k));
                    }
                }
            } else {
                decodeChars.push(letter);
            }
        }

        StringBuilder result= new StringBuilder();
        while(!decodeChars.isEmpty()){
            result.insert(0, decodeChars.pop());
        }

        return result.toString();
    }
```