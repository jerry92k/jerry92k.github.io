---
layout: post
title:  Spring Boot - AutoConfiguration
date:   2021-10-26 00:00:00
categories: java
---

## Auto Configuration?

Spring Boot의 "auto configuration"은 프로젝트 클래스 패스에 위치한 의존 관계에 있는 jar들의 기본 설정과 빈 생성을 자동으로 도와줍니다.

"auto configuration"을 활성화 하기 위해서는 ```@EnableAutoConfiguration``` 혹은 ```@SpringBootApplication``` 어노테이션을 사용합니다.

> @SpringBootApplication 어노테이션은 @EnableAutoConfiguration을 포함하고 있습니다.



### Spring.factories

Spring Boot는 Spring으로 개발 시에 자주 사용하는 라이브러리들의 설정파일들을 만들어 놓았습니다.

Spring Boot의 의존 라이브러리 중 ```spring-boot-autoconfigure-2.5.5.jar```의 ```spring.factories``` 파일에

다음은 설정이 정의되어 있습니다.

EnableAutoConfiguration를 true로 활성화하면, 클래스 패스에 존재하는 라이브러리인 경우 아래의 설정이 적용됩니다.

```yml
# Auto Configure
org.springframework.boot.autoconfigure.EnableAutoConfiguration=\
org.springframework.boot.autoconfigure.admin.SpringApplicationAdminJmxAutoConfiguration,\
org.springframework.boot.autoconfigure.aop.AopAutoConfiguration,\
org.springframework.boot.autoconfigure.amqp.RabbitAutoConfiguration,\
org.springframework.boot.autoconfigure.batch.BatchAutoConfiguration,\
org.springframework.boot.autoconfigure.cache.CacheAutoConfiguration,\
org.springframework.boot.autoconfigure.cassandra.CassandraAutoConfiguration,\
org.springframework.boot.autoconfigure.context.ConfigurationPropertiesAutoConfiguration,\
org.springframework.boot.autoconfigure.context.LifecycleAutoConfiguration,\
org.springframework.boot.autoconfigure.context.MessageSourceAutoConfiguration,\
org.springframework.boot.autoconfigure.context.PropertyPlaceholderAutoConfiguration,\
org.springframework.boot.autoconfigure.couchbase.CouchbaseAutoConfiguration,\
org.springframework.boot.autoconfigure.dao.PersistenceExceptionTranslationAutoConfiguration,\
...
```

> Spring Boot에서 미리 정해놓은 설정 외에 직접 설정파일을 만들어서 AutoConfiguration과 연동할 수 도 있습니다.



### @Conditional

Spring 4에서는 @Conditional을 도입하였습니다.

@Conditional은 특정 조건에 따라 bean의 생성을 다르게 컨트롤 할 수 있어 보다 유연하게 bean을 생성 합니다.



#### Conditional 종류

- 클래스 패스에 특정 클래스가 있는지 체크
- ApplicationContext에 생성되지 않은 bean인지 체크
- 특정 위치에 특정 파일이 없는지 체크
- 설정파일에 특정 property 값이 있는지 체크
- 특정 시스템 property가 있는지 체크



[참고]

[dzone - How Spring Boot Auto-Configuration Works](https://dzone.com/articles/how-springboot-autoconfiguration-magic-works)

[javadevjournal - How Spring Boot auto-configuration works](https://www.javadevjournal.com/spring-boot/how-spring-boot-auto-configuration-works/)

[javadevjournal - Spring Boot Auto Configuration](