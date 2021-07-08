
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

   - JVM은 자바 프로그램에서 메소드가 호출되면, 메소드의 호출과 관계되는 지역 벼눗와 매개변수를 스택 영역에 저장

   - 스텍 영역은 메소드의 호출과 함께 할당되며, 메소드의 호출이 완료되면 소멸

   - 높은 주소에서 낮은 주소의 방향으로 할당
---
## 다형성(polymorphism)

- 다형성
  - 조상클래스 타입의 참조변수로 자손클래스의 인스터스를 참조할 수 있도록 하였다는 것.

  ```java

  // CationTv는 tv를 상속받음
  Tv t = new Tv();
  CpationTv c = new CationTv();
  Tv t = new CaptionTv();

  // t 는 CaptionT 인스터스의 모든 멤버를 사용할 수 없다.
  // 둘 다 같은 타입으 ㅣ인스턴스지만 참조변수의 타입에 따라 사용할 수 있는 멤버의 개수가 달라진다.
 
  ```
- 참조변수가 사용할 수  멤버의 개수는 인스턴스의 멤버 개수보다 같거나 적어야 한다.

- 조상타입의 참조변수로 자손타입의 인스턴스를 참조할 수 있다.
- 자손타입의 참조변수로 조상타입의 인스터스를 참조할 수 없다.


- 형변환
  - 자손 타입 > 조상 타입 (Up-Casting) : 형변환 생략 가능
  - 자손 타입 < 조상타입 (Down-casting) : 형변환 생략 불가

  ```java
  // FireEngine 은 Car을 상속 받음
  Car car = null
  FireEngine fe = new FireEngine();
  FireEngine fe2 = null;

  car = fe // 형변환 생략 가능 (car)fe
  fe2 = (FireEngine)car // 형변환 생략 불가능
  // 여기서 중요한점 car의 인스턴스 참조 타입을 봐야함 fireEngine이여야지만 가능 
  ```
  - 서로 상속관계에 있는 타입간의 형변환은 양방향으로 자유롭게 수행될 수 있으나, 참조변수가 가리키는 인스턴스의 자손타입으로형변환은 허용되지 않는다.  결국 참조변수가 가르키는 인스턴스의 타입이 무언인지 확인하는 것이 중요하다.

- instance of 
  - 어떤 타입에 대한 instanceof연산의 결과가 true라는 것은 검사한 타입으로 형변환이 가능하다는것이다.

- 참조변수와 인스턴스의 연결
  - 조상 클래스에 선언도니 멤버변수와 같은 이름의 인스턴스변수를 자손 클래스에 중복으로 정의했을 때, 조상타입의 참조변수로 자손 인스턴스를 참조하는 경우와 자손타입의 참조 변수로 자손 인스턴스를 참조하는 경우는 서로 다른 결과를 얻는다
    - 메서드의 경우 조상 클래스의 메서드를 자손의 클래스에서 오버라이딩한 경우에도 참조 변수의 타입에 관계없이 항상 실제 인스턴스의 메서드가 호출 되지만, 멤버변수의 경우 참조변수의 타입에 따라 달라진다.

    - static 메서드는 static 변수처럼 참조변수의 타입에 영향을 받는다.
    참조변수의 타입에 영향을 받지 않는 것은 인스턴스 메서드 뿐이다.
     
    - static 메서드는 반드시 참조변수가 아닌 클래스이름. 메서드()로 호출해야 한다.

  ```java
  //child 는 parent 를 상속
  // method 메소드 중복 ,  x 변수 중복

  Parent p = new Child();
  Child c = new Child();

  System.out.prinit(p.x); // parent x 
  
  System.out.prinit(c.x); // child x 
  
  System.out.prinit(p.method); // child method
  
  System.out.prinit(c.method); // child method
  

  ```
- 매개변수의 다형성
  ```java
  void buy(product p ){
    money =money - p.price;
    bonusPoint = ponusPoint + p.bounsPoint;
  }

  ```
  - 매개변수가 Product타입의 참조변수라는 것은 메서드의 매개변수로 Productc클래스의 자손타입의 참조변수면 어느것이나 매개변수로
  받아들일 수 있다. 

---
## 추상 클래스 

- 미완성 설계도 (미완성 메서드를 포함하고 있다는 의미)

- 공통 부분만은 그린 미완성 설계도 
  - ex) Tv - > 여러 종류의 모델이 있지만 90% 동일한 설계도 사용 : 공통 부분만을 그린 미완성 설계도를 만들어놓고 각각의 설계도 작성

- 추상 메서드 : 선언부만 작성하고 구현부는 작성하지 않은 채로 남겨 둔 것
  ```java
  // 주석을 통해 어떤 기능을 수행할 목적으로 작성됐는지 적어놓기
  abstract void play(int pos);
  ```

- 추상화 , 구체화
   - 추상: 낱낱의 구체적 표상이나 개념에서 공통된 성질을 뽑아 이를 일반적인 개념으로 파악하는 정신 작용
   - 추상화: 클래스 간의 공통점을 찾아내서 공통의 조상을 만드는 작업
   - 구체화:  상속을 통해 클래스를 구현, 확장하는 작업

- 사용예시
  ```java
    abstract class Unit{
      int x,y ;
      abstract void move(int x int y ); // 선언부만 작성하기 몸통은 자손클래스가 작성
    }

    class Marine extends Unit{
      void move(int x, int y) {} // 메소드 구현하기
    }

    
    class Tank extends Unit{
      void move(int x, int y) {} // 메소드 구현하기
    }

    Unit [] group = new Unit[2];

    group[0] = new Marine();
    group[1] = new Tank();

    for (int i = 0 : i< group.length; i++){
      group[i].move(100,200);
    }

  ```


---
## Call By Value
- 메소드에 값(primitive type)을 전달 하는 것과 객체(reference type)를 전달한느 것에는 큰 차이가 있다. 

- 메소드로 객체를 전달할 경우 메소드에서 객체의 객체변수값을 변경할 수 있게 된다.
```java
// 값전달
class value_Plus{
  public void num_plus(int count){
    count ++;
    //값을 전달 받았기 떄문에 반환타입이 없는 이상 값을 전달하지 않는다.
  }
}

  // 객체 변수
  class ref_Plus{
    public void num_plus(Num_class num_class){
      num_class.num ++;
    }
  }

public class Num_class{
  int num ;

  public static void main(String[] args){
    Num_class num_cl = new Num_class();

    num_cl.num = 1;

    value_Plus(num_cl);
    System.out.println(num_cl.num);

    ref_plus(num_cl);

    System.out.println(num_cl); //1 
    value_Plus(num_cl);
    System.out.println(num_cl.num); // 2

    // 객체를 전달 할 경우만 값이 변경된다.

    
  }
}
```



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

--- 
## 스트림

- 데이터의 흐름
- 배열 또는 컬렉션 인스턴스에 함수 여러 개를 조합해서 원하는 결과를 필터링 하고 가공된 결과를 얻을 수 있다.
- 람다를 이용해서 코드의 양을 줄이고 간결하게 표현할 수 있다.
- 간단하게 병렬 처리가 가능하다
- 스트림 생성 방법
  - 생성하기, 가공하기, 결과 만들기
```java
      //ORDER  데이터 2개
        List<Order> orders = orderRepository.findAllByString(new OrderSearch());


        List<SimpeOrderDto> result = orders.stream() // 생성하기
                .map(o -> new SimpeOrderDto(o)) //가공하기 Mapping 스트림 내 요소들을 하나씩 특정 값으로 변환 해 준다.
                .collect(Collectors.toList()); //결과 만들기 Stream elements를 List로 변경한 메서드
        return result;
```
- Collectors란 "Stream을 일반적인 List,Set등으로 변경시키는 Stream 매서드" 