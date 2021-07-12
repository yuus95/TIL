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


