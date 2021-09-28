---
layout: post
title:  Switch Expressions
date:   2021-09-05 00:00:00
categories: java
---

### JDK 12 이전까지 switch 문법

```java
switch(비교대상변수){
  case ... :
    statement
    break;
  case ... ;
    statement
    break;
  default: // 필수는 아님
    statement
}
```

##### 기본 규칙

- 비교대상변수가 case 조건에 일치하면 해당 절의 로직이 실행됩니다.

- 비교대상변수가 일치하는 case 조건이 없으면 아무일도 일어나지 않습니다.

  => default 절이 명시되어 있으면 일치하는 조건이 없을때 default 절이 실행됩니다.

- **fall through** : case 문 내에 break를 명시하지 않으면 해당 case 이후 case들은 조건비교 없이 항상 실행됩니다.

  => 이후 case 중 break가 명시되어있는 것이 있으면 switch문을 빠져나옵니다.

##### 비교대상변수

- ~java 6 : **"long을 제외한 정수, enum, Character, Byte, Short, Integer"** 타입만 만 비교대상변수로 사용 가능합니다.
- java 7 ~ : "String" 타입도 비교대상변수로 사용 가능합니다.



```java
public class ExtandSwitchTest {

    public static void main(String[] args){

        ExtandSwitchTest extandSwitchTest = new ExtandSwitchTest();
        System.out.println("input age : 20");
        extandSwitchTest.traditionalSwitch(20); // case 20: 절에 break가 없으므로 case 10: 절도 실행됨 
        System.out.println("input age : 10");
        extandSwitchTest.traditionalSwitch(10); // break절이 있으므로 switch문을 빠져나옴.
        // => if(age==20 || age == 10) 과 같은 논리가 됨

        System.out.println("input age : 17");
        extandSwitchTest.traditionalSwitch(17); // case 조건에 해당하지 않는 값이므로 default로 빠짐

        System.out.println();
        System.out.println("input name : jerry");
        extandSwitchTest.stringSwitch("jerry"); //java 7부터는 String로 비교대상변수로 사용 가능

    }

    public void traditionalSwitch(int age){
        switch (age){
            case 30:
                System.out.println("age : 30");
                break;
            case 20:
                System.out.println("age : 20");
//                break;
            case 10:
                System.out.println("age : 10");
                break;
            case 5:
                System.out.println("age : 5");
                break;
            default:
                System.out.println("age : default");
        }
    }

    public void stringSwitch(String name){
        switch (name){
            case "kim":
                System.out.println("name : kim");
                break;
            case "lee":
                System.out.println("name : lee");
                break;
            default:
                System.out.println("not fount");

        }
    }
}

```

> [output]
>
> input age : 20
> age : 20
> age : 10
> input age : 10
> age : 10
> input age : 17
> age : default
>
> input name : jerry
> not fount



### java 12 - java 13를 거치며 Switch문은 기존보다 간편한 방식도 제공합니다.

- **java 12 : JEP - 325 (Preview)**

  1. 조건절에 복수개의 값 사용 가능 

  2. 화살표 표현식 이용 가능 : **no fall throught**

     ```case ... -> labels```

     => 기존 swtich문과 달리 case에 해당하는 절만 실행되고, 그 이후 case들은 실행되지 않음

  3. break 키워드를 이용하여 값 리턴 가능

  4. 화살표 표현식을 이용하여 값 리턴 가능

     => 화살표 표현식 사용할 경우, 값 리턴 전에 다른 처리를 하려면 블록으로 코드를 묶고 break로 마지막에 리턴 

     => 블록의 중간에서 break로 리턴하면

     

- **java 13 : JEP - 354 (Second Preview)**

  break 키워드로 리턴하는 대신 yield 사용 (java 13에서는 break 사용하면 컴파일 오류)



```java
// jdk 16 기반으로 테스트

public class ExtandSwitchTest {

    public static void main(String[] args){

        ExtandSwitchTest extandSwitchTest = new ExtandSwitchTest();

        System.out.println("---- without result test ---");
        System.out.println("input name : kim");
        extandSwitchTest.arrowSwitchWithoutResult("kim"); // 기존 switch문과 달리 조건에 해당하는 구문만 실행
        System.out.println("input name : jerry");
        extandSwitchTest.arrowSwitchWithoutResult("jerry"); // 기존 switch문과 달리 조건에 해당하는 구문만 실행

        System.out.println("---- with result test ---");
        System.out.println("input name : lee");
        extandSwitchTest.arrowSwitchWithResult("lee");
        System.out.println("input name : jerry");
        extandSwitchTest.arrowSwitchWithResult("jerry");
        System.out.println("input name : lucy");
        extandSwitchTest.arrowSwitchWithResult("lucy");
    }

    public void arrowSwitchWithoutResult(String name){
        switch (name){
            case "kim" -> System.out.println("name : kim");
            case "lee" -> System.out.println("name : lee");
            case "jerry" -> {
                System.out.println("before yield");
               // yield 0;  // 리턴 받지 않는 경우는 yield 사용하지 않
            }
            default -> System.out.println("not fount");
        }
    }
    public void arrowSwitchWithResult(String name){
       String result= switch (name){
            case "kim" -> "called kim";
            case "lee" -> "called lee";
            case "jerry" -> {
                System.out.println("before yield jerry");
                yield "jerry";
            }
            case "lucy" ->{
                System.out.println("before yield lucy");
                yield "lucy";
              //  System.out.println("after yield lucy"); // unreachable statement. yield 이후에 statement가 있으면 컴파일 오류
            }
            default -> "not found";
        };
        System.out.println("result : "+result);
    }
}
```

> [output]
>
> ---- without result test ---
> input name : kim
> name : kim
> input name : jerry
> before yield
> ---- with result test ---
> input name : lee
> result : called lee
> input name : jerry
> before yield jerry
> result : jerry
> input name : lucy
> before yield lucy
> result : lucy



[참고]

- openjdk
  - [Switch Expressions (Preview)](https://openjdk.java.net/jeps/325)
  - [Switch Expressions (Second Preview)](https://openjdk.java.net/jeps/354)
  - [Switch Expressions](https://openjdk.java.net/jeps/361)

- [mkyoung.com - switch expression](https://mkyong.com/java/java-13-switch-expressions/)