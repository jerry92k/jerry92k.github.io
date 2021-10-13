---
layout: post
title:  Spring Session - HttpSession
date:   2021-10-11 00:00:00
categories: java
---


# Spring Session

- 스프링 세션을 이용하면 외부 저장 매체를 이용해서 여러 서버의 session을 쉽게 동기화 할 수 있습니다.
- 스프링 세션은 필터로 동작합니다.

## Features

Spring Session은 다음 세가지 기능을 지원합니다. 

- HttpSession

  application container(ex. servlet container)의 HttpSession 구현체 대신 Spring에서 지원하는 HttpSession 구현체

- WebSocket

  웹소켓 프로그래밍의 세션 관리 지원

- WebSession

  비동기 프로그래밍을 지원하는 Spring WebFlux의 WebSession 대체 기능을 지원



이 포스트에서는 HttpSession에 대해서 더 다루고자 합니다.

### HttpSession

#### application container에서 제공하는 HttpSession과의 차이점

application container의 HttpSession를 사용할 때에는 WAS에서 세션을 관리하게 됩니다. WAS의 세션 관리는 단일 서버에서는 문제될 것이 없지만, 서버(인스턴스)의 수가 늘어나면 사용자가 하나의 WAS로만 요청을 보내길 보장할 수 없기 때문에 WAS간 세션 동기화가 필요하게 됩니다. 이를 **세션 클러스터링**이라 합니다.

WAS의 세션클러스터링을 위해서는 설정파일에 공유 서버들의 IP, port 정보를 명시하여야 합니다. 그리고 이는, 새로운 서버 혹은 인스턴스가 추가될 경우에 매번 번거로운 수작업과 기존 설정파일의 수정이 동반됩니다. 또한 세션을 메모리에 저장하게 되니 갑작스런 사고가 발생하여 WAS에 문제가 생기게 되면 정보가 다 휘발되어 버립니다.

Spring Session은 외부 저장소와의 HttpSession의 연동을 지원하여 이 문제를 해결합니다. 관계형 혹은 Redis와 같은 캐시형 고속 저장소를 이용하여 세션을 보다 안전하게 보관하고 데이터 무결성을 보장합니다. 



### A. Spring - Spring Session - JDBC

#### Spring configuration
```java
@EnableJdbcHttpSession 
public class Config {

	@Bean
	public EmbeddedDatabase dataSource() {
		return new EmbeddedDatabaseBuilder() 
				.setType(EmbeddedDatabaseType.H2).addScript("org/springframework/session/jdbc/schema-h2.sql").build();
	}

	@Bean
	public PlatformTransactionManager transactionManager(DataSource dataSource) {
		return new DataSourceTransactionManager(dataSource); 
	}

}
```
- @EnableJdbcHttpSession 이 springSessionRepositoryFilter를 생성합니다.
- springSessionRepositoryFilter은 Filter 인터페이스를 implements 합니다.
- springSessionRepositoryFilter을 이용해 Servlet Container에서 생성하는 HttpSession을 대신하여 Spring Session이 HttpSession을 생성합니다.
- Servlet Container는 springSessionRepositoryFilter이 생성한 HttpSession을 사용하게 됩니다.


#### Config 클래스를 로드하기 위한 initialization
```java
public class Initializer extends AbstractHttpSessionApplicationInitializer { 

	public Initializer() {
		super(Config.class); 
	}

}
```

### B. Spring Boot - Spring Session - JDBC


#### application.properties

```properties
# 세션을 저장할 스토리지 타입
spring.session.store-type=jdbc 
# 세션 타임아웃 설정
server.servlet.session.timeout=
# 데이터베이스 생성 타입
spring.session.jdbc.initialize-schema=embedded 
# 데이터베이스 스키마가 저장될 경로
spring.session.jdbc.schema=classpath:org/springframework/session/jdbc/schema-@@platform@@.sql
# 데이터베이스에 세션을 저장할 테이블명
spring.session.jdbc.table-name=SPRING_SESSION
# 데이터베이스 호스트 주소. 데이터베이터명도 함께 명시
spring.datasource.url=
# 데이터베이스 접속 계정 ID
spring.datasource.username=
# 데이터베이스 접속 계정 비밀번호
spring.datasource.password=
```

- Spring Boot를 사용하면 Spring을 사용할 때 보다 간단합니다.
- Spring Boot는 위 설정 정보를 읽어서 @EnableJdbcHttpSession 효과와 동일하게 springSessionRepositoryFilter 빈을 생성하고 Servlet Container의 HttpSession을 대신합니다.


[참고]

- [spring.io - spring session](https://spring.io/projects/spring-session)
- [spring.io - spring session - java-jdbc](https://docs.spring.io/spring-session/docs/current/reference/html5/guides/java-jdbc.html)
- [스프링 세션 동작 원리](https://thecodinglog.github.io/spring-session/2020/08/07/filter-chain.html)
- [plateer open wiki](http://wiki.x2bee.com/pages/viewpage.action?pageId=8552454)

