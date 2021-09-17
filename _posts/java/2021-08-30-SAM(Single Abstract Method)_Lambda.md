---
layout: post
title:  SAM(Single Abstract Method)_Lambda
date:   2021-08-30 00:00:00 +0900
categories: java
---

자바로 작성된 메서드가 하나인 인터페이스를 구현할 때는 대신 함수를 작성하는 것을 말합니다.

자바에서 작성한 인터페이스일 때만 동작합니다.



```
//구현해야할 메서드가 하나뿐일 때는 람다식으로 변형이 가능합니다.
```



---

Functional Interfaces are known as Single Abstract Method (SAM) in Java 8. These interfaces would have only one abstract method.The example interfaces are Runnanle,Callable, Comparator, etc. Java 8 has introduced Lambda Expressions to instantiate the interface and access the single abstract method easily.



----

```java
package java.util.concurrent; @FunctionalInterface     public interface Callable<V> {     V call() throws Exception; }

Callable<String> aCallable = () -> "dummy";
```





---

. 함수형 인터페이스는 컴파일러가 람다의 타입을 추론할 수 있도록 정보를 제공하는 역할을 합니다. 이 함수형 인터페이스는 단 하나의 추상 메소드(*Single Abstract Method, SAM*)만을 가질 수 있습니다.



함수형 인터페이스에는 `@FunctionalInterface` 라는 어노테이션을 붙일 수 있습니다. `@FunctionalInterface` 는 개발자들에게 해당 인터페이스가 함수형 인터페이스라는 것을 알려주고 컴파일러가 SAM 여부를 체크할 수 있도록 합니다.

```
@FunctionalInterface
interface Calculation {
  Integer apply(Integer x, Integer y);
}

Calculation addition = (x, y) -> x + y;
Calculation subtraction = (x, y) -> x - y;
```



---

JAVA8에서의 다양한 변화중 가장큰 변화는 Lambda표현식을 통한 Funtional Programming (함수형 프로그래밍)을 지원한다는 점



---

JDK8부터 인터페이스도 default 접근제한자와 함께 메소드 **"구현"**도 갖을 수 있게 되었다. 이 때, **FuntionalInterface는 오직 하나의 메소드 선언을 갖는 인터페이스**를 말한다.

출처: https://javaplant.tistory.com/32 [자바공작소]

```java
@FunctionalInterface
interface MyFunction {
	
	void methodA(); // 오직하나의 메소드 선언
	
	// 이런건 메소드 구현 까지 포함
	default void methodB(){ 
		System.out.println("Call Method B");
	}
}

```



```java
@FunctionalInterface
interface MyFunction {
	
	void methodA(); // 오직하나의 메소드 선언
	
	// 이런건 메소드 구현 까지 포함
	default void methodB(){ 
		System.out.println("Call Method B");
	}
}

//이때 MyFunction 오브젝트를 파라메터로 받는 다음과 같은 method가 있다면
public static void test(MyFunction f) {
	f.methodA();
}

//구현체를 생성하여 호출
MyFunction f = new MyFunction() {
	@Override
	public void methodA() {
		System.out.println("Call Method A");
	}
};

test(f);

//Lambda표현식으로 호출
test(()->{System.out.println("Call Method A");});

//위에 두개 패턴은 동일한 소스이다. 즉 Lambda 표현식은 인터페이스의 구현체와 동일한 의미를 갖는다.
//이렇게 하면 더욱 명확해 진다.
MyFunction f = ()->{System.out.println("Call Method A");};
test(f);

```



---

메소드의 이름이 필요 없기 때문에, 람다식은 익명 함수(Anonymous Function)의 한 종류라고 볼 수 있다.

익명함수(Anonymous Function)란 함수의 이름이 없는 함수로, 익명함수들은 모두 일급 객체이다. 일급 객체가 무엇인지는 [앞선 포스팅](https://mangkyu.tistory.com/111)에서 자세히 다루었다. 일급 객체인 함수는 변수처럼 사용가능하며 매개 변수로 전달이 가능하는 등의 특징을 가지고 있다.



람다식이 등장하게 된 이유는 불필요한 코드를 줄이고, 가독성을 높이기 위함이다. 그렇기 때문에 함수형 인터페이스의 인스턴스를 생성하여 함수를 변수처럼 선언하는 람다식에서는 메소드의 이름이 불필요하다고 여겨져서 이를 사용하지 않는다. 대신 컴파일러가 문맥을 살펴 타입을 추론한다. 또한 람다식으로 선언된 함수는 1급 객체이기 때문에 Stream API의 매개변수로 전달이 가능해진다



예를 들어 우리가 두 값 중 큰 값을 구하는 익명 함수를 개발하였다고 하자. 그러면 우리는 지금까지 다음과 같이 개발을 하였을 것이다.

```
public class Lambda {

    public static void main(String[] args) {
    
        // 기존의 익명함수
        System.out.println(new MyLambdaFunction() {
            public int max(int a, int b) {
                return a > b ? a : b;
            }
        }.max(3, 5));

    }

}
```

 

하지만 함수형 인터페이스의 등장으로 우리는 함수를 변수처럼 선언할 수 있게 되었고, 코드 역시 간결하게 작성할 수 있게 되었다. 함수형 인터페이스를 구현하기 위해서는 인터페이스를 개발하여 그 내부에는 1개 뿐인 abstract 함수를 선언하고, 위에는 @FunctionalInterface 어노테이션을 붙여주면 된다. 위의 코드를 람다식으로 변경하면 다음과 같다.

```
@FunctionalInterface
interface MyLambdaFunction {
    int max(int a, int b);
}

public class Lambda {

    public static void main(String[] args) {

        // 람다식을 이용한 익명함수
        MyLambdaFunction lambdaFunction = (int a, int b) -> a > b ? a : b;
        System.out.println(lambdaFunction.max(3, 5));
    }

}
```

 

이제 우리는 Java8 이전에 사용했던 익명함수들을 람다식으로 변경해 코드를 줄일 수 있게 되었고, 여기서 놓치지 말아야 하는 것은 람다식으로 생성된 순수 함수는 함수형 인터페이스로만 선언이 가능하다는 점이다. 또한 @FunctionalInterface는 해당 인터페이스가 1개의 함수만을 갖도록 제한하기 때문에, 여러 개의 함수를 선언하면 컴파일 에러가 발생할 것이라는 점이다. 





