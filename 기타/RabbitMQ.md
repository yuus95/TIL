## RabbitMQ


-참고
    - https://blog.dudaji.com/general/2020/05/25/rabbitmq.html
    - https://redis.io/

```
RabbitMQ is the most widely deployed open source message broker.


RabbitMQ is lightweight and easy to deploy on premises and in the cloud. It supports multiple messaging protocols. RabbitMQ can be deployed in distributed and federated configurations to meet high-scale, high-availability requirements.

```

- RabbitMq는 가장 널리 주입된 오픈소스 메세지 브로커이다
- 레빗은 가볍고 사내나 클라우드에 주입하기 쉽다. 그리고 다중 메세지 프로토콜을 지원한다.
- 레빗은 분산 및 연합 구성으로 배포할 수 있다. 

- 레빗은 AMQP를 따르는 오픈소스 메세지 브로커인데, 메세지를 많은 사용자에게 전달하거나, 요청에 대한 처리시간이 길 때, 해당 요청을 다른 API에게 위임하고 빠른 응답을 할 때 사용합니다.
- 애플리케이션 결합을 낮출 수 있다.
    - AMQP(Advanced Message Queuing Protocol, 어드밴스트 메시지 큐잉 프로토콜)는 메시지 지향 미들웨어를 위한 개방형 표준 응용 계층 프로토콜이다.

- 큐는 Producer들이 발생한 메세지들이 Consumer가 소비하기 전까지 보관되는 장소이다.

- 주요 단어 
    - Producer, Consumer, Queue, Exchange, Binding
    
    -  Producer: 메세지를 생성하고 발송하는 주체, 메세지는 Exchange를 통해 큐에 저장
    
    - Consumer : 메세지를 수신하는 주체,   Queue에 직접 접근하여 메세지를 가져온다.

    - Exchange : Producer들에게서 전달받은 메세지들을 어떤 Queue들에게 발송할지를 결정하는 객체(일종의 라우터)

    - Binding : Exchange에게 메세지를 라우팅 할 규칙을 지정하는 행위