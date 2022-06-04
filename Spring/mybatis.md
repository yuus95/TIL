## mybatis


### What is Mybatis?
```
JDBC를 통해 데이터베이스에 엑세스하는 작업을 캡슐화하고 일반 SQL쿼리, 저장 프로 시저 및 고급 매핑을 지원하며 모든 JDBC 코드 및 매개 변수의 중복 작업을 제거한다.
복잡한쿼리나 다이나믹한 쿼리에 강합니다.

jdbc 코드들을 거의 제거해서 우리가 작성할 필요가 없다.
MyBatis는 xml또는 어노테이션으로 간단하게 사용할 수 있다.

MyBatis is a first class persistence framework with support for custom SQL, stored procedures and advanced mappings. 
```


### mybatis를 사용하는 이유
[참고자료](https://ofstack.com/Java/41020/difference-and-performance-comparison-between-mybatis-and-jpa.html)

- mybatis를 사용하면 jpa를 사용하는것보다 퍼포먼스가 더 좋기 떄문이다.

```
위에블로그내용을 요약하면
대량의 데이터에 대하여 insert를 했을 때 성능이 얼마나 차이나는지에 대하여 실험을 한 내용이다.
결과적으로 jpa 구현체는 리스트의 내용을 한번에 saveAll을 했을 경우
mybatis보다 느린 결과값이 나오게 되는 이유는 saveAll로직에서는 하나의 리스트를 또 만들어 각각 인스턴스를 save함수를 실행시키기 떄문이다.
```


### 간단하게 실행방법 작성

- application.yml

```

spring:
  datasource:
    driver-class-name: org.mariadb.jdbc.Driver
    url: jdbc:mariadb://localhost:3306/test?characterEncoding=UTF-8&serverTimezone=UTC
    username: {username}
    password: {password}

mybatis:
  config: mybatis-config.xml
  type-aliases-package: com.example.softsq.model # dto 모인곳 
  mapper-locations: mybatis/mapper/*.xml # mapper.xml 파일 위치 
```

- mybatis-config.xml
    - mybatis conifg파일 매퍼파일들을 등록해줘야한다.
```
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE configuration PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-config.dtd">
<configuration>
    <mappers>
        <mapper resource="TestMapper.xml"/>
    </mappers>
</configuration>
```

- com.example.softsq.model.{dto명} ex) Dto
    - 매퍼에서 반환한 결과값을 저장하기위한 용도

```
package com.example.softsq.model;
@Getter
@Setter
@NoArgsConstructor
@AllArgsConstructor
public class Dto {
    private int id;
    private String name;
}
```

- com.example.softsq.model.{Dao}
    - Mapper.xml과 매핑되는 인터페이스
        - 여기에 쿼리를 직접 사용해도된다.
```
@Repository
@Mapper
public interface Dao {
    List<Dto> getAllDto() throws Exception;
}
```


- TestMapper 
    - 실제 sql문을 작성을하는 xml파일

```java 
<?xml version="1.0" encoding="UTF-8"?> # xml유효성검사를 위해 필요하다.
<!DOCTYPE mapper PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-mapper.dtd">

<mapper namespace="com.example.softsq.model.Dao"> # Dao 패키지위치까지작성
    <select id="getAllDto" resultType="Dto"> # Yml에서 dto의 위치를 지정해줘서 패키지위치까지는 작성안해도된다.
        SELECT * FROM member;
    </select>

</mapper>
```