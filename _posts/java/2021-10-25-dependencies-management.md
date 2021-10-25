---
layout: post
title:  의존 라이브러리의 버전 관리를 편리하게 할 순 없을까?
date:   2021-10-25 00:00:00
categories: java
---


빌드 도구들은 설정파일에 remote repository와 외부라이브러리의 종류와 버전 등 정보를 명시해주면 자동으로 라이브러리를 다운 받고 의존관계를 설정할 수 있습니다.

> (이전 포스트 참고: **[Java Build Tools - Ant, Maven 그리고 Gradle](https://jerry92k.github.io/java/2021/10/24/Java-Build-Tools.html)** )



#### 의존 라이브러리가 많아지면 어떻게 될까요? 

 \- 호환성이 문제되지 않게 버전 설정에 주의해야합니다. 라이브러리들 간에도 서로를 참조하는 경우가 많기 때문에 임의로 한 라이브러리 버전을 올려버릴 경우 문제가 생길 수 있습니다.

 \- 예를 들어,  라이브러리 A(v1.0), B(v1.0), C(v1.0) 가 있고 A -> C ,  B -> C 의존 관계를 가진다고 가정해봅시다. 이후, C가 v.2.0을 출시합니다. 버전이 달라지면서 많은 부분이 변경되었지만 개발자는 최신 버전을 적용하고자 임의로 C의 버전을 올립니다. 이때, C(v2.0)의 변경된 부분이 A(v1.0)와 B(v1.0)가 참조하는 것과 관련이 있다면 컴파일 에러가 날 가능성이 큽니다.

=> 이렇듯 특정 라이브러리의 버전을 하나를 올리는 것은 다른 라이브러리들과의 호환성 문제 때문에 간단한 작업이 아닙니다. 버전을 올리려면 충분히 테스트를 해보아야하고, 호환되지 않는다면 다른 라이브러리들도 호환되게 업데이트가 되는 것을 모니터링 했다가 한번에 버전을 올리는 식으로 접근해야 합니다.



#### BOM은 이를 간편하게 해줍니다.

BOM(Bill Of Materials)는 Maven에서 사용하는 설정파일 pom의 일종으로,

의존 라이브러리들의 호환되는 버전을 명시해두어 개발자가 별도로 호환성을 테스트하고 검증하는 수고를 덜어줍니다.

프로젝트에서 BOM을 사용하면, 개별 라이브러리에 대한 버전을 별도로 명시하지 않아도 됩니다.

라이브러리 업데이트가 필요하면 BOM의 버전만 변경해주면 되므로 간편하고 안전합니다.



#### Spring은 BOM을 적극 활용합니다.

Maven 설정

```xml
<dependencyManagement>
    <dependencies>
        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-framework-bom</artifactId>
            <version>5.2.18.RELEASE</version>
            <type>pom</type>
            <scope>import</scope>
        </dependency>
    </dependencies>
</dependencyManagement>
```

Gradle 설정

```groovy
framework-bom
implementation group: 'org.springframework', name: 'spring-framework-bom', version: '5.2.18.RELEASE', ext: 'pom'
```

>  참고: [spring-framework-bom 5.2.18.RELEASE pom.xml](https://repo1.maven.org/maven2/org/springframework/spring-framework-bom/5.2.18.RELEASE/spring-framework-bom-5.2.18.RELEASE.pom)



위와 같이 BOM을 설정하게 되면 라이브러리 정보에 버전을 따로 명시 하지 않아도 됩니다.

```xml
<dependencies>
    <dependency>
        <groupId>org.springframework</groupId>
        <artifactId>spring-context</artifactId>
    </dependency>
    <dependency>
        <groupId>org.springframework</groupId>
        <artifactId>spring-web</artifactId>
    </dependency>
<dependencies>
```

**Gradle도 maven repository에 있는 BOM을 사용할 수 있습니다.**



#### Spring Boot 의존성 BOM 설정

Maven 설정

```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-parent</artifactId>
    <version>2.5.6</version>
    <type>pom</type>
</dependency>
```

Gradle 설정

```groovy
implementation group: 'org.springframework.boot', name: 'spring-boot-starter-parent', version: '2.5.6', ext: 'pom'
```

> spring-boot-starter-parent은 실제로는 spring-boot-dependencies를 상속하는데, 이 spring-boot-dependencies에 라이브러리의 버전 정보들이 관리되어있습니다.

스프링 부트에서 버전을 일관성있게 다루는 방법 (참고 : BOM)





[참고]

[Spring with Maven BOM](https://www.baeldung.com/spring-maven-bom)

[Maven - dependency mechanism](https://maven.apache.org/guides/introduction/introduction-to-dependency-mechanism.html)

[Maven - spring-framework-bom](https://mvnrepository.com/artifact/org.springframework/spring-framework-bom/5.2.18.RELEASE)

[Maven - spring-boot-starter-parent](https://mvnrepository.com/artifact/org.springframework.boot/spring-boot-starter-parent)

