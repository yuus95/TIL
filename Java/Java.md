
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
## 생성자를 이용한 final멤버 변수의 초기화

- final 변수 
  - "마지막의 또는 변경될 수 없는"의 의미를 가지고 있으며 거의 모든 대상에 사용될 수 있다.

  - 변수에 사용되면 값을 변경할 수 없는 상수가 되며, 메서드에 사용되면 오버라이딩을 할 수 없게 된다. 클래스에 사용되면 자신을 확장하는 자손 클래스를 정의하지 못하게 된다.

  ```
  final이 사용될 수 있는 곳 - 클래스,메서드,멤버변수, 지역 변수
  ```
- 생성자를 이용한 final멤버 변수의 초기화

  - final이 붙은 변수는 상수이므로 일반적으로 선언과 초기화를 동시에 하지만, 인스턴스 변수의 경우 생성자에서 초기화 되도록 할 수 있다.

  - 이기능을 화룡하면 각 인스턴스마다 final이 붙은 멤버변수가 다른 값을 갖도록 하는것이 가능하다

  ```java
  class card{
    finial int Number;
    final int String KIND;
    Card(String kind,int num){
      KIND = kind;
      Number = num;
    }
  }
  ```

---
## 인터페이스

- 인터페이스는 일종의 추상 클래스이다. 추상화 정도가 높아서 추상클래스와 달리 몸통을 갖춘 일반 메서드 또는 멤버변수를 구성원으로 가질 수 없다. 
- 다른 클래스를 작성하는데 도움 줄 목적으로 작성된다.

- 작성
  ```java
  interface 인터페이스이름{
    public static final 타입 상수이름 = 값;
    public abstract 메서드 이름(매개변수 목록);
  }
  // 모든 멤버 변수는 public static final이어야 하며, 이를 생략 할 수 있다.
  // 모든 메서드는 public abstract이어야 하며 이를 생략할 수 있다.
  // static메서드와 디폴트 메서드는 예외
  ```

- 상속
  - 인터페이스는 인터페이스로부터만 상속받을 수 있으며 클래스와는 달리 다중상속, 즉 여러 개의 인터페이스로부터 상속을 받는 것이 가능하다.

  ```java
  interface Movable{
    void move(int x,int y)
  }

  interface Attackable{
    void attack(Unit u);
  }

  interface Fightable extends Movable,Attackable{}
  ```

- 구현
  - 인터페이스도 추상클래스처럼 그 자체로는 인스턴스를 생성할 수 없으며 추상클래스가 상속을 통해 추상 메서드를 완성하는 것처럼, 인터페이스도 자신에 정의된 추상메서드의 몸통을 만들어주는 클래스를 작성해야 한다.

  ```java
  
  class 클래스이름 implements 인터페이스이름{

  }

  // 만일 구현하는 인터페이스의 메서드 중 일부만 구현한다면, abstract를 붙여서 추상클래스로 선언해야 한다.

  abstract class Fighter implements Fightable{
    public void move(int x, int y ){
      this.x +=x
      this.y += y
    }
  }


  // 상속과 구현을 동시에 하는법
  class A extend b implemnets b{
    public void move(int x , int y ){
      this.x +=x
      this.y += y
    }
  }

- 오버라이딩 할 때는 조상의 메서드보다 넓은 범위의 접근 제어자를 결정해야한다.
  - interface메소드는 public abstract이 기본적으로 지정되있으므로,
  구현할 때는 제어자를 public으로 반드시 해줘야 한다.

  
- 인터페이스를 이용한 다형성
  - 다형성 : 자손클래스의 인스턴스를 조상타입의 참조변수로 참조하는것이 가능하다.

  - 인터페이스 타입의 매개변수가 갖는 의미는 메서드 호출 시 해당 인터페이스를 구현한 클래스의 인스턴스를 매개변수로 제공해야 한다는 것이다.
    ```java
    Fightable method(){
      return new firter() 
    }
    ```


    ```java
     static void main(String argsp[]){
       b test = makeA.getAClass();  
      // 리턴타입이 인터페이스이므로 구현한 클래스의 인스턴스를 반환 한다.
       // b test = new A(); 다형성을 사용한것과 같다. 조상 타입의 참조변수로 사용할 수 있다. 

       test.testmethod();
     }

     class makeA {
       public static b getAClass{
         return new a();
       }
     }


     class a implements b{
      public void testmethod(){
        Syetem.out.println("b인터페이스 테스트");
      }
     }


     interface b{
       void testmethod(); // public abstract 생략가능
     }

    ```

- 인터페이스 장점
  - 개발시간을 단축시킬 수 있다.
    - 인터페이스를 작성해놓으면  여러군데에서 인터페이스를 통해 개발을 할 때 동시에 개발을 할 수 있다. 
- 표준화
  - 프로젝트에 사용되는 기본 틀을 인터페이스로 작성한다음 개발자들에게 인터페이스를 구현하여 프로그램을 작성하도록 함으로써 보다 일관되고 정형화된 프로그램의 개발이 가능하다.

- 서로 관계 없는 클래스들에게 관계를 맺어 줄 수 있다.
  - 서로 아무런 관계도 없는 클래스들에게 하나의 인터페이스를 공통적으로 구현하도록 함으로써 관계를 맺어 줄 수 있다.

- 인터페이스 사용예시
  - Barrack, factory,a,b..  > build에 속해 있는 자식 클래스들 
  - Barrack,factory에만 따로 기능을 추가하고 싶을 때 인터페이스사용
  ```java
  interface Liftable{
    void liftoff(); // 물건들어올리기
  }

  //Liftable의 메소드 기능 구현 클래스
  class LiftableImpl implements Liftable{
    public void liftoff(){
      // 함수기능구현
    }

  class Barrack extends Building implements Liftable{
    LiftableImpl l = new LiftableImpl();
    void liftOff() { I.liftOff();} // LiftableImpl 클래스를 이용하여 메소드 기능 구현 
    } 
  }
  ```

- 인터페이스 이해
  - 클래스를 사용하는 쪽과 클래스를 제공하는 쪽이 있다.
  - 메서드를 사용하는 쪽에서는 사용하려는 메서드의 선언부분만 알면 된다.
  ```java
  class InterfaceTest3{
    public static void main(String[] args){
      A a = new A();
      a.methodA();
    }
  }


  class A{
    void methodA(){
      I i=  InstanceManager.getInstance();
      i.methodB();
      System.out.println(i.toString()); // i로 Object클래스이 메서드 호출 가능
    }
  }

  interface I {
    public abstract void methodB();
  }

  class B implements I{
    public void methodB(){
      System.out.println("methodB in B class");

    }
    public String toString() {return "Class B"};

  }

  class InstanceManager{
    public static I getInstance(){
      return new B();
    }
  }

  // 모든 객체는 Object클래스에 정의된 메서드를 가지고 있을 것이기 떄문에 ToString사용이 허용된다.
  ```


---
## 다형성(polymorphism)

- 다형성
  - 조상클래스 타입의 참조변수로 자손클래스의 인스터스를 참조할 수 있도록 하였다는 것.

  ```java

  // CationTv는 tv를 상속받음
  Tv t = new Tv();
  CpationTv c = new CationTv();
  Tv t = c

  // t 는 CaptionT 인스터스의 모든 멤버를 사용할 수 없다.
  // 둘 다 같은 타입 인스턴스지만 참조변수의 타입에 따라 사용할 수 있는 멤버의 개수가 달라진다.
 
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
## 내부 클래스
- 장점
  - 내부 클래스에서 외부 클래스의 멤버들을 쉽게 접근할 수 있다.
  - 코드의 복잡성을 줄일 수 있다(캡슐화)


  ```java
  public class innerEx1 {
      public static void main(String args[]){
      innerEx1 test1 = new innerEx1();
      System.out.println("test1.outIv" + innerEx1.StaticInner.scv);
      }
      private int outerIv = 0;
      static int outerCv= 0;

      class InstanceInner{
          int iiv = outerIv; // 외부 클래스의 private 멤버도 접근 가능하다
          int iiv2 = outerCv;
      }

      static class StaticInner{
  //        int siv = outIv; 스태틱 클래스는 외부 클래스의 인스턴스 멤버에 접근할 수 없다.
          static int scv = outerCv;
      }

      void myMethod(){
          int lv = 0;
          final int LV=0; // final 생략가능
          class LocalInner{
              int liv = outerIv;
              int liv2 = outerCv;

              int liv3 = lv;
              int liv4 = LV;
          }

      }

  }
  ```


---
## Exception
- 오류 (에러 - 예외)
  - 에러: 프로그램 코드에 의해서 수습될 수 없는 심각한 오류
  - 예외 프로그램 코드에 의해서 수습될 수 있는 다소 미약한 오류

- 예외 클래스
  - Exception 클래스들: 사용자의 실수와 같은 외적인 요인에 의해 발생하는 예외
    - checked예외라고불림
  - RuntimeException 클래스들 : 프로그래머의 실수로 발생하는 예외
    - unchecked예외라고불림  

- 모든 예외 클래스는 Exception 클래스의 자손이므로 catch블럭의 괄호에 Exception 클래스 타입의 참조 변수를 선언해 놓으면 어떤 종류의 예외가 발생하더라도 이 catch블럭에 의해서 처리된다.

- 예외가 발생했을 떄 생성되는 예외 클래스의 인스턴스에는 발생한 예외에 대한 정보가 담겨 있으며, getMessage()와 printStackTrace()를 통해서 얻을 수 있다.
  - getMessage() : 발생한 예외클래스의 인스턴스에 저장된 메시지를 얻을 수 있다.
  - printStackTrace() : 예외발생 당시의 호출스택에 있었던 메서드의 정보와 예외 메시지를 화면에 출력한다.  

- 멀티 catch블럭
  ```java
  catch(ExceptionA e){} 
  catch(ExceptionB e2{}

  // 두개를 한번에 사용할 수 있다.
  catch(ExceptionA | ExceptionB e){}
  
  ```
- 예외 발생시키기
  - 먼저 연산자 new를 이용해서 발생시키려는 예외 클래스의 객체를 만든다음
  
  - 키워드 throw를 이용해서 예외를 발생시킨다.

  ```java
  Exception  e =new Exception("예외 메세지 출력");
  throw e 
  // 생성자에 String을 넣어주면 이 STring 이 Exception인스턴스에 메시지로 저장된다 --> Getmessage()로 얻을 수 있다.
  ```

  - ### 사용자정의 예외만들기
  ```java

  public class test1 {
      public static void main(String args[]) throws myException {
          System.out.println("Test START");
          int  a= 5;
          Test2 start = new Test2(a);
          start.test_method(5);

      }

      static class Test2{
          int a ;

          public Test2(int num ){
              this.a = num;
          }

          void test_method(int a ) throws myException {
              if(a!= 2) {
                throw new myException("에러발생");
              }
          }

      }
  }

  // 예외클래스 별도로 만들기

  public class myException extends Exception{
      private final int ERR_CODE;

      public myException(String msg){
          this(msg,100);
      }
      public myException(String msg,int errCode){
          super(msg);
          ERR_CODE=errCode;
      }

  }


  ```
- ### 예외 선언하기
  
  - throw 예외 발생, throws 예외 선언
  - 메서드의 선언부에 예외를 선언함으로써 이 메서드를 사용하기 위해서 어떠한 예외들이 처리되어져야 하는지 쉽게 알 수 있다.
   

  ```java 
  void method() throws Exception1,Exception2{
  }
  ```


- ### 연결된 예외
  - Throwable initCause(Throwable cause) : 지정한 예외를 원인 예외로 등록
  - Throwable getCause() 원인 예외를 반환
  ```java
  static void startInstall() throws SpaceException,MemoryException{
    if (!enoughMemory())
    throw new MemoryException("메모리가 부족합니다") // checked예외
  }

  // unchecked예외로바꾸기

  static void startInstall2() throws SpaceException{
    if (!enoughMemory())
    throw new RuntimeException(new MemoryException("메모리가 부족합니다")); // RuntimeException으로 감싸버렸기 떄문에 unchecked예외가 되버림

    //checked예외를 unchecked예외로바꾸면 예외처리가 선택적이 되므로 억지로  예외처리를 하지 않아도 된다.
  }

  ```

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

- ## 래퍼 클래스
  - 기본 타입에 해당하는 데이터를 객체로 포장해 주는 클래스
    - byte,short,int,long,float,double,boolean,char

- ## Arrays 
  - 배열을 다루는데 유용한 메서드가 정의되어 있다.
    - 배열의 복사 : copyOf() - 전체복사 , copyOfRange(arr,2,4) 부분 복사
    - 배열 채우기 : 배열의 모든 요소를 지정된 값으로 채운다. setAll() 배열을 채우는데 사용할 함수형 인터페이스를 매개변수로 받는다.

- ## Stack , Queue
  - Stack
    - LIFO(Last in First out)
      - Object push() - 맨 끝에 데이터 추가
      - Object pop() - 맨 끝에 있는 데이터 추출

  - Queue 
    - FIFO(First In First Out)
      - Object poll() - 맨 앞에서객체를 꺼내서 반환
      - boolean Offer() - 맨 뒤에 객체를 저장
  - PriorityQueue
    - 우선순위 큐 : 우선순위가 제일 높은것부터 꺼내옴 - heap이라는 자료구조의 형태로 저장한다.
    - null을 저장하면 nullPointerException발생
      ```java
      Queue = pq = new PriorityQueue();
      pq.offer(3);
      pqp.offer(1);
      pq.offer(5);
      System.out.println(pq); // 1이 제일 먼저 출력 우선순위는 숫자가 작을수록 높다
      ```
  - Deque
    - Double-Ended-Queue :양쪽에서 추가/삭제가 가능한 큐(조상이 큐이다)
    - 구현체로는 ArrayDeque과 LinekedList등이 있다.

    - offerFirst,offerLast (데이터 저장)
    - pollFirst, pollLast (데이터 삭제 )
  
---

- ## Iterator

  - 컬렉션 프레임웍에서는 컬렉션에 저장된 요소들을 읽어오는 방법을 표준화 했다

  - 컬렉션에 저장된 각 요소에 접근하는 기능을 가진 Iterator 인터페이스를 정의하고, Collection인터페이스에는 iterator(iterator를 구현한 클래스의 인스턴스)를 반환하는 iterator()를 정의하고 있다.

  - 코드의 일관성을 유지하여 재사용성을 극대화하는 것이 객체지향 프로그래밍의 중요한 목적 중의 하나.
```java
// Iterator 인터페이스
public interface Iterator{
  boolean hasNext(); // 읽어 올 요소가 남아있는지 확인
  Object next(); // 다음 요소를 읽어온다
  void remove(); // next()로 읽어 온 요소를 삭제한다.(선택적 기능)
}

// Collection 인터페이스 일부

public interface Collection{
  public Iterator iterator(); // iterator를 구현한 클래스의 인스턴스르르 반환하는 메소드;
}

// 사용방법

public void static main(String args[]){
  Collection c = new ArrayList(); // 참조변수의 타입을 Collection을 해서 구현하면 Collection 다른 타입도 사용할 수 있게 재사용성을 보장해준다.
  Iterator it = c.iterator();
  
  while(it.hasNext()){
    System.out.print(it.nenxt());
  }
}

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