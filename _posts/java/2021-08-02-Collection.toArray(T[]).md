---
layout: post
title:  Collection.toArray(T[])
date:   2021-08-02 00:00:00
categories: java
---
### ArrayList는 자바로 개발을 하면서 가장 많이 쓰는 클래스 중 하나입니다.
ArrayList 타입의 객체를 배열로 변환하려 할때는 아래와 같이 사용합니다.

```java
ArrayList<String> strs=new ArrayList<>(); 
strs.toArray(new String[0]);
```
### strs.toArray(new String[0]);
 은 어떻게 동작하는 것일까?

### "ArrayList extends List" "List extends Collection"
- ArrayList는 List 인터페이스의 구현 클래스입니다.
- List 인터페이스는 Collection 인터페이스를 상속합니다.

> #### Collection을 상속한 인터페이스
- Set
- Queue
- List

#### Collection.toArray(T[])
ArrayList의 toArray(T[]) 매소드는 Collection에 정의된 추상 매소드를 구현한 매소드 입니다.
(Collection interface를 구현한 클래스들은 toArray(T[]) 매소드를 구현해야 합니다.)

#### ArrayList
ArrayList는 toArray를 다음과 같이 구현되어있습니다.
```java
  public <T> T[] toArray(T[] a) {
        if (a.length < size)
            // Make a new array of a's runtime type, but my contents:
            return (T[]) Arrays.copyOf(elementData, size, a.getClass());
        System.arraycopy(elementData, 0, a, 0, size);
        if (a.length > size)
            a[size] = null;
        return a;
    }
```

>
> if (a.length < size)
>      // Make a new array of a's runtime type, but my contents:
>      return (T[]) Arrays.copyOf(elementData, size, a.getClass());

매개변수 a로 넘겨받은 배열의 length < ArrayList 객체 size 경우,
ArrayList size만큼 배열을 생성하고 데이터 복사 후 리턴

>
>System.arraycopy(elementData, 0, a, 0, size);

매개변수 a로 넘겨받은 배열의 length >= ArrayList 객체 size 경우, 
size > index에 ArrayList element 복사

> if (a.length > size)
>        a[size] = null;

size에 해당하는 index에 null을 넣어주는 부분이 있지만
참조타입의 경우 초기화시 null로 초기화되므로 size<= index 들은 null로 초기화

### 그래서 ArrayList를 Array로 변경할 경우,
#### strs.toArray(new String[0]); 와 같이 길이 0의 배열을 넘겨주면 ArrayList 길이만큼 배열을 자동으로 생성


### 코드로 확인하기

```java
import java.util.ArrayList;

public class TestToArray {
    
    public static void main(String[] args){

        TestToArray testToArray=new TestToArray();
        testToArray.test0();
        testToArray.test1();
        testToArray.test2();
        testToArray.test3();
    }

    // 참조타입 객체 초기화시 null 할당
    public void test0(){
        System.out.println("test0");
        String[] strs=new String[5];

        for(String str : strs){
            System.out.println(str);
        }
    }
 
    // ArrayList size와 동일한 길이의 배열을 매개변수로 넘김
    public void test1(){
        ArrayList<String> arr=new ArrayList<>();
        arr.add("hi");
        arr.add("wel");
        arr.add("come");

        System.out.println("test1");
        String[] convertedArr= arr.toArray(new String[3]);

        for(String word : convertedArr){
            System.out.println(word);
        }
    }

    // ArrayList size 보다 큰 길이의 배열을 매개변수로 넘김
    public void test2(){
        ArrayList<String> arr=new ArrayList<>();
        arr.add("hi");
        arr.add("wel");
        arr.add("come");

        System.out.println("test2");
        String[] convertedArr= arr.toArray(new String[5]);

        for(String word : convertedArr){
            System.out.println(word);
        }
    }

    // ArrayList size 보다 작은 길이의 배열을 매개변수로 넘김
    public void test3(){
        ArrayList<String> arr=new ArrayList<>();
        arr.add("hi");
        arr.add("wel");
        arr.add("come");

        System.out.println("test3");
        String[] convertedArr= arr.toArray(new String[2]);

        for(String word : convertedArr){
            System.out.println(word);
        }
    }
}

```