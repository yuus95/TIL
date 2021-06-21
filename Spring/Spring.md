# Spring TIL


## 결합

- 강한 결합: 객체 내부에서 다른 객체를 생성하는 것으로 강한 결합도를 가지는 구조

- 느슨한 결합: 객체를 주입 받는다는 것으로 외부에서 생성된 객체를 인터페이스를 통해 넘겨 받는것 

---

## 객체 컨테이너

- 스프링의 핵심 기능은 객체를 생성하고 초기화 하는 것이다.

- @Bean 애노테이션을 매서드에 붙이면 해당 메서드가 생성한 객체를 스프링이 관리하는 빈 객체로 등록한다.

- AnnotationConfigApplicationContext 클래스는 자바 클래스에서 정보를 읽어와 __객채 생성과 초기화를 수행한다.__ 

- ApplicationContext또는BeanFactory는 빈 객체의 생성, 초기화 , 보관, 제거 등을 관리하고 있어서 컨테이너라고도 부른다.


## 싱글톤 객체
- 스프링에서 별도 설정을 하지 않을 경우 스프링은 한 개의 빈 객체만을 생성하며, 이때 빈 객체는 싱글톤 범위를 갖는다고 표현한다.

- 한 개의 @Bean애노테이션에 대해 한 개의 빈 객체를 생성한다.


---

## 스프링 DI

- DI는 Dependency Injection의 약자로 의존 주입이라고 번역한다.

- 여기서 말하는 의존은 객체 간의 의존을 의미한다.
  - 한 클래스가 다른 클래스의 메서드를 실행할 떄 이를 '의존'한다고 표현한다.ㅣ
  - 의존은 변경에 의해 영향을 받는 관계를 의미한다.
  - MemberDao의 insert()메서드의 이름을 insertMember()로 변경하면 이 메서드를 사용하는 MemberRegisterService클래스의 소스 코드도 함께 변경된다.  이렇게 변경에 따른 영향이 전파되는 관계를 의존한다고 표현한다.

- 의존 대상 객체를 직접 생성
```Java
MemberRegisterService svc = new MemberRegisterService();

public class MemberRegisterService{
    private MemberDao memberDao = new MemberDao(); // 의존하는 MemberDao의 객체를 안에서 생성 
}

/*
유지 보수 관점에서 문제점을 유발 할 수 있다. 
*/
``` 

- DI 
```Java
MemberDao dao = new MemberDao(); 
// MemberDao dao = new MembercashDao(); 생성자만 변경하면 모든게 변경된다.
MemberRegisterService svc = new MemberRegisterService(dao);

public class MemberRegisterService{
    private MemberDao memberDao;


    public MemberRegisterService(MemberDao memberDao){
        this.memberDao=memberDao;
    }
    
/*
의존  객체를 전달받는 방식
즉 생성자를 통해 의존하고 있는 객체를 주입 받는 것
의존 객체를 직접 구하지 않고 생성자를 통해서 전달받기 떄문에 이 코드는 DI패턴을 따르고 있다.

장점: 변경에 유연하다.
생성한 dao에 대해서만 새롭게 생성하면 모든 부분에서 변경이 된다.

*/
``` 

- 캐시
  - 데이터 갑승ㄹ 복사해 놓은 임시 장소를 가리킨다. 보통 조회 속도 향상을 위해 캐시를 사용한다.

- 객체 조립기
  - 객체를 생성하고 의존 객체를 주입해주는 클래스를 따로 작성하는 곳으로 쓰이는 클래스
  - 의존 객체를 주입한다는 것은 서로 다른 두 객체를 조립한다고 생각할 수 있는데 이런 의미에서 이 클래스를 조립기라고도 표현한다.