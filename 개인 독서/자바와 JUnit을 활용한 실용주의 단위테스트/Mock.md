# Mock 개념

## Mock이란

- 모의 객체란 주로 객체 지향 프로그래밍으로 개발한 프로그램을 테스트할 경우 테스트를 수행할 모듈과 연결되는 외부의 다른 서비스나 모듈들을 실제 사용하는 모듈을 사용하지 않고 실제의 모듈을 "흉내"내는 가짜 모듈을 작성하여 테스트의 효율성을 높이는 데 사용하는 객체이다. 사용자 인터페이스나 데이터 베이스 테스트 등과 같이 자동화된 테스트를 수행하기 어려울 때 널리 사용된다.


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

>> 테스트 스텁은(Test Stub)은  "테스트 중에 만들어진 호출에 대해 미리 준비된 답변"을 제공하는 것이다.

>> 만들어진 mock 객체의 메소드를 실행했을 때 어떤 리턴 값을 리턴할지를 정의해준다.
ex) When(memberRepository.findById(1)).thenReturn(member);
# 리턴값을 정하지 않으면  primitive type은 0 참조형은 null을 반환합니다.
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
//멤버 클래스
public class Member{
    String name;

    public Member(){
        this.name "test";
    }
}

// 테스트 코드

public TestClass{
    @Spy
    MemberTestService memberTestService;


    publi void Test1(){
        Member member = memberTestService.getMember();
        Assertions.assert(member.name).isEqualto("test");
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



## 스터빙

### 1.Mockito에서 스터빙하는 방법

- OngoingStubbing 메소드

```bash
when에 넣은 메소드의 리턴 값을 정의해주는 메소드

#사용방법
when(member.getUser()).(OngoinStubbing 메소드);

#주의사항
"void반환 타입 메서드는 사용 불가"

# 메소드
thenReturn
>> "스터빙한 메소드 호출 후 어떤 객체를 리턴할 건지 정의"

thenThrow
>>"스터빙한 메소드 호출 후 어떤 EXception을 Throw할 건지 정의"

thenCallRealMethod
>> "실제 메소드 호출"
```


- Stubber 메소드
```bash
#의미
"OngoingStubbing 과 다르게 when에 스터빙할 클래스를 넣고 그 후에 메소드를 호출"

#사용방법
{Stubber 메소드}.when({스터빙 할 클래스}).{스터빙할 메소드}

# 특징
void메소드도 리턴값을 받을 수 있다.

#메소드 
doReturn
>> 스터빙 메소드 호출 후 어떤 행동을 할 건지 정의

doThrow
>> 스터빙 메소드 호출 후 어떤 Exception을 throw할 건지 정의

doNothing
>> 스터빙 메소드 호출 후 어떤 행동도 하지 않게 정의

doCallRealMethod
>> 실제 메소드 호출
```



### 2.예시

```java
List temp = new ArrayList<>();

whien(temo.get(0)).thenReturn("foo"); // 오류

doReturn("foo").when(temp).get(0); // 정상적으로 "foo 반환"

```


## Verify

- 스터빙한 메소드가 실행됐는지, n번 실행됐는지, 실행이 초과되지 않았는지 등 다양하게 검증해 볼수 있습니다.

```bash
#사용 방법
verify(T mock, VerificationMode mode) 

#Verification Mode
>> 검증할 값을 정의하는 메소드
```

- 메소드

```bash
# 메소드
times(n)
>> 몇 번이 호출됐는지 검증

never
>>한 번도 호출되지 않았는지 검증

atLeastOne
>> 최소 한 번은 호출 됐는지 검증

atLeast(n) 
>> 최소 n번이 호출됐는지 검증

atMostOnce
>> 최대 한 번이 호출 됐는지 검사

atMost(n)
>> 최대 n번이 호출 됐는지 검사

Only 
>> 해당 검증 메소드만 실행됐는지 검증

timeOut(long mills)
>> n ms이상 걸리면 Fail 그리고 바로 검증 종료

after(long mills)
>> n ms이상 걸리는지 확인 "timeout과 다르게 시간이 지나도 바로 검증 종료가 되지 않는다."

desciption
>> 실패한 경우 나올 문구 
```


## BDD란?

```bash
TDD(Test Driven Development)는 "테스트"를 기준으로 하는 개발 방법론입니다.
BDD(Behavior Driven Development)는 "행동"을 기준으로 하는 개발 방법론입니다. TDD를 참고
```


- 구조

```bash
#Given 
>> 초기 context값

#When
>> 테스트 하려는 조건

#Then
>> 테스트 결과
```

- 예시

```java
@Test
public void TestCode(){
    //given - 초기 설정값 
    // when - > given(stub을 할 때 when 대신 given을 사용한다.)
    given(memberTestService.getMember()).thenReturn(member);

    //when - 테스트 하려는 조건
    memberTestService.getMember();

    //Then
    //verify 검증
    then(memberTestService).getMember();

}
```