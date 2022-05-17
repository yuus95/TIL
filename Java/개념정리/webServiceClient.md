## WebServiceClient
```
Building a web service client
A web services client is an application capable of sending and receiving SOAP messages. Such an application serializes or deserializes the SOAP messages to a programming language type system enabling programmatic processing.

Here is the sequence for invoking a web service:
Client serializes the arguments of the method call into the XML payload of the SOAP message
Send the message to the web service
Wait for a response (or timeout)
Deserialize the XML payload in the response message to a local type or structure
Return that type/structure as a value from the method call.

// SOAP 메세지를 받거나 보낼 수 있게 해준다.
```


- SOAP는 다른 언어로 다른 플랫폼에서 빌드된 애플리케이션이 통신할 수 있도록 설계된 최초의 표준 프로토콜입니다. 프로토콜이기 때문에 복잡성과 오버헤드를 증가시키는 빌트인 룰을 적용하므로, 페이지 로드 시간이 길어질 수 있습니다. 그러나 이러한 표준은 빌트인 컴플라이언스를 제공한다는 의미이므로, 기업에서 선호하는 방식이기도 합니다. 빌트인 컴플라이언스 표준에는 보안과 안정적인 데이터베이스 트랜잭션의 기본 속성인 원자성, 일관성, 격리성, 내구성(Atomicity, Consistency, Isolation and Durability, ACID)이 포함됩니다.
    - [참고자료](https://www.redhat.com/ko/topics/integration/whats-the-difference-between-soap-rest)