---
layout: post
title:  float와 double 연산이 제대로 되지 않는 이유
date:   2021-08-15 00:00:00
categories: java
---

##  10진 소수를 2진수로 나타낼 수 있을까?

k, x 는 정수일 때,

10진 소수 표현 => k/((2*5)^x) : 

2진 소수 표현 => k/2^x

=> 10진 소수를 2진 소수로 표현할 수 없는 값들이 존재

예를들어,  0.2를 2진 소수로 변환해봅시다.



> [10진 소수를 2진수로 변환하는 방법]
>
> 1. 10진 소수에 2를 곱한 값이 > 1 이면 -> 1, 넘지않으면 0으로 변환
> 2. 1을 넘는 경우에는 정수부 1을 제외한다.
> 3. 1.0이 나올때까지 1,2 과정을 반복한다.
>
> ---
>
> 0.2*2 =0.4 -> 0
>
> 0.4*2=0.8 -> 0
>
> 0.8*2=1.6 -> 1
>
> 0,6*2=1.2 -> 1
>
> 0.2*2=0.4 ... 반복됨.

##### => 0.00110011 이 반복되어 무한소수가 됨.



## float와 double은 소수를 정확히 연산하지 못할 수 있습니다.

컴퓨터는 2진법을 사용합니다.

때문에, float나 double에 0.2를 다루면 실제로는 0.00110011... 이 되어 정확한 0.2의 값을 저장하지 못하게 됩니다.

#### double 간 연산

0.2 + 0.2 => 0.00110011....+ 0.00110011.... 
애초에 double로 정의한 0.2는 실제로 0.2의 값을 가질수 없기 때문에 0.4의 값과 같을 수 없게 됩니다.

#### 정확한 계산이 필요한 비즈니스에 double을 사용하면 문제가 생길 수 있습니다.

예를들어 급여의 30%를 보너스로 계산하는 프로그래밍을 해야한다고 했을때,

```java
long salary= 70_000_000L;
double bonusRate=0.3;
System.out.println(salary*bonusRate); // 출력 2.1E7 => 21,000,000
```

이 결과값으로 보면 double 타입 0.3이 근사치로 문제가 되는 것 같진 않습니다. 

하지만 이것은 실제로는 아래와 같이 연산된 것이고

> 0.3 -> 0.300000000000000001
> 70,000,000 * 0.300000000000000001 = 21,000,000.00000000000000001

1X10^-17 은 너무나 작은 값이기 때문에 System.out.println에서 근사치로  21,000,000와 같이 표현되는 것입니다.



이번에는 급여의 15.5555555555566666%를 보너스로 계산해봅시다.

```java
long salary= 1_000_000_000_000_000_000L;
double bonusRateDouble=0.155555555555566666;

System.out.println(salary*bonusRateDouble);
// 출력 : 1.55555555555566656E17
```

1.555555555555666**56**E17

정확하지 않은 계산값이 도출된 것을 확인할 수 있습니다. 

 

## 정확한 소수 연산이 필요할때는 BigDecimal을 사용

소수점 단위의 정확한 연산이 필요한 서비스에는 float, double 대신 BigDecimal을 사용해야 합니다.

위 예시에서 정확한 결과 값을 얻지 못한 예시에 double을 BigDecimal 타입으로 변경해봅시다.

```java
long salary= 1_000_000_000_000_000_000L;
BigDecimal bonusRateBigDecimal = new BigDecimal("0.155555555555566666");

System.out.println(bonusRateBigDecimal.multiply(BigDecimal.valueOf(salary))); // 출력 : 1.55555555555566666E17
        
```

1.555555555555666**66**E17

double을 사용했을때와 달리 정확한 결과값을 얻는 것을 확인할 수 있습니다.

### BigDecimal

- 참조타입
- Number 클래스를 상속하고 Comparable<T> 인터페이스를 구현하여 비교연산을 위한 compareTo를 재정의

```java
public class BigDecimal extends Number implements Comparable<BigDecimal> 
```

```java
    private final BigInteger intVal; // 정수화 한 숫자
    private final int scale; // 소수부 길이
    private transient int precision; // 소수점을 제외한 전체 자리수 길이
```

- BigDecimal은 소수를 사용하지 않고 내부적으로 정수*10^x 로 표현하므로 정확한 숫자 표현이 가능

```java
 BigDecimal testBigDecimal=new BigDecimal("24.354654654");
 System.out.println("unscaledValue: " + testBigDecimal.unscaledValue());
 System.out.println("정수부: "+testBigDecimal.intValue());
 System.out.println("소수부 자리수: "+testBigDecimal.scale());
 System.out.println("전체길이 : "+testBigDecimal.precision());
```

> 출력
>
> unscaledValue: 24354654654
> 정수부: 24
> 소수부 자리수: 9
> 전체길이 : 11

- BigDecimal 생성자에 호출 파라미터는 문자열로 넘겨주어야 합니다. 
  double형으로 넘겨주게되면 Double.doubleToLongBits(val) 를 호출하여 비트로 나타낸 근사치의 값을 리턴받아 사용하기 때문에 정확하지 않은 값을 사용하게 됩니다.

```java
BigDecimal testBigDecimal2=new BigDecimal(24.354654654);
System.out.println("unscaledValue: " + testBigDecimal2.unscaledValue());
System.out.println("정수부: "+testBigDecimal2.intValue());
System.out.println("소수부 자리수: "+testBigDecimal2.scale());
System.out.println("전체길이 : "+testBigDecimal2.precision());

// 2진수로 나타낼 수 있는 0.5(1/2^1)인 경우에는 올바른 값이 할당된다.
BigDecimal testBigDecimal3=new BigDecimal(0.5);
System.out.println("unscaledValue: " + testBigDecimal3.unscaledValue());
System.out.println("정수부: "+testBigDecimal3.intValue());
System.out.println("소수부 자리수: "+testBigDecimal3.scale());
System.out.println("전체길이 : "+testBigDecimal3.precision());
```

> 출력
>
> unscaledValue: 243546546540000008462811820209026336669921875
> 정수부: 24
> 소수부 자리수: 43
> 전체길이 : 45
>
> unscaledValue: 5
> 정수부: 0
> 소수부 자리수: 1
> 전체길이 : 1

- double형을 그대로 쓰고싶을 경우에는 BigDecimal.valueOf()를 사용합니다.
  BigDecimal.valueOf(double val)는 내부적으로 Double.toString(val)를 호출하여 문자열을 반환한 후 생성자를 호출합니다.

```java
BigDecimal testBigDecimal4=BigDecimal.valueOf(24.354654654);
System.out.println("unscaledValue: " + testBigDecimal4.unscaledValue());
System.out.println("정수부: "+testBigDecimal4.intValue());
System.out.println("소수부 자리수: "+testBigDecimal4.scale());
System.out.println("전체길이 : "+testBigDecimal4.precision());
```

> 출력
>
> unscaledValue: 24354654654
> 정수부: 24
> 소수부 자리수: 9
> 전체길이 : 11
