
## graphql java 해석중 모르는 단어 정리

https://www.graphql-java.com/tutorials/getting-started-with-spring-boot //해석

```
brief 간략한 
barely 거의 (부정)
introduction into ~에대한 소개
retrieve 검색하다
in some way. 어떤면에서는 
It basically says: 기본적으로 다음과 같다.
introspect 반성하다
for everything else. 다른 모든 것들에 대해 
fetch 가져오다
parse 구문 분석하다
the same 동일하다
clause: 조항
어플리케이션 로그를 파일로그로 뺴야한다.
internal : 내부의 
underlying: 근본적인
prevalence:유행

```


- GraphQL은 서버로 부터 데이터를 받아오기 위한 Query Language이다.
- 데이터 요청법
```
{
  getUser(id: 10){
    id
    name
    Team {
      name
    }
  }
}
```
- JSON이 아니다. 

- GraphQl의 가장 중요한 점은 입력이 정적으로 입력된다.
- 서버에서는 너가 보낼 쿼리의 모양을 정확히 알아야 한다.(스카마로 정의해놔야 한다.)
    ```
    Type User{
        id:Long,
        name:String
    }

    Type Query{
        getUSer(id:Long):User
    }
    //  쿼리에 쓰이는 모든 내용이 스카마로 정의해놔야한다.(쿼리문,엔티티,input)
    ``` 
- Graphql Java는 HTTP 또는 JSON관련된 항목을 다루지 않는다.
- Graphql Java의 제일 중요한점은 그것 스스로 쿼리를 실행에만 관련이 있다.


- GraphQL Java 서버를 만드는 주요 단계는 다음과 같습니다.
    - GraphQL 스키마 정의.
    - 쿼리의 실제 데이터를 가져오는 방법 결정.
