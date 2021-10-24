# Mock 개념

## Mock이란

- 모의 객체란 주로 객체 지향 프로그래밍으로 개발한 프로그램을 테스트할 경우 테스트를 수행할 모듈과 연결되는 외부의 다른 서비스나 모듈들을 실제 사용하는 모듈을 사용하지 않고 실제의 모듈을 "흉내"내는 가짜 모듈을 작성하여 테스트의 효용ㅇ성을 높이는 데 사용하는 객체이다. 사용자 인터페이스나 데이터 베이스 테스트 등과 같이 자동화된 테스트를 수행하기 어려울 때 널리 사용된다.


## Mockito 

- Mockito란
```bash
Mock을 쉽게 만들고 mock의 행동을 정하는 stubbing,정상적으로 작동하는지에 대한 verify등 다양한 기능을 제공해주는 프레임워크
```

- @Mock ,@InjectMock
    - @Mock: 가짜 객체
    - @InjectMock은 DI를 @Mock이나 @Spy로 생성도니 mock객체를 자동으로 주입해주는 어노테이션입니다.
- Stub
```bash
Stubbing은 stub이라고도 하며 토막, 남은 부분이라는 뜻을 가지고 있다.

>> 테스트 스텁은(Test Stub)은 테스트 호출 중 테스트 스텁은 테스트 중에 만들어진 호출에 대해 미리 준비된 답변을 제공하는 것이다.

>> 만들어진 mock 객체의 메소드를 실행했을 때 어떤 리턴 값을 리턴할지를 정의해준다.
ex) When(memberRepository.findById(1)).thenReturn(member);
```

- @Spy 
```bash
@Spy로 만든 mock객체는 진짜 객체이며 메소드 실행 시 스터빙을 하지 않으면 기존 객체의 로직을 실행한 값을, 스터빙을 한 경우엔 스터빙 값을 리턴합니다.
```
- @Spy 에시
```java
public class MemberTestService{
    public member getMember(){
        return new Member(); // Member 리턴
    }
}

```


- 에시
```java
// @Mock으로 만든 객체는 가짜 객체이며 그 안에 메소드 호출해서 사용하려면 반드시 스터빙을 해야한다.

// 스터빙을 하지 않으면 primitive type은 0 참조형은 null을 반환합니다.
import org.assertj.core.api.Assertions;


@RequiredArgsConstructor
@Service
public class MemberGetService{
    private final MemberRepository memberRepository;
    public Member getMember(Long id){
     Member member =   memberRepository.findById(id).orElseThrow(()-> new CustomError());
     return member;
    }
}

// 테스트
@ExtendWith(MockitoExtension.class)
public class MemberGetService{
    @Mock
    MemberRepository memberRepository;

    @InjectMock
    MemberGetService memberGetService;

    void memberGet테스트(){
        Member member = new Member();
        Assertions.assertNull(memberRepository.findById(1)); // true 스터빙을 하지 않아 널값이 나온다.
    }

}


```
