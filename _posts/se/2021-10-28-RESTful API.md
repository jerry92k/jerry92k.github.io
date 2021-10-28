---
layout: post
title:  RESTful API
date:   2021-10-28 00:00:00
categories: se
---

## RESTful한 API인가?

다음 API 예제는 RESTful할까요?

```url
GET /users/1 HTTP/1.1
Host: api.example.com
```



"REST는 자원 중심적인 요청과 응답" 관점에서 보면,

위의 요청은 서버에서 관리하는 자원 중 첫번째 유저에 대한 요청을 보내고 있으니 RESTful API로 보일 수 있습니다.

하지만, RESTful을 조금 더 명확하게 살펴보면 위의 문제는 RESTful 하다고 하기에는 정보가 부족합니다.



## REST(Representational State Transfer)란?

**REST는 통신 프로토콜이 아닙니다.**

REST는 소프트웨어 아키텍처의 한 형식으로 다음과 같은 제한 조건을 가지고 있습니다.

이 제한 조건을 준수하는 시스템을 RESTful 하다고 말합니다.



### REST 아키텍처에 적용되는 6가지 제한 조건



#### 1. Client–server architecture(클라이언트 서버 아키텍처)

클라이언트-서버 아키텍처를 준수해야합니다. 클라이언트-서버 디자인 패턴은 사용자 인터페이스에 대한 관심과 데이터를 저장하고 다루는 것의 관심을 분리함으로써 관심사 분리 원칙을 지킵니다. 

이에 따라, 서버와 클라이언트는 서로 간 의존을 최소화여 독립적으로 존재할수 있게 됩니다.

클라이언트 측면에서는 사용자 인터페이스의 이식성(portability)이 향상되고, 서버 측면에서는 컴포넌트들을 심플하게 하여 확장성(scalability)이 보장되는 장점이 있습니다.



#### 2. Resources and Representations

RESTful 시스템의 핵심은 자원(resource) 입니다. 자원은 웹페이지, 비디오 스트림, 이미지와 같은 것들이 될 수 있습니다. 자원은 서버에서 관리하는 데이터 베이스의 유저 목록이나 특정 지역의 날씨와 같은 추상적인 개념도 될 수 있습니다.

중요한 점은, 시스템에 있는 모든 자원은 유니크하고 식별 가능해야 합니다.

서버는 클라이언트의 요청에 따라 자원을 특정한 표현 형식(representation)[ex)Text, JSON, XML...]으로 가공하여 클라이언트로 전송합니다. 서버는 자원의 특정 representation을 강제하지 않습니다. 클라이언트가 동일 자원에 대해 json 혹은 xml representation 타입으로 요청하면 서버는 그에 맞는 형식으로 결과를 반환해주어야 합니다. 클라이언트가 이해할 수 있는 포맷으로 전달해 주는 것이 REST 서버의 책임입니다.

**representation : xml인 경우**

```xml
<user> 
   <id>1</id> 
   <name>jerry</name>
</user> 
```

**representation : json인 경우**

```json
{ 
   "id":1, 
   "name":"jerry", 
}
```



자원의 representation 형식을 정의할 때는 다음 사항을 고려해야 합니다.

- **Understandability** : 서버와 클라이언트가 모두 이해할 수 있어야 하고 활용할 수 있어야 합니다
- **Completeness** : representation 형식은 자원을 완전하게 나타낼 수 있어야 합니다. 예를 들어 자원이 다른 자원을 포함하는 경우에도 이를 나타낼 수 있어야 합니다.
- **Linkablity** : 자원은 다른 자원으로의 연결점을 가질 수 있습니다. 이러한 것도 표현이 가능해야합니다.



#### 3. Uniform Interface(일관된 인터페이스)

uniform interface는 인터페이스를 표준화하여 클라이언트 플랫폼과 무관하게 항상 동일한 인터페이스를 제공하는 것을 뜻합니다. 이는 SOAP 같은 통신 방식과 달리 클라이언트와 서버간 직접적인 인터페이스 결합을 없애서 하나의 API로 다중 클라이언트로 구현 가능한 등의 이점을 가지고 있습니다. 



#### 4. Stateless(무상태)

RESTful 시스템의 모든 기능들은  무상태여야 합니다.

이것은 클라이언트에서 서버로의 모든 요청은 서로간에 공유하는 특정 상태 값에 의존 없이 행해야합니다. 또한 서버로의 모든 요청은 



#### 5. Cacheability

잘 정의된 캐싱은 서버-클라이언트 간 상호 작용을 줄여주고 성능을 향상합니다.

> 하지만 잘못된 캐싱의 경우 변경된 자원의 상태와 호환되지 않는 요청을 일으킬 수 있으므로 캐싱을 적용할지 유무를 잘 판단하여 명시하여야 합니다.

REST는 필요에 따라 클라이언트 캐싱을 사용할 수 있도록 지원해야합니다.



#### 6. Layered system

클라이언트는 서버로의 요청 중간에 다른 계층이 존재해도 영향 받지 않아야 합니다. 

예를 들어, 프록시, 로드밸런서, 보안 계층과 같은 것이 클라이언트와 서버 사이에 존재해도, 클라이언트나 서버의 코드를 변경할 필요가 없어야 하고  통신에도 영향이 없어야 합니다. 



## HTTP

HTTP는 인터넷 통신 표준 프로토콜입니다.

REST는 통신 프로토콜이 아닙니다. 그리고 HTTP 프로토콜을 사용해야만 하는 것이 아니기 때문에 FTP, SMTP 등을 사용하여도 상관이 없습니다.



## RESTful API와 HTTP API : Applied to web services 

REST 아키텍처를 준수한 Web service API를 RESTful API라고 합니다.

**RESTful API가 반드시 HTTP 프로토콜을 사용해야하는 것은 아닙니다.**

하지만, 대부분의 RESTful API들이 HTTP 프로토콜 기반으로 서비스합니다. 
HTTP 프로토콜이 REST 아키텍처에 잘 부합하고 HTTP가 인터넷 표준 프로토콜이기 때문에 대부분의 Web Service들이 HTTP를 사용하기 때문입니다.

따라서, **HTTP API를 REST 아키텍처를 잘 준수하여 설계하면 RESTful API**라 부를 수 있습니다.

HTTP 기반의 RESTful API는 아래와 특징을 가집니다.

- base URI : http://api.example.com/
- HTTP 표준 메서드 사용
  - GET : 타겟 자원의 특정 상태를 representation으로 요청
  - POST : 서버로의 요청 representation에 타겟 자원이 특정한 처리를 하도록 명시하여 요청
  - PUT : 서버로의 요청 representation에 타겟 자원의 특정 상태를 명시하여 서버에 생성하거나 변경할 것을 요청 
  - DELETE : 타겟 자원의 상태를 삭제할 것을 요청

- [media type](https://en.wikipedia.org/wiki/Media_type) 명시 : 자원의 상태를 반환해줄 representation 지정



[참고]

[baeldung - rest-vs-http](https://www.baeldung.com/cs/rest-vs-http)

[tutorialspoint - restful resources](https://www.tutorialspoint.com/restful/restful_resources.htm)

[wiki - REST](https://en.wikipedia.org/wiki/Representational_state_transfer)

