---
layout: post
title:  eqauls와 hashcode
date:   2021-08-21 00:00:00
categories: java
---

# eqauls, hashCode

equals와 hashCode 매소드는 모든 클래스의 상위 클래스인 Object  클래스에 정의된 매소드



## equals



### equals를 재정의 하지 않아도 되는 경우

- 객체의 논리적 동치성을 검사할 필요가 없는 경우

- equals가 논리적 동치성보다는 인스턴스 고유의 본질을 비교에 적합한 경우(동일한 메모리에 할당)

  

### equals를 재정의 해야하는 경우

- 객체의 논리적 동치성을 확인해야하는 경우
- Map, Set에 활용할 때



### equals 재정의시 일반 규약(Object 명세)

null이 아닌 모든 참조값 x,y,z에 대해

- 반사성(reflexivity) : x.equals(x)는 true 

  - 객체는 자기 자신과 같아야함
  - ex) 컬렉션 클래스에 넣은 다음 contains 매서드로 찾음

- 대칭성(symmetry): x.equals(y)가 true면 y.equals(x)도 true

- 추이성(transitivity): x.equals(y)가 true이고 y.equals(z)도 true이면 x.equals(z)도 true

  => 구체 클래스를 확장한 하위클래스에서 새로운 값을 추가하면서 equals 규약을 만족시킬 방법은 존재하지 않음

  == > prefer composition over inheritance 

- 일관성(consistency): x.equals(y)를 반복해서 호출하면 항상 true를 반환하거나 항상 false를 반환

- null-아님 : x.equals(null)은 false

  - ```if(!(o instance of MyType))``` 을 사용하면 동일한 클래스 타입이 아닌 경우와 null 인 경우 모두 체크가 되어 false로 리턴되므로 명시적 null 검사 ``` if (o==null) ``` 불필요



### 일반규약을 이용한 단계별 구현방법

1. == 연산자를 사용해 입력이 자기 자신의 참조인지 확인

2. instanceof 연산자로 입력이 올바른 타입인지 확인

3. 입력을 올바른 타입으로 변환

4. 입력 객체와 자기 자신에 대응되는 핵심 필드들이 모두 일치하는지 검사

   - float, double을 제외한 기본 타입 필드는 == 연산자로 비교

   - float : Float.compare(float,float) , double: Double.compare(double,double)로 비교

     => Float.equals, Double.equals는 오토방식을 수반하게 되므로 성능에 좋지 않음

   - 다를 가능성이 더 크거나 비교하는 비용이 싼 필드를 먼저 비교하는 것이 성능에 좋음

   - 동기화용 락(lock) 과 같은 논리적 상태와 관련없는 필드는 비교하면 안됨



## hashCode

- equals를 재정의할때 hashCode도 반드시 함께 정의해야 함
- hashCode는 HashMap, HashSet과 같은 컬렉션에서 객체를 key로 사용할 때 해쉬값으로 사용함. 때문에 잘못사용하게 되면 객체를 찾을 수 없게 됨



### hashCode 일반 규약 (Object 명세)

1. equals가 true를 반환하면 hashCode도 똑같은 값을 반환해야 함

   => 논리적으로 같은 객체는 같은 해시코드를 반환해야 함

2. equals가 false를 반환하더라도 hashCode가 반드시 다른 값을 반환해야할 필요는 없음

   => 단, 다른 값을 반환하면 해시테이블 성능이 좋아짐

3. 주체 객체에 equals 반환값 관련 변화가 없다면, hashCode는 일관된 값을 반환해야 함



### hashCode 재정의

- 핵심필드를 반드시 포함
-  equals에 사용되지 않은 필드는 반드시 제외

```java
@Override
public int hashCode(){
  return Objects.hash(핵심필드1,...,핵심필드N);
}
```