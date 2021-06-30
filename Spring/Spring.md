# Spring TIL


## 결합

- 강한 결합: 객체 내부에서 다른 객체를 생성하는 것으로 강한 결합도를 가지는 구조

<<<<<<< HEAD
- 느슨한 결합: 객체를 주입 받는다는 것으로 외부에서 생성된 객체를 인터페이스를 통해 넘겨 받는것



---
## JPA

- API 값 리턴
  - 엔티티를 직접 노출하는 것은 좋지 않다.

- 항상 지연로딩을 기본으로 하고, 성능 최적화가 필요한 경우에는 페치 조인을 사용해라.
=======
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

##  DI

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


## 스프링 DI

- 스프링은 DI를 지원하는 조립기이다.

- 필요한 객체를 생성하고 생성한 객체에 의존을 주입한다

```java

@Configuration // 스프링 설정 클래스를 의미한다.  이 어노테이션이 있어야  스프링 설정 클래스로 사용할 수 있다.
public class AppCtx{
  @Bean // 해당 메서드가 생성한 객체를 스프링 빈이라고 설정한다.  메서드 이름을 빈 객체의 이름으로 사용한다.
  public MemberDao memberDao(){
    return new MemberDao();
  }
}

// 스프링 컨테이너 생성
ApplicationContext ctx =new AnnotationConfigApplicationContext(AppCtx.class);

```

- 객체를 생성하고 의존 객체를 주입하는 것은 스프링 컨테이너이므로 설정 클래스를 이용해서 컨테이너를 생성해야한다.


- 컨테이너를 생성하면 getBean()메서드를 이용햇 사용할 객체를 구할 수 있다. 

```java
MemberRegisterService regSvc= ctx.getBean("memberRegSvc",MemberRegisterService.class);

```

- DI ->생성자 방식

```java
Public class MemberRegisterService{
  pivate MemberDao memberDao;

  public MemberRegisterService(MemberDao memberDao){
    this.memberDao = memberDao;
  }

  public Long regist(RegisterRequest req){
    Member member = memberDao.selectByEmail(req.getEmail());

    return member.getId();
  }
}


```


- DI주입방식 

  - 생성자 방식: 빈 객체를 생성하는 시점에 모든 의존 객체가 주입된다.
      - 빈 객체를 생성하는 시점에 필요한 모든 의존 객체를 주입받기 때문에 객체를 사용할 때 완전한 상태로 사용할 수 있다.

  - 설정 메서드 방식 : 세터 메서드이름을 통해 어떤 의존 객체가 주입되는지 알 수 있다.
    - NullPointerException이 발생할 수 있음 ( Setter메소드를 작성 안하고 생성하는 경우가 있기 떄문에)


- 싱글톤

  - 스프링 컨테이너가 생성한 빈은 싱글톤 객체이다.
  - 스프링은 설정 클래스를 그대로 사용하지 않는다. 대신 설정 클래스를 상속한 새로운 설정 클래스를 만들어서 사용한다. 

- 자동주입(@Autowired)
  - 스프링의 자동 주입 기능을 위한 것이다.
  - 스프링 설정 클래스의 필드에 @Autowired 붙이면 해당 타입의 빈을 찾아서 필드에 할당 한다.
  - @autowired 어노테이션을 이용해서 다른 설정 파일에 정의한 빈을 필드에 할당했다면 설정 메서드에서 이 필드를 사용해서 피룡한 빈을 주입하면 된다.

  ```Java
  public class ChangePasswordService{
    @Autowired
    private MemberDao memberDao;

  }

  // 설정클래스

  public class AppCtx{
    @Bean
    public MemberDao memberDao(){
      return new MemberDao();

    }

    @Bean
    public ChangePasswordervice changePwdSvc(){
      ChangePasswordService pwdSvc = new ChangePasswordService();
      return pwdSvc
    }

    //@Autowired어노테이션을 붙이면 설정 클래스에서 의존을 주입하지 않아도 된다.
    // 스프링이 해당 타입의 빈 객체를 찾아서 필드에 할당한다.
  }
  ```
  
- 주입대상
  - 스프링 컨테이너는 자동 주입, 라이프사이클 관리등 단순 객체 생성 외에 객체 관리를 위한 다양한 기능을 제공하는데 빈으로 등록한 객체에만 기능을 적용한다.

  - 객체를 스프링 빈으로 등록할 떄와 등록하지 않을 때의 차이는 스프링 컨테이너가 객체를 관리하는지 여부이다.