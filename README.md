

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


## 프로세스 
- 운영체제로부터 시스템 자원을 할당받는 작업의 단위 코드,데이터,스텍,힙 부분으로 나뉘어짐


## 스레드   
- 한 프로세스내에서 동작되는 여러 실행의 흐름 프로세스 하나에 자원을 공유


- 비트나미(APM환경구축)
    - 비트나미는 가상 어플라이언스 및 웹 애플리케이션, 개발 스택용 소프트웨어 패키지 및 설치 라이브러리이다


## 변수의 명명 규칙
​
-   대소문자가 구분되며 길이에 제한이 없다
    -   True와 true는 서로 다른 것으로 간주한다.
-   예약어를 사용해서는 안된다.
    -   true는 예약어라서 사용할 수 없지만, True는 가능하다
-   숫자로 시작해서는 안 된다.
    -   top10은 허용하지만 7up은 허용되지 않는다.
-   특수 문자는 '\_'와 '$'만 허용된다
    -   $harp는 허용되지만, s#arp는 허용되지않는다.
​
## 권장 규칙
​
-   클래스 이름의 첫 글자는 항상 대문자로 한다.
    
    -   변수와 메서드의 이름의 첫 글자는 항상 소문자로 한다.
-   여러 단어로 이루어진 이름은 단어의 첫 글자를 대문자로 한다.
    
    -   lastIndexOf, StringBuffer
-   상수의 이름은 모두 대문자로 한다. 여러 단어로 이루어진 경우 \_로 구분한다.
    
    -   PI, MAX\_NUMBER
​
## 변수의 이름
​
-   변수의 이름은 짧을 수록 좋지만, 약간 길더라도 용도를 알기 쉽게 '의미있는 이름'으로 하는 것이 바람직하다. 변수의 선언문에 주석으로 변수에 대한 정보를 주는 것도 좋은 생각이다.

## 패키지 저장
- 패키지 형식은 카멜케이스 x

## bolean 
- is 로표시하기

## 메서드의 이름
- 메서드는 특정 작업을 수행하므로 메서드의 이름 'add'처럼 동사인 경우가 많으며, 이름만으로도 메서드의 기능을 쉽게 알 수 있도록 함축적이면서도 의미있는 이름을 짓도록 노력해야 한다.
                          