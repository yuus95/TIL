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
## IOC
- Inversion of Control : 제어의 역전
- 제어의 흐름을 사용자가 컨트롤 하지않고 위임한 특별한 객체에 모든것을 맡긴다.

---
## POJO

- EJB 등에서 사용되는 Java Bean이 아닌 Getter 와 Setter 로 구성된 가장 순수한 형태의 기본 클래스를 POJO라 한다.



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
  - 데이터 값을 복사해 놓은 임시 장소를 가리킨다. 보통 조회 속도 향상을 위해 캐시를 사용한다.

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

- 객체를 생성하고 의존 객체를 주입하는 것은 스프링 컨테이너의 역할이므로  설정 클래스를 이용해서 컨테이너를 생성해야한다.


- 컨테이너를 생성하면 getBean()메서드를 이용해서 사용할 객체를 구할 수 있다. 

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
  - @autowired 어노테이션을 이용해서 다른 설정 파일에 정의한 빈을 필드에 할당했다면 설정 메서드에서 이 필드를 사용해서 필요한 빈을 주입하면 된다.

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
  
  - 자동 주입을 하는 코드와 수동으로 주입하는 코드가 섞여 있으면 주입을 제대로 하지 않아서 NullPointerException이 발생했을 때 원인을 찾는 데 오랜 시간이 걸릴 수 있다.

  - 설정 클래스에서 세터 메서드를 통해 의존을 주입해도 해당 세터 메서드에 @Autowired 어노테이션이 붙어 있으면 자동 주입을 통해 일치하는 빈을 주입한다.
  @Autowired어노테이션을 사용했다면 설정 클래스에서 객체를 주입하기 보다는
  스프링이 제공하느 자동 주입 기능을 사용하는 편이 낫다.


---

## 컴포넌트 스캔

- 컴포터는 스캔은 스프링이 직접 클래스를 검색해서 빈으로 등록해주는 기능이다.

- 설정 클래스에 빈으로 등록하지 않아도 원하는 클래스를 빈으로 등록할 수 있으므로 컴포넌트 스캔 기능을 사용하면 설정 코드가 크게 줄어든다.

- @Component: 해당 클래스를 스캔 대상으로 표시한다.
- @ComponentScan : @Component 어노테이션을 붙인 클래스를 스캔해서 스프링 빈으로 등록하려면 설정 클래스에 @ComponentSacn 어노테이션을 적용해야 한다.

- 2개의 패키지에 같은 이름의 클래스가 존재하고 둘다 컴포넌트 어노테이션이 붙어있으면 둘 중 하나는 명시적으로 빈이름을 지정해서 '빈 이름 충돌'을 피해야 한다.

- componet vs configure
```
component는 개발자가 만든 클래스를 빈객체에 등록할 때 사용하구  configure  + bean은 외부 라이브러리 또는 내부 라이브러리를 빈객체로 등록할 때 사용한다.
```

--- 

## 빈 라이프사이클과 범위 

- 컨테이너 초기화와 종료
  - 스프링 컨테이너는 초기화와 종료라는 라이프 사이클을 갖는다.

  ```java
  
  //컨테이너 초기화 -> 
  AnnotationConfigApplicationContext ctx = new AnnotationConfigApplicationContext(AppContext.class); // 생성자를 통해 객체를 생성할 때 컨테이너 초기화

  // 컨테이너에서 빈 객체를 구해서 사용
  Greeter g = ctx.getBean("gretter",Greeter.calls); // getBean을 이용하여 빈 객체 사용

  // 컨테이너 종료
  ctx.close();
  ```

  - 초기화와 종료과정
      - 컨테이너 초기화 -> 빈 객체의 생성, 의존 주입, 초기화
      - 컨테이너 종료 -> 빈 객체의 소멸

- 스프링 빈 객체의 라이프사이클
  - 객체 생성 -> 의존 설정 -> 초기화 -> 소멸

- 빈 객체의 초기화와 소멸 : 스프링 인터페이스
  - ```java
    public interface InitializingBean{
      void afterPropertiesSet() throws Exception;
    }

    public interface DisposableBean{
      void destroy() throws Exception;;
    }

    ```

- 빈 객체의 초기화와 소멸: 커스텀 메서드
   - 필요할 떄 자세히 알아보기

   ```java
  @Bean(initMethod = "connect",destroyMethod = "close")
  public Client2 client2(){
    Client2 client = new Client2();
    client.setHost("host");
    return client;
  }
  // Client2 의 connect 메소드를 빈 객체를 초기화할 떄 실행한다.


   ```

- 프로토타입의 빈

  - 빈의 범위를 프로토타입으로 지정하면 빈 객체를 구할 때마다 새로운 객체를 생성한다 // 기본적인 빈은 싱글톤 객체

  ```java
  @Bean
  @scope("prototype")
  public Client client(){
    Client client = new Client();
    client.setHost("host");
    return client;
  }

  ```

  - 프로토타입 범위를 갖는 빈은 완전한 라이프 사이클을 따르지 않는다는 점에 주의해야 한다.

  - 프로토타입 범위의 빈을 사용할 떄에는 빈 객체의 소멸 처리를 코드에서 직접 해야 한다.

--- 

## AOP

- 프록시 객체 
   - 핵심기능의 실행은 다른 객체에 위임하고 부가적인 기능을 제공하는 객체를 프록시라고 한다. ex) 핵심기능 - 팩토리얼 계산 , 부가기능 - 함수 시작 시간,끝나는시간계산
   핵심기능이 있는 클래스를 인터페이스로 상속받아서 사용한다.

  </br>

   - 프록시 객체는 원래 객체를 감싸고 있는 객체이다. 
   원래 객체와 타입은 동일하다 프록시 객체가 원래 객체를 감싸서 client의 요청을 처리하게 하는 패턴 
   - 접근을 제어하고 시거나, 부가 기능을 추가하고 싶을 떄 사용한다.
   
   - 특징:
      - 핵심 기능은 구현하지 않는다.
      - 여러 객체에 공통으로 적용할 수 있는 기능을 구현
      - 핵심 기능에 공통 기능을 삽입하는 것

- AOP(Aspect Oriented Programming)
  - 여러 객체에 공통으로 적용할 수 있는 기능을 분리해서 재사용성을 높여주는 프로그래밍 기법

  - AOP는 핵심 기능과 공통 기능의 구현을 분리함으로써 핵심 기능을 구현한 코드의 수정 없이 공통 기능을 적용할 수 있게 만들어 준다.

  - 핵심 기능에 공통 기능을 삽입하는 방법
    - 1 컴파일 시점에 코드에 공통 기능을 삽입하는 방법
    - 2 클래스 로딩 시점에 바이트 코드에 공통 기능을 삽입하는 방법
    - 3 런타임에 프록시 객체를 생성해서 공통 기능을 삽입하는 방법
  
    </br>

    ```
     1~2번은 스프링 AOP에서는 지원하지 않으며 Aspect와 같이 AOP전용 도구를 사용해야 한다.
    ```

- 스프링 AOP
```
관점 지향 프로그래밍 : 
  어떤 로직을 기준으로 핵심적인 관점, 부가적인 관점으로 나누어서 보고 그 관점을 기준으로 각각 모듈화 

소스 코드상에서 다른 부분에 계속 반복해서 쓰는 코드를 흩어진 관심사 라고 한다.
  -> 흩어진 관심사를 Aspect로 모듈화 하고 핵심적인 비즈니슬죅에서 분리하여 재사용하는 것이 AOP의 취지이다.
```

  - 스프링 AOP는 프록시 객체를 자동으로 만들어 준다.(상위 타입의 인터페이스를 상속받은 프록시 클래스를 직접 구현할 필요가 없다) - > 공통 기능을 구현한 클래스만 알맞게 구현하면 된다

  - AOP 주요 용어

    - Advice : 언제 공통 관심 기능을 핵심 로직에 적용할 지를 정의하고 있다. ex) 메소드를 호출하기 전 (언제)에 트랜잭션 시작 기능을 적용한다는 것을 정의하고있다.

    - Joinpoint : Advice를 적용 가능한 지점을 의미 한다. (스프링은 프록시를 이용해서 AOP를 구현하기 때문에 메서드 호출에 대한 Joinpoint만 적용한다)

    - Pointcut : Jointpoint의 부분 집합으로서 실제 Adivce가 적용되는 Joinpoint를 나타낸다.

    - Weaving : Advice를 핵심 로직 코드에 적용하는 것을 weaving이라고 한다
    
    - Aspect:여러 객체에 공통으로 적용되는 기능을 Aspect라고 한다. ex) 트랜잭션이나 보안

  </br>
  
  - Advice 종류
    - Before Advice : 대상 객체의 메서드 호출 전에 공통 기능을 실행한다.
    - After Returning Advice : 대상 객체의 메서드가 익셉션 없이 실행된 이후에 공통 기능을 실행 한다.

    - After Throwing Advice : 대상 객체의 메서드를 실행하는 도중 익셉션이 발생한 경우에 공통 기능을 실행한다.

    - After Advice : 익셉션 발생 여부에 상관 없이 대상 객체의 메서드 실행 후 공통 기능을 실해한다.

    - Around Advice : 대상 객체의 메서드 실행 전, 후 또는 익셉션 발생 시점에 공통 기능을 실행하는데 사용한다.


  - 구현
    - Aspect로 사용할 클래스에 @Aspect 어노테이션을 붙인다.
    - @Pointcut 어노테이션으로 공통 기능을 적용할 Pointcut을 정의한다.
    - 공통 기능을 구현한 메서드에 @Around 어노테이션을 적용한다

  - ```java

      // 사용방법
      
        @Aspect
        @Component
        public class TestYushin {


        @Pointcut("execution(* com.example.jpa_shop..*())")/
        private void publicTarget(){

        }

        @Around("publicTarget()") // publicTarget메소드에 정의한 PointCut에 공통 기능을 적용한다
        public Object measure(ProceedingJoinPoint joinPoint) throws Throwable{
                long start = System.nanoTime();
                try{
                        Object result = joinPoint.proceed(); // 대상 객체의 메소드가 실행된다 (프록시객체 말고 주요기능들이 실행)
                        return result;

                } finally {
                        long finish = System.nanoTime();
                        Signature sig = joinPoint.getSignature();
                        System.out.printf("%s.%s(%s) 실행시간 : %d ns\n",joinPoint.getTarget().getClass().getSimpleName()
                                ,sig.getName()
                                , Arrays.toString(joinPoint.getArgs())
                                ,(finish-start));
                  // 주요 기능을 실행한 다음 실행되는 부분
                }
        }

      }
    ```

  - 메소드 정의
    - 메서드 시그니처 : 메서드 이름 + 파라미터 
    - @EnableAspectJAutoProxy : 설정 클래스에 붙여 놓으면 @Aspect어노테이션을 읽어올 수 있다. -- > Aspect붙어있는 클래스에 @Component 어노테이션을 붙여도 된다. 
    - @Pointcut : 공통 기능을 적용할 대상을 설정
    - @Around : Around Advice를 설정한다.
    - ProceedingJoinPoint : 프록시 대상 객체의 메서드를 호출할 떄 사용한다. 
      ex) proceed()메소드를 실행하면 주요기능 실행 
    - execution 명시자 표현식 : Advice를 적용할 메서드를 지정할 떄 사용
    execution(수식어 패턴? 리턴타입패턴 클래스이름 패턴?메서드이름패턴(파라미터패턴))
      ```
        - 클래스..(.. : 하위 클래스을 의미)
        - * 모든값
        - 매개변수 .. : 0개이상
      ```
    - Advice 적용순서
      - @Aspoect 어노테이션 밑에 @Order옵션추가하기

    - @Around의 Pointcut 설정과 @Pointcut재사용
      - @Around 에 Execution명시자를 직접 지정
        ```Java

        @Around("execution(public * chap07..*(..))")

        ```
      - 공통 @Pointcut재사용
        - ```java
            // test_yushin 패키지에 있는 AOP
          @Aspect
          public Class Example1{
            @Pointcut("execution(public * yushin..*(..))")
            public void exampleStart(){

            }
          }

          //다른 패키지에 있는 AOP
          @Aspect
          public class Example2
          
            @Around("test_yushin.example1.exampleStart()"))
            public object execute(ProceedingJoinPoint joinPoint) throw Throwable{

            }
          ```
          - 해당 Pointcut의 완전한 클래스 이름을 포함한 메서드 이름을 @Around어노테이션에서 사용하면 된다.



  - 프록시 객체
    - AOP을 적용하여 실행 된 클래스는 프록시로 변한다.
      ex) MemberDService 클레스에서 실행된 함수는  AOP을 적용 한 순간
      프록시로 변하게 된다.
    - 생성 방식
      - 스프링은 AOP를 위한 프록시 객체를 생성할 때 실제 생성할 빈 객체가 인터페이스를 상속하면 인터페이스를 이용해서 프록시를 생성한다. 
      -- > 인터페이스말고 클래스를 상속하고 싶을 땐 
      설정클래스의 @EnableAspectJAutoProxy(proxyTargetClass = True) 옵션을 추가해준다.

---
## DB

- 커넥션 풀
  - 일정 개수의 DB커넥션을 미리 만둘어두는 기법
  - 최초 연결에 따른 응답 속도 저하와 동시 접속자가 많을 떄 발생하는 부하를 줄이기 위해 사용하는 것
  
  - 커넥션 풀 기능을 제공하는 모듈 -> HikariCP,Tomcat JDBC 등등

---

## Transactional

- @Transactional 어노테이션을 사용하면 트랜잭션 범위를 매우 쉽게 지정할 수 있다.


어노테이션을 사용하지 않은 경우 
```java
public void  yushin_jpa{
  public static void main(String[] args){

      //엔티티 매니저  팩토리는 하나만 생성해서 애플리케이션 전체에서 공유 .
        EntityManagerFactory emf = Persistence.createEntityManagerFactory("hello");

        //엔티티 매니저는 쓰레드간에 공유X (사용하고 버려야 한다)
        EntityManager em = emf.createEntityManager();


      // 트랜잭션 처리를 직접 해줘야한다.
        EntityTransaction tx = em.getTransaction();
        tx.begin();
      
      try{
          Team teamA = new Team();
          teamA.setName("A팀");
          em.persist(teamA);
          em.commit;
      }
      catch{
        tx.rollback();
      }
      finally{
        em.close()
      }
      emf.close()

  }
}

```


- 트랜젹선 어노테이션 사용 할 경우 


```Java

// saveItem 안에서 동일한 트랜잭션 범위를 쉽게 할 수 있다. 
public class ItemService{
  private final ItemRepository itemRepository;

  @Transactional
  public void saveItem(Item item){
    itemRepository.save(item);
  }
}

```

- 프록시
  - 스프링은 @Transactional 어노테이션을 이용해서 트랜잭션을 처리하기 위해 내부적으로 AOP를 사용한다.

- SQLException
  - 별도 설정을 추가하지 않으면 발생한 익셉션이 RuntimeException일 떄 트랜잭션을 롤백 한다. 
  Exception클래스를 구현할 때 RuntimeException을 상속하는 이유다
  
    하지만 SQLException은 상속하고 있지 않으므로 롤백을 하지 않는다. 롤백을 하고싶다면 

     ```java
    @Transational(rollbackFor=SQLException.class)
    을 설정해주면 된다/
     ```

- 트랜잭션 전파

  - @Transaction 의 propagation속성은 기본값이 REQUIRED이다.
  현재 진행 중인 트랜잭션이 존재하면 해당 트랜잭션을 사용하고 존재하지 않으면 새로운 트랜잭션을 생성한다.

  ```java
  public class ChangePasswordService{
    @Transactional
    public void changePassword(String,email,String oldPwd,String newPwd){
      Member member = memberDao.selectByEmail(email);
      if(member == null){
        throw new MemberNotFoundException();

      }

      memberchangePassword(oldPwd,newPwd);
      memberDao.update(member);

      //memberDao.update의 쿼리문도 하나의 트랜잭션 안에서 실행된다.
    }
  }

  // Transaction이 없는 클래스
  public class MemberDao{
    private JdbcTemplate jdbcTemplate;

    public void update(Member member){
      jdbcTemplate.update(
        "update MEMBER set NAME = ?, PASSWORD =? where EMAIL =?", member.geName(),member.getPassword(),member.getEmail());
    }
  }

  
  ```


---
## SPRING MVC

```java

@Configuration
@EnableWenMvc
public class MvcConfig imlements WebMvcConfigure{
  @Override
  public void configureDefaultServletHandling{
    confirer.enable();
  }

  @Override
  public void confureViewResolvers(ViewResolverRegistry registry){
    registry.jsp("/WEB-INF/view/",".jsp")
  
  }

}


```


- @EnableWebMvc : 스프링 MVC설정을 활성화 한다.
  - 스프링 MVC를 사용하려면 다양한 구성 요소를 설정해야 한다.
  이 요소를 처음부터 끝까찌 직접 구성하면 매우 복잡해진다. 
  이러한 복잡한 설정을 대신 해주는 어노테이션이다.


- WebMvcConfigurer 
  - @EnableWebMvc 어노테이션이 스프링 MVC를 사용하는데 필요한 기본적인 구성을 설정해준다면,
  WebMvcConfigurer 인터페이스는 스프링 MVC의 개별 설정을 조정할 때 사용한다.


- @Controller
  - 웹 요청을 처리하고 그 결과를 뷰에 전달하는 스프링 빈 객체이다.

  - @Controller 어노테이션과 @GetMapping 또는 @PostMapping 와 같은 요청 매핑 어노테이션을 이용해서 처리할 경로를 지정해 주어야 한다.

```java
@Controller
public class HelloController{
  
  @GetMapping("/hello")
  public String hello(Model model,
  @RepestParam(value = "name",required = false) String name){
    model.addAttribute("greeting","안녕하세요"+name);
    return "hello";
  } )
}

```
  - model : 컨트롤러 처리 결과를 뷰에 전달할 떄 사용한다.
  - @RequestParam어놑에ㅣ션은 HTTP 요청 파라미터의 값을 메서드의 파라미터로 전달할 떄 사용된다. 

  - 컨트롤러의 처리 결과를 보여줄 뷰 이름"hello"을 리턴한다.  


--- 

- Controller 와 Service를 나누는 이유
  - 중복되는 코드가 생기기떄문
  - 비즈니스 로직은 다른 요청 URL에서 사용해야 하는 경우가 있다. 비즈니스로직 코드가 컨트롤러에서 구현되어 있을 경우 재사용성이 줄어든다.

  - 모든 기능들을 세분화해서 @Service에 작성하게 되면 나중에는 서비스의 기능들을 조합만 해서 새로운 기능을 만들 수 있음

  - 확장성과 재사용성 그리고 중복 코드의 제거를 하기위해서 분리한다

  - 출처 : https://devlog-wjdrbs96.tistory.com/209
