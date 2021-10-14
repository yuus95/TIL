# QueryDsl

## QueryDsl이란?
- Querydsl 은 정적 타입을 이용해서 SQL과 같은 쿼리를 생성할 수 있도록 해주는 프레임 워크다.


## QueryDsl을 사용하는 이유
- 타입에 안전한 방식으로 HQL쿼리를 실행하기 위한 목적으로 만들어졌다.

- JPQL은 쿼리를 작성하다보면 String 연결을 이용하게 되고, 이는 결과적으로 읽기 어려운 코드를 만드는 문제를 야기 한다.

- 타입에 안전하도록 도메인 모델을 변경하면 소프트웨어 개발에서 큰 이득을 얻게 된다.
도메인의 변경이 직접적으로 쿼리에 반영되고, 쿼리 작성 과정에서 코드 자동완성 기능을 사용함으로써 쿼리를 더 빠르고 안전하게 만들 수 있게 된다.


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


- ## Select(JPQL)
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

- ## QueryDsl - Select
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

</br>

- ## Search
    ```java

    @Test
    public void search(){
        Member findMember = qf
                .selectFrom(member)
                .where(member.username.eq("member1")
                        .and(member.age.eq(10)))
                .fetchOne();

        Assertions.assertThat(findMember.getUsername()).isEqualTo("member1");

    }


    @Test //Search 매소드랑 같은역할
    public void searchAndParam(){
        Member findMember = qf
                .selectFrom(member)
                .where(
                        member.username.eq("member1"),
                        member.age.eq(10) // and 일 경우 ,만 써도된다.
                )
                .fetchOne();

        Assertions.assertThat(findMember.getUsername()).isEqualTo("member1");

    }


    ```

</br>

- ##  fetchResults

    ```java
        @Test
        public void resultFetch(){
            List<Member> fetch = qf
                    .selectFrom(member)
                    .fetch();


            Member fetchOne = qf
                    .selectFrom(member) // .where(member.age.eq(10))
                    .limit(1)
                    .fetchOne();



            Member fetchFirst = qf
                    .selectFrom(member)
                    .fetchFirst(); // .limit(1).fetchOne()
            
            
            //페이징 기능을 한다 - > 쿼리문 2개 실행 count함수, 조회함수
            QueryResults<Member> results = qf
                    .selectFrom(member)
                    .fetchResults();// 페이징 정보 보함 total count 쿼리 추가실행

            long total = results.getTotal();
            List<Member> content = results.getResults();

            System.out.println("content ==> " + content + "total == " + total);   //results.get
        }

    ```

- ## Sort

```java
/**
     * 1. 회원 나이 내림차순(desc)
     * 2. 회원 이름 올림차순(asc)
     * 단 2에서 회원 이름이 없으면 마지막에 출력(nulls last)
     */
    @Test
    public void sort(){
        em.persist(new Member(null,100));
        em.persist(new Member("member5",100));
        em.persist(new Member("member6",100));

        List<Member> result = qf
                .selectFrom(member)
                .where(member.age.eq(100))
                .orderBy(member.age.desc(), member.username.asc().nullsLast())
                .fetch();

        Member member5 = result.get(0);
        Member member6 = result.get(1);
        Member membernull = result.get(2);

        Assertions.assertThat(member5.getUsername()).isEqualTo("member5");
        Assertions.assertThat(member6.getUsername()).isEqualTo("member6");
        Assertions.assertThat(membernull.getUsername()).isNull();



    }


```


- ## 페이징
    - ### Count 쿼리에 조인이 필요없는 성능 최적화가 필요하다면, count전용 쿼리를 별도로 작성해야 한다.

    - 1번
    ```java
        // limit 와 offset을 이용
        @Test
        public void paging1(){
            List<Member> result = qf
                    .selectFrom(member)
                    .orderBy(member.username.desc())
                    .offset(1)
                    .limit(2)
                    .fetch();

            Assertions.assertThat(result.size()).isEqualTo(2);
        }

    ```

    - 2번
    ```java
    //fetchResults을 이용하여 총 갯수도 같이 조회할 수 있다.  
    // fetchResult를 사용하면 자동적으로 count쿼리를 발생 시키는데 이 쿼리는 원본 쿼리와 같이 모두 조인을 해버리기 떄문에 성능이 안나올 수 있다. 
        @Test // 총갯수도 같이조회
        public void paging2(){
            
            // fetchresults는 웬만하면 사용xx 성능안나옴
            QueryResults<Member> results = qf
                    .selectFrom(member)
                    .orderBy(member.username.desc())
                    .offset(1)
                    .limit(2)
                    .fetchResults();

            Assertions.assertThat(results.getTotal()).isEqualTo(4);
            Assertions.assertThat(results.getOffset()).isEqualTo(1);
            Assertions.assertThat(results.getLimit()).isEqualTo(2);
            Assertions.assertThat(results.getResults()).isEqualTo(2);

        }

    ```

- ## 집합 함수

    ```java 
    @Test
        public void aggregation(){
            List<Tuple> result = qf
                    .select(
                            member.count(),
                            member.age.sum(),
                            member.age.avg(),
                            member.age.max(),
                            member.age.min())
                    .from(member)
                    .fetch();

            Tuple tuple = result.get(0);

            Assertions.assertThat(tuple.get(member.count())).isEqualTo(5);
            Assertions.assertThat(tuple.get(member.age.sum())).isEqualTo(150);
            Assertions.assertThat(tuple.get(member.age.avg())).isEqualTo(30);
            Assertions.assertThat(tuple.get(member.age.max())).isEqualTo(50);
            Assertions.assertThat(tuple.get(member.age.min())).isEqualTo(10);


        }


    ```

- ## GroupBy 사용

    ```java


        /**
        * 팀의 이름과 각 팀의 평균 연령을 구해라.
        */
        @Test
        public void group() throws Exception{
            List<Tuple> result = qf
                    .select(team.name, member.age.avg())
                    .from(member)
                    .join(member.team, team)
                    .groupBy(team.name)
                    .fetch();
            Tuple teamA = result.get(0);
            Tuple teamB = result.get(1);

            Assertions.assertThat(teamA.get(team.name)).isEqualTo("teamA");
            Assertions.assertThat(teamA.get(member.age.avg())).isEqualTo(15);


            Assertions.assertThat(teamB.get(team.name)).isEqualTo("teamB");
            Assertions.assertThat(teamB.get(member.age.avg())).isEqualTo(40);
        }


    ```

- ## join

    - ### Left Outer Join
    ```java

        /**
        * Left Outer Join  - > teamA에 속하지 않아도 멤버객체는 다 나온다
        */
        // join(조인 대상, 별칭으로 사용할 Q타입)
        @Test
        public void join(){

            List<Member> result = qf
                    .select(member)
                    .from(member)
                    .leftJoin(member.team, team) //QMember, QTeam
                    .where(team.name.eq("teamA"))
                    .fetch();

            Assertions.assertThat(result)
                    .extracting("username")// 필드 추출 member의name 필드 추출
                    .containsExactly("member1","member2","member3");

        }

    ```

    - ### 세타 조인
    ```java
    /**
        * 세타 조인
        * 회원의 이름이 팀 이름과 같은 회원 조회
        */
        @Test
        public void theta_join(){

            em.persist(new Member("teamA"));
            em.persist(new Member("teamB"));

            List<Member> result = qf
                    .select(member)
                    .from(member, team)
                    .where(member.username.eq(team.name))
                    .fetch();

            Assertions.assertThat(result)
                    .extracting("username")
                    .containsExactly("teamA","teamB");


        }

    ```
    - ###  조인 대상 필터링

    ```java
    /**
        * 조인 대상 필터링
        *
        * 예) 회원과 팀을 조인하면서, 팀 이름이 teamA인 팀만 조인, 회원은 모두 조인
        *  JPQL : select m, t from Member m left join m.team t on t.name = 'teamA'
        */
        @Test
        public void join_on_filtering(){

            List<Tuple> result = qf
                    .select(member, team) // 여러가지 타입이라 튜플로 조회됨
                    .from(member)
                    .leftJoin(member.team, team).on(team.name.eq("teamA"))
                    .fetch();
            System.out.println("==== "+ result.get(0).get(team.name));

            // iter + tap
            for (Tuple tuple : result) {
                System.out.println("tuple"+tuple);
            }
        }


    ```
    - ##   연관관계 없는 외부 조인
    ```java
    /**
        *  연관관계 없는 엔티티 외부 조인
        *
        * 회원의 이름이 팀 이름과 같은 대상 외부 조인
        */
        @Test
        public void join_on_no_relation(){

            // join할 때 member.team 으로 조인하면 쿼리문이 날라갈 때 id값으로 조인이 걸린다.

            em.persist(new Member("teamA"));
            em.persist(new Member("teamB"));
            List<Tuple> result = qf
                    .select(member, team)
                    .from(member)
                    .leftJoin(team).on(member.username.eq(team.name)) // Team 객체와 외부조인 member.team 과 조인하는게 아님
                    .fetch();

            for (Tuple tuple : result) {
                System.out.println("tuple == "+ tuple);
            }

    //        System.out.println("tuple[6] ==> 1" + result.get(6).get(team.name));
        }

    ```
    - ### 패치조인  - 연관관계 초기화 관련 코드
    ```java
    @Test
        public void fetchJoinNo(){
            em.flush();
            em.clear();

            Member findMember = qf
                    .selectFrom(QMember.member)
                    .where(QMember.member.username.eq("member1"))
                    .fetchOne();

            boolean loaded = emf.getPersistenceUnitUtil().isLoaded(findMember.getTeam());
            Assertions.assertThat(loaded).as("페치 조인 미적용").isFalse(); // 결과값이 False 오류가 나지 않는다. 프록시 객체초기화를 안했기때문.


        }


        @Test //페치조인사용
        public void fetchJoinUse(){
            em.flush();
            em.clear();


            //연관된 객체를 한번에 끌고온다.
            Member findMember = qf
                    .selectFrom(QMember.member)
                    .join(member.team,team).fetchJoin()
                    .where(QMember.member.username.eq("member1"))
                    .fetchOne();

            boolean loaded = emf.getPersistenceUnitUtil().isLoaded(findMember.getTeam());
            Assertions.assertThat(loaded).as("페치 조인 적용").isFalse();  // 실패 결과값이 True이기 떄문에 오류가 나타난다. 


        }

    ```
- ## Sub_Query
    - 데이터를 최소화해서 보내는기 위해 From절의 서브쿼리는 최대한 지양하기
    - DB는 데이터만 필터링하고 그룹핑해서 가져오고  애플리케이션 계층에서 데이터를 변경한다.

    - ### Where 


    ```java

        /**
        * 서브쿼리 사용 - 나이가 가장많은 사람 조회
        */
        @Test
        public void use_subQuery(){
            QMember sub_m = new QMember("sub_m");

            List<Member> member = qf
                    .selectFrom(QMember.member)
                    .where(QMember.member.age.eq(
                            JPAExpressions
                                    .select(sub_m.age.max())
                                    .from(sub_m)
                    ))
                    .fetch();

            Assertions.assertThat(member.get(0).getAge()).isEqualTo(50);


        }



        /**
        * 서브쿼리 사용 - 나이가 10살 이상인사람
        */
        @Test
        public void use_subQuery_goe(){
            QMember sub_m = new QMember("sub_m");

            List<Member> member = qf
                    .selectFrom(QMember.member)
                    .where(QMember.member.age.in(
                            JPAExpressions
                                    .select(sub_m.age)
                                    .from(sub_m)
                                    .where(sub_m.age.gt(10))
                    ))
                    .fetch();
            Assertions.assertThat(member.size()).isEqualTo(3);

        }


    ```

    - ### Select 서브쿼리

    ```java
        /**
        * select -subQuery
        */
        @Test
        public void Select_Suq_Query(){
            QMember sub_m = new QMember("sub_m");


            List<Tuple> Tuple = qf
                    .select(
                            member.username,
                            JPAExpressions
                                    .select(sub_m.age.avg())
                                    .from(sub_m))
                    .from(member)
                    .fetch();

            for (com.querydsl.core.Tuple tuple : Tuple) {
                System.out.println("Tuple ==> "+tuple);
            }
        }
    ```

- ## case문
    - ### 기본

    ```java
        /**
        * case문 기본 문법
        */
        @Test
        public void basicCase(){
            List<String> members = qf
                    .select(member.age
                            .when(10).then("열살")
                            .when(20).then("스무살")
                            .otherwise("기타"))
                    .from(member)
                    .fetch();
            for (String s : members) {
                System.out.println("member ==>  "+s);
            }
        }

    ```

    - ### 복잡한 case문 작성

    ```java
        /**
        * 복잡한 case문  - caseBuilder() 사용하기
        */
        @Test
        public void caseBuildUse(){

            List<String> caseMember = qf
                    .select(
                            new CaseBuilder()
                                    .when(member.age.between(0, 20)).then("0~20살")
                                    .when(member.age.between(21, 30)).then("21~30")
                                    .otherwise("기타"))
                    .from(member)
                    .fetch();

            for (String s : caseMember) {
                System.out.println("caseMember ==> " + s);
            }
        }

    ```

    - ###   OrderBy에서 Case문 함계 사용하기

    ```java
    /**
        * OrderBy에서 Case문 함계 사용하기
        */
        @Test
        public void OrderByCase(){
            NumberExpression<Integer> rankPath = new CaseBuilder()
                    .when(member.age.between(0, 20)).then(2)
                    .when(member.age.between(21, 30)).then(1)
                    .otherwise(3);
            List<Tuple> result = qf
                    .select(member.username,
                            member.age,
                            rankPath)
                    .from(member)
                    .orderBy(rankPath.desc())
                    .fetch();

            for (Tuple tuple : result) {
                String username = tuple.get(member.username);
                Integer age = tuple.get(member.age);
                Integer rank = tuple.get(rankPath);
                System.out.println("username = "  + username + " age = " + age + " rank = " + rank);
            }
        }

    ```

- ## 상수 사용하기

```java
/**
     * 상수 표시
     */
    @Test
    public void constant(){

        List<Tuple> members = qf
                .select(QMember.member.username, Expressions.constant("A"))
                .from(member)
                .fetch();

        for (Tuple tuple : members) {
            System.out.println("tuple ==> " + tuple);
        }
    }
```

- ##  문자 더하기

```java
  /**
     * 문자더하기 {member.username}_{member.age}
     */
    @Test
    public void str_plus(){

        List<String> members = qf
                .select(
                        member.username.concat("_").concat(member.age.stringValue()))
                .from(member)
                .fetch();

        for (String s : members) {
            System.out.println("s ==> " + s );
        }

    }

```

---

- ## JPAQueryFactory 
    - QueryDSL 사용할 떄 엔티티 매니저 같은 느낌을 준다.
    - 필드로 제공해도 동시성 문제를 걱정안해도된다.
        스프링 프레임워크는 여러 쓰레드에서 동시에 같은 EntityManager에 접근해도, 트랜잭션 마다 별도의 영속성 컨텍스트를 제공하기 떄문에 동시성 문제는 걱정 안해도 된다. 

