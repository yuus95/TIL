# QueryDsl


## 환경 설정
- QueryDsl은 환결설정 자체가 어려운거 같다
새로운 프로젝트할 떄마다 챙겨보기

```java

// Build.gradle
// 적용해도 안 될경우 .idea 폴더, .gradle 폴더 삭제하기 다시 실행하기

plugins {
	id 'org.springframework.boot' version '2.5.2'
	id 'io.spring.dependency-management' version '1.0.11.RELEASE'
	id 'java'

	//querydsl 추가
	id "com.ewerk.gradle.plugins.querydsl" version "1.0.10"
}


group = '?'
version = '?'
sourceCompatibility = '?'

configurations {
	compileOnly {
		extendsFrom annotationProcessor
	}
}

repositories {
	mavenCentral()
}

dependencies {
	implementation 'org.springframework.boot:spring-boot-starter-data-jpa'
	implementation 'org.springframework.boot:spring-boot-starter-web'


	//querydsl 추가
	implementation 'com.querydsl:querydsl-jpa'

	implementation 'com.github.gavlyukovskiy:p6spy-spring-boot-starter:1.5.8'

	compileOnly 'org.projectlombok:lombok'
	runtimeOnly 'com.h2database:h2'
	annotationProcessor 'org.projectlombok:lombok'
	testImplementation 'org.springframework.boot:spring-boot-starter-test'


}


test {
	useJUnitPlatform()
}

//querydsl 추가 시작
def querydslDir = "$buildDir/generated/querydsl"
querydsl {
	jpa = true
	querydslSourcesDir = querydslDir
}
sourceSets {
	main.java.srcDir querydslDir
}
configurations {
	querydsl.extendsFrom compileClasspath
}
compileQuerydsl {
	options.annotationProcessorPath = configurations.querydsl
}
//querydsl 추가 끝

```

- 검증용 Q타입 생성(임의의 Entity 클래스 생성해놔야함)
    - Gradle Intellij 사용법
        - gradle - > Tasks - > build -> clean
        - Gradle -> Tasks -> other -> compileQuerydsl(javacomplie을 사용해도된다)

    -   Gradle 콘솔 사용법
        - ./gradlew clean compileQuerydsl

- Q 타입은 컴파일 시점에 자동 생성되므로 버전관리에 포함하지 않는 것이 좋다.



---

## 기본 문법
- @SpringBootTest: 통합 테스트를 제공하는 기본적인 스프링 부트 테스트 어노테이션
- @SpringBootTest-> 스프링부트 어플리케이션 테스트에 필요한 의존성을 대부분 제공 한다.


- Assertions.assertThat(테스트객체).isEqaulTo("원하는결과")

    - ex) Assertions.assertTaht(findMember.getUsername()).isEqaulTo("member1");



- 초기 설정 (테스트 코드로 작성예시)
    ```java
    
    @SpringBootTest
    @Transactional
    public class QuerydslTest{

        @Autowirde // 빈으로 등록된 객체를 자동 주입
        EntityManager em;
        
        // QueryDsl을 사용할려면 무조건 필요하다
        JPAQueryFactory qf; 

          @BeforeEach
    public void before(){
        qf = new JPAQueryFactory(em); // 엔티티 매니저를 통해 생성

        Team teamA= new Team("teamA");
        Team teamB= new Team("teamB");
        em.persist(teamA);
        em.persist(teamB);

        Member member1 = new Member("member1",10,teamA);
        Member member2 = new Member("member2",20,teamA);
        Member member3 = new Member("member3",30,teamB);
        Member member4 = new Member("member4",40,teamB);
        Member member5 = new Member("member5",50,teamB);
        em.persist(member1);
        em.persist(member2);
        em.persist(member3);
        em.persist(member4);
        em.persist(member5);
    }


    }



    
    ```


- Select(JPQL)
    ```java
    @Test
    public void select_jpql(){
        Member findByJPQL = em.createQuery(
            "select m from Member m "
            "where m. username =:username"
            ,Member.class
        )
        .setParameter("username","member1")
        .getResultList();
    }

    Assertions.assertThat(findByJPQL.getUsername()).isEqualTo("member1");

    ```    

- QueryDsl - Select
    - QueryDsl은 JPQL빌더
    - JPQ: 문자 실행(실행 시점 오류), QueryDsl : 코드(컴파일 시점 오류)
    - JPQL:파라미터 바인딩 직업, QueryDsl : 파라미터 바인딩 자동 처리
    ```java
    @Test
    public void QueryDsl_select(){
        
        // JPA 의 엔티티 매니저 느낌 
        //JPAQueryFactory qf
        Member findMember =     qf
                                    .select(Qmember.member)
                                    .from(Qmember.member)
                                    .where(Qmember.member.username.eq("member1")) // 바인딩 자동 처리 
                                    .fetchOne();

        //Assertions으로 테스트하기
        // Qmember.member - > Qmember  static으로 import하기

    }


    ```


---

- JPAQueryFactory 
    - QueryDSL 사용할 떄 엔티티 매니저 같은 느낌을 준다.
    - 필드로 제공해도 동시성 문제를 걱정안해도된다.
        스프링 프레임워크는 여러 쓰레드에서 동시에 같은 EntityManager에 접근해도, 트랜잭션 마다 별도의 영속성 컨텍스트를 제공하기 떄문에 동시성 문제는 걱정 안해도 된다.
        
