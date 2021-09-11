# TIL


## git 명령어

git satus - > 상태확인 <br/>

``` 
git commit 하는 방법
git add . 
git commit -m "메세지"
git push origin main( 원격 저장소명 브랜치명)

```

git pull - 원격 저장소에서 로컬 저장소로 소스 가져오는 명령어

```
git pull origin main 
```

처음 프로젝트 가져왔을 때
```
git init 
git remote add origin 주소
git pull origin main

```


---
## 스프링 어노테이션

내가 생각하는 자주 사용하는 어노테이션
나중에 정리하기

@GetMapping("/주소")


@PostMapping("/주소")

@PathVariable("id") Long id

@RequestBody

@Vaild

@Data

@AllArgsConstructor

@NotEmpty validation을 DTO할 때 유용함




--- 
## 아키텍처

- Spring 웹 계층
    - Presentation Layer , Business Layer(Service Layer) , Data Access Layer(Repository Layer)

    - 프레젠테이션 계층
        - 브라우정상의 웹 클라이언트의 요청 및 응답을 처리
        - @Controller 또는 @RestController 어노테이션을 사용
        - 서비스 계층, 데이터 엑세스 계층에서 발생하는 Exception을 처리
    
    - Service 계층
        - 요구사항에 맞게 비즈니스 로직을 작성하는계층
        - Service어노테이션 사용
        - 트랜잭션에 관리(트랜잭선전파효과)
        - 프레젠테이션 계층과 데이터 엑세스 계층 사이를 연결하는 역할(직접적인 통신을 못하게함)

    
    - Data Access Layer(데이터 접근 계층)
        - 데이터를 저장하거나 조회하기 위해 DB에 접근하는 계층 
        - ORM(Mybatis,Hibernate)를 주로 사용하는 계층
        - DataBase에 data를 CRUD하는 계층 

    - 도메인 모델 계층
        - DB의 테이블과 매칭될 클래스
        - Entity 클래스라고도 부른다.

---
## Data 관련 객체

- DAO(Data Access Object)
    - DB에 접근하는 객체, DB를 사용해 데이터를 조작하는 기능을 하는 객체(MyBatis 사용시에 DAO 또는 Mapper사용)
    - Repository라고도 부른다(JPA사용시 Repository 사용)
    - Service 계층과 DB를 연결하는 고리 역할

- DTO(Data Tranfer Object)
    - 각 계층간 데이터 교환을 위한 객체(데이터를 주고 받을 포맷)
    - Domain,VO라고도 부름
    - DB에서 데이터를 얻어 Service,Controller등으로 보낼 때 사용
    - 로직을 갖기 않고 순수하게 Getter,Setter메소드를 가진다

- Entity클래스
    - Domain이라고도 부름(JPA사용할 때 사용)
    - 실제 DB테이블과 매칭될 클래스
    - Entity클래스 또는 가장 Core한 클래스라고 부름
    - Domain 로직만을 가지고 있어야 하며 Presentation Logic을 가지고 있어서는 안된다.

- Domain 클래스와 DTO클래스를 분리하는 이유
    - View Layer와 DB Layer의 역할을 철저하게 분리하기 위해서
    - 테이블과 매핑되는 Entity 클래스가 변경되면 여러 클래스에 영향을 끼치게 되지만 View와 통신하는 DTO클래스는 자주 변경되므로 분리해야 한다.
    - 즉 DTO는 Domain Model을 복사한 형태로, 다양한 Presentation Logic을 추가한 정도로 사용


- 출처 :https://devlog-wjdrbs96.tistory.com/209


---

## IntelliJ 팁

- Intellij Gradle 대신 자바로 바로 실행하기

    - Preferences -> Build , Execution, Deployment -> Build Tools -> Gradle

    - Build and run using:Gradle -> IntellijDea
    - Run tests using : Gradle -> Intellij IDEA


- 롬북 적용

    - Preferences -> plugin -> lombok 검색 실행(재시작)
    - Preferences -> Annotation processors 검색 -> Enable annotation processing 체크(재시작)

    - 임의의 테스트 클래스를 만들고 게터 세터 확인하기



## Ec2

- EBS 용량 꽉찼을 떄

```
df -h //용량 확인하기  /dev/xvda1 용량이 풀이였음

aws 들어가서 용량 늘린 다음

차례대로 실행하기
sudo growpart /dev/xvda 1 //디스크 용량을 1번째 파티션으로 할당

sudo resize2fs xvda1 // xvda1 (1번째 파티션 사이즈 조정)
```


- QueryDsl이랑 SpringData JPA사용할 때 조심해야하는점
    ```
    아마존 프리티어 에서는 QueryDsl과 SpringDataJPA을 사용한 프로젝트를 
    Test하면 Ec2가 멈춘다.
    테스트하지말기.. 
    
    - ec2 하나 날리면서 배웠다.
    ```


## 데드락

-  둘 이상의 스레드가 lock을 흭득하기 위해 기다리는데 lock을 가지고 있는 스레드도 똑같이 다른 lock을 기다리면서 서로 블락 상태시 놓이는것을 말한다

- 해결 : 우선 순위를 선전해 자원을 선정, 공유 불가능한 상호 배제조건을 제기

## Http메소드

- post : 특정 리소스에 엔티티 제출
- get : 참조요청
- put : 리소스에 대한 전체수정
- patch : 리소스의 부분수정

## OSI 7계층
(요약)
- 물리계층 : 데이터 전송

- 데이터 링크계층(맥주소 활용) : 프레임에 주소부여

- 네트워크 계층: 주소부여 경로 설정

- 전송 계층 : 패킷 생성 및 전송

- 세션 계층: 통신하는 사용자 동기화

- 표현 계층 : 암호화

- 응용계층: 일반적인 응용서비스를 수행
