# elasticsearch - opensearch


```
로그스테쉬와 연동하여 쓸수도 있고 그냥 직접적으로 로그를 받을수도 있다.
로그스테쉬를 사용하는 이유는 여러가지 로그의 형태를 받아 json으로 변환하여 보내기 위해서이고
스프링만 사용한다면 스프링에서 바로 오픈서치로 보내줘도 상관이 없다.
불필요한 진행단계를 넣을 필요는 없다.
```

## Logback Elasticsearch Appender 

SpringBoot Opensearch연동
목적
logstash에 log를 적재하지 않고 바로 Opensaerch(elasticSearch)로 보낼 수 있도록 한다.

 

라이브러리 설치
```
// https://mvnrepository.com/artifact/com.internetitem/logback-elasticsearch-appender
implementation("com.internetitem:logback-elasticsearch-appender:1.6")
```


```java

# logback-spring.xml
<?xml version="1.0" encoding="UTF-8"?>
<configuration scan="true" scanPeriod="30 seconds">

<!-- openSearch Appender추가하기-->
    <appender name="ELASTIC" class="com.internetitem.logback.elasticsearch.ElasticsearchAppender">
  
    <!-- url에 opensearch URL을 추가한다. 이 때 뒤에_bulk를 추가해줘야한다. -->
        <url>{url}/_bulk</url> 
         <!-- index추가하기  -->
        <index>logstash-%date{yyyy-MM-dd}</index>
        <headers>
            <header>
                <name>Content-Type</name>
                <value>application/json</value>
            </header>
        </headers>
    </appender>
    
    # root 에 ELASTIC appender추가하기
    <root level="info">
        <appender-ref ref="ELASTIC"/>
    </root>
</configuration>
```

테스트과정
로컬 환경에서 api하나를 작성하여 실제로 로그가 opensearch에 보내지는 확인한다.
아래 API는 하나의 예시이다.
 curl -X GET "localhost:8080/test"

```
package com.example.logstashsimple

import mu.KLogging
import mu.KotlinLogging
import org.springframework.web.bind.annotation.GetMapping
import org.springframework.web.bind.annotation.RestController

@RestController
class logTest {
    private val logger = KotlinLogging.logger {}
    @GetMapping("/test")
    fun test(){
        logger.info { "testLog" }
    }

    companion object : KLogging()
}
```
참고자료
https://github.com/internetitem/logback-elasticsearch-appender

