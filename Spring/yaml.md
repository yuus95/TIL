# yml 관련 내용 정리

## profile 설정

```java
spring:
    include: dev,...

```

##  하나의 yml파일로 여러개의 profile 정리하기

```java
spring:
    config:
        activate:
            on-profile: local
---
spring:
    config:
        activate:
            on-profile: prod

    datasource:
        ~
---

```

## profile 그룹화

```java
spring:
    profiles:
        group:
            "local":"testdb,com"
            "dev":"devdb,com"

---
spring:
    config:
        activate:
            on-profile:"testdb"
    datasource:
        ~
---
spring:
    config:
        activate:
            on-profile:"devdb"
    datasource:
        url:~
---
spring:
    config:
        activate:
            on-profile: "com"
server:
    prot: 8080
```

## jar 파일에서 프로필 적용시키기

```bash
java -jar \ -Dspring.profiles.active=dev \
```