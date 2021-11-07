---
layout: post
title:  RESTful API는 왜 지키기 어려울까
date:   2021-11-07 00:00:00
categories: se
---

지난 포스트에서 다룬 [REST와 RESTful API 탐구](https://jerry92k.github.io/se/2021/10/28/RESTful-API.html)에 이어서 이번 포스트에서는 **RESTful API는 왜 지키기 어려울까** 에 대해 다루고자 합니다.

### REST 아키텍처에는 6가지 제한 조건이 존재합니다. 

#### - 지난 포스트 참고( [REST와 RESTful API 탐구](https://jerry92k.github.io/se/2021/10/28/RESTful-API.html))

1. Client–server architecture(클라이언트 서버 아키텍처)

2. Resources and Representations

3. Uniform Interface(일관된 인터페이스)

4. Stateless(무상태)

5. Cacheability

6. Layered system
  


대부분의 조건은

**1)서버-클라이언트 구조의 웹서비스 아키텍처에서**

**2)REST 가이드에 따라 자원 중점적 URL 패턴을 사용하고**

**3)HTTP 프로토콜을 사용한다면**

만족하게 됩니다.



### 문제가 부분은 **Uniform Interface(일관된 인터페이스)** 입니다.

Uniform Interface는 다시 4가지의 세부 제한 조건으로 이루어져 있습니다.

- Identification of resources
  - URI로 리소스 식별

- Manipulation of resources through representations
  - 클라이언트의 요청 해당 시점의 리소스를 클라이언트와 호환가능한 표준화된 representation으로 변환하여 전송
- Self-descriptive messages
  - 메세지는 스스로를 설명해야한다.
- Hypermedia as the engine of application state(HATEOAS)
  - 애플리케이션의 상태는 Hyperlink를 이용해 전이되어야 한다.

이 중 어려운 부분이 **Self-descriptive messages**와 **HATEOAS** 입니다.



#### 예시로 함께 보는 **Self-descriptive messages**,  **HATEOAS**  지키는 법

일반적으로 아래와 같은 형식을 RESTful API로 많이 사용하곤 합니다.

```json
HTTP/1.1 200 OK
Content-Type: application/json

[ { "title": "interstella", "director": "Christopher Nolan" } ]
```

위의 메세지는 self-descriptive와 HATEOAS 조건을 만족하지 못하는 전형적인 예 입니다.



#### Self-descriptive messages

: 메세지가 담고 있는 정보에 대해 메세지 스스로 이해할 수 있는 정보를 포함해야 하는 것을 뜻합니다.

1)HTTP 1.1 프로토콜을 사용하고, 2)응답 상태는 200-OK, 3)representation type은 json이라는 정보가 담겨있지만

body의 정보를 어떻게 해석해야 하는지에 대해서는 알 수가 없습니다.

클라이언트 입장에서는 title, director이 뜻하는 바를 해석할 수 없고 단순 문자열로 생각할 수 밖에 없습니다.

 

**message body의 정보를 self-desciptive 하기 위해서는 두가지 방법이 있습니다.**

1. **Media Type :** HTTP Header의 Content-Type으로 전달되는 Media Type에 정보를 포함할 수 있습니다.

>  Media Type 규약에 맞춰 커스텀 Media Type을 작성 후 [IANA - Media Type 공식 저장소](http://www.iana.org/cgi-bin/mediatypes.pl)의
>
> vnd.* tree 밑으로 등록을 신청하면 일주일 정도 소요

```json
HTTP/1.1 200 OK
Content-Type: application/vnd.movie+json

[ { "title": "interstella", "director": "Christopher Nolan" } ]
```
  


2. **헤더 정보의 Link 이용 **: profile relation을 이용하여 명세 전달

```json
HTTP/1.1 200 OK
Content-Type: application/json
Link:<https://example.com/docs/movie>; rel="profile"

[ { "title": "interstella", "director": "Christopher Nolan" } ]
```

> 클라이언트가 Link헤더(RFC 5988)와 profile(RFC 6906)을 이해해야 한다.



#### HATEOAS

> HATEOAS란 서버가 클라이언트에게 하이퍼 미디어를 통해 정보를 동적으로 제공해주는 것을 뜻합니다.
>
> 클라이언트는 서버와의 인터렉션을 위한 정보를 사전에 가지고 있지 않아도 되됩니다.



HATEOAS를 잘지키는 것으로 HTML을 예시로 보면 \<a> 태그의 링크로 다음 가능한 상태를 나타내고 이동할 수 있게 해줍니다.

> ````html
> ```
> <a href="https://example.com">다음 보기</a>
> ```
> ````



**HATEOAS를 만족하지 않는 응답 메세지의 문제점**

위의 HTML에 반해, 아래 예시에서는 다음 가능한 interaction 정보가 하이퍼 미디어에 포함되어있지 않습니다.

```json
HTTP/1.1 200 OK
Content-Type: application/vnd.movie+json

[ { "title": "interstella", "director": "Christopher Nolan" } ]
```

클라이언트가 다음 상태에 대한 정보를 서버에서 받지 못하면, 다음 상태에 대한 정보를 클라이언트에서 관리할 수 밖에 없습니다.

이는 1)클라이언트와 서버의 Action Flow가 강하게 결합될 수밖에 없게 만들고 2)클라이언트에서 다음 상태 정보를 관리해야 하므로(소스코드 하드코딩이나 클라이언트 로컬DB에), url이 변경될 경우 서버와 클라이언트가 함께 수정해야하는 영향도 있습니다.



**HATEOAS를 지키기 위해선 아래와 같은 방법을 적용할 수 있습니다.**

1. **헤더 정보의 Link 이용 **

```json
HTTP/1.1 200 OK
Content-Type: application/json
Link:</movie/5>; rel="next",
			</movie/3>; rel="prev"
```



2. **body에 link정보 함께 제공**

```json
HTTP/1.1 200 OK
Content-Type: application/json

[ "result" : { "title": "interstella", "director": "Christopher Nolan" },
	"links":{
    "next":"/movie/5",
    "prev":"/movie/3"
  }	
]
```


[참고]

[Self descriptive 와 HATEOS / 대부분 못 지키고 있는 REST 제약조건](https://ecsimsw.tistory.com/entry/REST-API-Self-descriptive%EC%99%80-HATEOS-%EB%8C%80%EB%B6%80%EB%B6%84-%EB%AA%BB-%EC%A7%80%ED%82%A4%EA%B3%A0-%EC%9E%88%EB%8A%94-%EC%A0%9C%EC%95%BD%EC%A1%B0%EA%B1%B4)

[그런 REST API로 괜찮은가](https://velog.io/@kjh03160/%EA%B7%B8%EB%9F%B0-REST-API%EB%A1%9C-%EA%B4%9C%EC%B0%AE%EC%9D%80%EA%B0%80)

[What RESTful actually means](https://codewords.recurse.com/issues/five/what-restful-actually-means)

[Web REST API 설명](https://wordbe.tistory.com/entry/Web-Rest-API-%EC%84%A4%EB%AA%85)

[wiki - HATEOAS](https://en.wikipedia.org/wiki/HATEOAS)

