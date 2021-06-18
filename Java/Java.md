
## Java 복습 
--- 



## 메모리 구조

- 메소드 영역
  - 자바 프로그램에서 사용되는 클래스에 대한 정보와 함께 클래스 변수(static variable)가 지정되는 영역
  
- 힙(heap) 영역
  - 모든 인스턴스 변수가 저장되는 영역
  - new 키워드를 사용하여 인스턴스가 생성되면, 해당 인스턴스의 정보를 힙 영역에 저장
  -  __메모리의 나증은 주소에서 높은 주소의 방향으로 할당 __ 

 - Stack 영역
   - 메소드가 호출될 떄 메소드의 스택 프레임이 저장되는 영역

   - JVM은 자바 프로그램에서 메소드ㅏㄱ 호출되면, 메소드의 호출과 관계되는 지역 벼눗와 매개변수를 스택 영역에 저장

   - 스텍 영역은 메소드의 호출과 함께 할당되며, 메소드의 호출이 완료되면 소멸

   - 높은 주소에서 낮은 주소의 방향으로 할당

--- 
## 제너릭(generic)

- 데이터 타입을 일반화한다는 것을 의미

- 클래스나 메소드에서 사용할 내부 데이터 타입을 컴파일 시에 미리 지정하는 방법

```
특징 : 
1. 클래스나 메소드 내부에서 사용되는 객체의 타입 안정성을 높일 수 있다.
2. 반환값에 대한 타입 변환 및 타입 검사에 들어가는 노력을 줄일 수 있다.
```

- 사용예시

```java

public class yushin<T>{
    T Food;
}

TestYushin<String> test = new TestYushin<>();
		String a = "좋아하는음식기록";
		test.Food = a;
		System.out.println("test.food ==> "+ test.Food);


```
--- 

## java.util.Optional <T> 클래스

- Integger나 Double 클래스 처럼 T타입의 객체를 포장해 주는 래퍼 클래스 -> __모든 타입의 참조 변수를 저장할 수 있다.__

- Optional 객체를 사용하면 예상치 못한 NullPointerException 예외를 제공되는 메소드로 간단히 회피  __ofNullable() 메소드__


```java
Optional<String> name = Optional.ofNullable("이름을 명시 안하면 비어있는 Optional객체를 반환 " )

```