---
layout: post
title:  어답터 패턴(Adapter design pattern)
date:   2021-10-22 00:00:00
categories: se
---
#### 어답터 패턴은 
#### 2가지 세부 패턴으로 나눌수 있습니다.

#### 1. object adapter pattern

![alt text](/public/img/2021-10-22-adapter-pattern-1.png)

- Client에서 사용하고자 하는 대상이 되는 Adaptee 클래스를,
- Client에서 직접 참조하지 않고 Adapter 클래스를 이용해 참조
- Adapter는 인터페이스로 추상화하여 Client와의 결합을 최소화함


#### 2. class adapter pattern

![alt text](/public/img/2021-10-22-adapter-pattern-2.png)

- Adpater 클래스에서 Target 인터페이스로 추상화함과 동시에 Adaptee 클래스를 상속받아 사용
- Adpater와 Adaptee의 관계가 composition이 아닌 상속 관계로 이루어져있기 때문에, Adapter는 특정 Adaptee에 종속됨. 



#### 어답터 패턴의 장점과 단점

- 장점
  - 1)메인 비즈니스 로직에서 특정한 기능을 분리하거나 2)서드 파티 라이브러리를 참조할때 결합을 최소화하며 유연한 composition을 제공합니다.
  - 참조 대상 Adaptee를 수정하거나 다른 클래스로 변경하는 등의 변화가 발생할 경우 Client 코드의 수정 없이 Adapter 클래스의 참조를 변경하여 해결할 수 있습니다.
- 단점
  - 참조 대상 클래스와의 결합은 낮아졌지만, 그만큼 중간에 삽입되고 추상화되는 클래스가 늘어나면서 코드의 복잡도가 올라갑니다.
  - 추상화와 느슨한 결합은 이상적으로는 훌륭하지만, 모든 케이스에 다 적용하기에는 설계 복잡도와의 트레이드 오프를 생각해 보아야 합니다.
  - class adapter pattern의 경우 참조대상 Adaptee와 Adapter가 상속으로 강한 결합이 발생하고 Adaptee별로 Adapter를 만들어 주어야 합니다.

[참고]

[refactoring guru](https://refactoring.guru/design-patterns/adapter)