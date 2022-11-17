## DateFormat vs JsonFormat

```
DateTimeFormat JsonFormat차이점

DateTimeFomat은 스프링부트에서 제공하는 LocalDate, LocalDateTime 의 포맷 라이브러리이다.
JsonFormat은 JSON을 직렬화 하기 위해 사용하는 JackSon라이브러리이다.


RequestParameter나 ModelAttribute에서는 JsonFormat을 사용해봤자 소용이 없다. 
JSON을 직렬화하는게 아니기 때문이다..

POST에 있는 RequestBody는 JSON을 직렬화해야되기 때문에 JSONFormat을 사용한다. 
```