# logback  - log4j2

```
spring boot 에서 opensearch로 바로 로그를 보내기 위해서는 라이브러리를 이용해야한다.
기존에는 logstash관련 라이브러리를 이용하여  logstsh를 통해 opensearch로 로그를 보냈지만  log4j2-elastic 라이브러리를 이용하면 바로 보낼 수 있다.
logback관련 elasticsearch 라이브러리는 3년이상 개발이 진행이 안되고 있다.
log4j2관련 elasticsearch 라이브러리는 현재까지도 개발이 진행되고 있어 보안상 이슈가 logback보다는 안전하다.

 기존 앱들의 로그 처리과정을 전부 변경해야하는점
logback의 스프링 프로필 이용하는 과정과 log4j2의 스프링 프로필 이용하는 과정도 다르다.

opensearch로 다이렉트로 보내기 위해 모든 과정을 처리하는 리소스와 logstash을 이용하는 비용을 고민해야 한다는 점이있다.

```
## logback  장단점 적어놓기
- 스프링 프로필을 사용할 때 편리하다. (하나의 파일안에서 프로필 관련내용을 모두 정리할 수 있다.)
## log4j2 장단점 적어놓기
- 비동기식으로 로그를 보낼 수 있다.