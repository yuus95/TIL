# Optional 내용정리

```
Optional 테스트 진행방법
- Optional 을 이용하여 null일 경우 에러를 반환하는 로직 테스트

Optional<000dto> dto = Optional.empty();
Mockito.when(000mapper.fetchDto()).thenReturn(dto);

// 비즈니스로직
000Dto dto = mapper.fetchDto().orElseThrow( () -> BusinessException(000));
```