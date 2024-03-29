
## Java 복습 
--- 

- 배열의 단점
  - 크기를 변경할 수 없다.
  - 비순차적인 데이티의 추가 또는 삭제에 시간이 많이 걸린다.

## 자바의 데이터를 다루는 최소단위
  - 1Byte
  - Boolean은 1bit만 필요하지만 최소단위보다 작으므로 1byte가되어버림

## 형변환

- boolean을 제외한 나머지 7개의 기본형은 서로 형변환이 가능하다
  - short와 ch는 서로 길이가 달라서 자동 형변환이 안된다.

- 기본형과 참조형은 서로 형변환할 수 없다.

- 서로 다른 타입의 변수간의 연산은 형변환을 하는 것이 원칙이지만, 값의 범위가 작은 타입에서 큰 타입으로의 형변환은 생략할 수 있다.

## 연산자

- 단항 연산자와 대입 연산자만 오른쪽에서 왼쪽의 순서로, 연산을 수행한다.
```java
x=y=3 
// y =3 
  // x = 3 
  // 뒤에서부터 수행된다.
```

## Switch

- switch문의 조건식 결과는 정수 또는 문자열이어야 한다.
- case문의 값은 정수 상수만 가능하며, 중복되지 않아야 한다.

```java
Scanner sc = new Scanner(System.in);
int month = scanner.nextInt();

switch(month){
  case 3:
  case 4:
  case 5:
    System.out.println("봄");
    break
  case 6:
  case 7:
  case 8: 
    System.out.println("여름");
    break
  case 9: case 10 : case 11:
  System.out.println("가을");
  case 12: case 1: case 2:
  System.out.println("겨울");

}

```

## 기본 자료형 & 참조형 변수  초기화
```
byte = 0 
short = 0
int = 0 
long = 0 
float = 0.0f

double = 0.0d
char = NULL
boolean = false
참조형 변수 = NULL
```

## 기본형 매개변수와 참조형 매개변수 

- 자바는 기본형 매개변수와 참조형 매개변수의 반환값을 달리한다.

- 함수내에서 값은? 115
- 기본값 참조후 height 100
- 기본값 참조후 height 115
- 자바에서는 메서드를 호출할 때 매개변수로 지정한 값을 메서드의 매개변수에 복사해서 넘겨준다.

- 매개변수의 타입이 기본형(Primitive type)일 때는 기본형 값이 복사되겠지만, 참조형(reference type)이면 인스턴스의 주소가 복사된다.

- 메서드의 매개변수를 기본형으로 선언하면 단순히 저장된 값만 얻지만,참조형으로 선언하면 값이 저장된 곳의 주소를 알 수 있기 때문에 값을 읽어 오는 것은

물론 값을 변경하는 것도 가능 하다.

 

 


```
public class test33 {
    public static void main(String[] args) {
        Human human = new Human();
        human.height= 100l;

        // 기본값 참조
        primitiveType(human.height);
        System.out.println("기본값 참조후 height " + human.height);

        referenceType(human);
        System.out.println("기본값 참조후 height "+ human.height);
    }

    static class Human {
        String name;
        Long height;
    }

    static void primitiveType(Long x) {
        x = x + 15;
        System.out.println("함수내에서 값은? " + x);
    }

    static void referenceType(Human human) {
        human.height += 15;
    }
```

결과
```
함수내에서 값은? 115
기본값 참조후 height 100
기본값 참조후 height 115
 ```


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
## 초기화 블럭(initialization block)

### 클래스 초기화 블럭: 클래스 변수의 복잡한 초기화에 사용된다.
### 인스턴스 초기화 블럭: 인스턴스 변수의 복잡한 초기화에 사용된다.

- 초기화 작업이 복잡하여 명시적 초기화만으로는 부족한 경우 초기화 블럭을 사용한다.

- 초기화 블럭 내에는 메서드 내에서와 같이 조건문, 반복문, 예외처리구문 등을 자유롭게 사용할 수 있다.

- 생성자 보다 인스턴스 초기화 블럭이 먼저 수행된다는 사실도 기억해 두자.

```java
static {/*클래스 초기화 블럭*/}
{/* 인스턴스 초기화 블럭*/}

```

- 인스턴스 변수의 초기화는 주로 생성자를 사용하고, 인스턴스 초기화 블럭은 모든 생성자에서 공통으로 수행돼야 하는 코드를 넣는데 사용한다.

```java
Car(){
  count__;
  serialNo = count;
  color= "White";
  gearType = "Auto";
}

Car(String color, String gearType){
  count++; 
  serialNo = count;
  this.color = color;
  this.gearType= gearType;
}
// count, serialNo이 중복됐다.

{
  count++;
  serialNo = count;
  // 인스턴스 초기화를 사용하여 따로 빼서 사용할 수 있다.
}

```

## 제어자

### 제어자란?
- 제어자는 클래스, 변수 또는 메서드의 선언부에 함께 사용되어 부가적인 의미를 부여한다.

```
접근제어자: public,protected,default,private
그 외: static,final ,abstract,native,transient,synchronized,volatile,strictfp
```

### static : 클래스의, 공통적인
```
스태틱이 사용될 수 있는 곳 - 멤버 변수 메서드, 초기화 블럭
```
- Static은 클래스의 또는 공통적인 의 의미를 가지고 있다.

- 인스턴스메서드와 static메서드의 근본적인 차이는 메서드 내에서 인스턴스 멤버를 사용하는가의 여부에 있다.

- 인스턴스 멤버를 사용하지 않는 메서드는 static을 붙여서 static 메서드로 선언하는 것을 고려하자.


- 스태틱 멤버변수
  - 모든 인스턴스에 공통적으로 사용되는 클래스변수가 된다.
  - 클래스변수는 인스턴스를 생성하지 않고도 사용 가능하다.
  - 클래스가 메모리에 로드될 때 생성된다.
- 메서드 
  - 인스턴스를 생성하지 않고도 호출이 가능한 static메서드가 된다.
  - static메서드 내에서는 인스턴스 멤버들을 직접 사용할 수 없다.
  



### final - 마지막의 변경될 수 없는

```
final이 사용될 수 있는 곳 - 클래스, 메서드, 멤버변수, 지역변수
```
- 변수에 사용되면 값을 변경할 수 없는 상수가 되며, 메서드에 사용되면 오버라이딩을 할 수 없게 되고, 클래스에 사용되면 자신을 확장하는 자손 클래스를 정의하지 못하게 된다.

  - 클래스
    - 변경될 수 없는 클래스, 확장 될 수 없는 클래스가 된다.
    - 다른 클래스의 조상이 될 수 없다.
  - 메서드
    - 변경될 수 없는 메서드, final로 지정된 메서드는 오버라이딩을 통해 재정의 될 수 없다.
  - 멤버변수 & 지역변수
    - 변수 앞에 final이 붙으면, 값을 변경할 수 없는 상수가 된다.
 
- 생성자를 이용한 final멤버 변수의 초기화

  - final붙은 변수는 상수이므로 일반적으로 선언과 초기화를 동시에 하지만 인스턴스 변수의 경우 생성자에서 초기화 되도록 할 수 있다.

  - 클래스 내에 매개변수를 갖는 생성자를 선언하여, 인스턴스를 생성할 대 final이 붙은 멤버변수를 초기화하는데 필요한 값을 생성자의 매개변수로부터 제공 받으면 각 인스턴스마다 final이 붙은 멤버변수가 다른 값을 갖도록 하는 것이 가능하다.


### abstract - 추상의 미완성의

- abstract는 미완성의 의미를 가지고 있다. 메서드의 선언부만 작성하고 실제 수행내용은 구현하지 않은 추상 메서드를 선언하는데 사용된다.

```java
abstract가 사용될 수 있는 곳 - 클래스,메서드
```
- 추상 클래스는 아직 완성되지 않은 메서드가 존재하는 '미완성 설계도'이므로 인스턴스를 생성할 수 없다.

```java
abstract class AbstractTest{ // 추상클래스
  abstract void move(); // 추상메서드
}

```

### 접근 제어자(access modifier)
- 접근 제어자는 멤버 또는 클래스에 사용되어, 해당하는 멤버 또는 클래스를 외부에서 접근하지 못하도록 제한하는 역할을 한다.

- 생성자에 접근 제어자가 지정되어 있지 않다면, 접근 제어자가 default임을 뜻한다.

```
private : 같은 클래스 내에서만 접근이 가능하다.
default:  같은 패키지 내에서만 접근이 가능하다.
protected:  같은 패키지 내에서, 그리고 다른 패키지의 자손 클래스에서 접근이 가능하다.
public : 접근 제한이 전혀 없다.
```

- 접근 제어자를 이용한 캡슐화
  - 클래스의 내부에 선언된 데이터를 보호하기 위해서이다.
  - 데이터가 유효한 값을 유지하도록, 또는 비밀번호와 같은 데이터를 외부에서 함부로 변경하지 못하도록 하기 위해서는 외부로부터의 접근을 제한하는 것이 필요하다.

### 생성자의 접근 제어자

- 생성자에 접근 제어자를 사용함으로써 인스턴스의 생성을 제한할 수 있다.

- 생성자의 접근 제어자를 private으로 지정하면, 외부에서 생성자에 접근할 수 없으므로 인스턴스를 생성할 수 없게 된다. 그래도 클래스 내부에서는 인스턴스를 생성할 수 있다.

```java
//  인스턴스를 생성해서 반환해주는 public메서드를 제공함으로써 외부에서 이 클래스의 인스턴스를 사용하도록 할 수 있다.
// 메서드는 public인 동시에 static이여야 한다.
class Singleton{
  private Singleton();

  pivate static Singleton s = new Singleton();

  public static Singleton getInstance(){
    return s;
  }
}
// 인스턴스의 개수를 제한할 수 있다.
```
- 생성자가 private인 클래스는 다른 클래스의 조상이 될 수 없다.
  - 자손클래스의 인스턴스를 생성할 때 조상클래스의 생성자를 호출해야만 하는데, 생성자의 접근 제어자가 private이므로 자손 클래스에서 호출하는 것이 불가능하기 떄문이다.

  - final을 추가하여 상속할 수 없는 클래스라는 것을 알리는 것이 좋다.


### 제어자 조합
```bath
1. 메서드에 static과 abstract를 함께 사용할 수 없다.
 >> static메서드는 몸통이 있는 메서드에만 사용할 수 있기 떄문이다.

2. 클래스에 abstract와 final을 동시에 사용할 수 없다.
>> 클래스에 사용되는 final은 클래스를 확장할 수 없다는 의미이고, abstract는 상속을 통해서 완성되어야 한다는 의미이므로 서로 모순 되기 때문이다.

3. abstract메서드의 접근 제어자가 private일 수 없다.
>>abstract메서드는 자손클래스에서 구현해주어야 하는데 접근 제어자가 private이면, 자손클래스에서 접근할 수 없기 때문이다.

4. 메서드에 private과 final을 같이 사용할 필요는 없다.
>> 접근제어자가 private인 메서드는 오버라이딩될 수 없기 떄문이다. 이 둘 중 하나만 사용해도 의미가 충분하다.

```


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
      System.out.println("test1.outIv" + innerEx1.StaticInner.scv); // 0 
      StaticInner test2 = new StaticInner();
      System.out.println("test2 + " + test2.scv2 ); // 0 출력가능

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
          int scv2 = outerCv;

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
- Static 클래스
  - static 키워드를 이용하여 이너 클래스를 생성하면 상위 클래스와 분리를 해준다.
  - 상위 클래스가 생성되지 않아도, 외부에서 직접 객체를 생성할 수 있다.
  ```java


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
```
---
## Set

- ### HashSet
  - Set인터페이스를 구현한 가장 대표적인 컬렉션 중복된 요소를 저장하지 않는다.

  ```java
  public class hashset1 {
      public static void main(String[] args) {
          Object [] objArr= {"1",new Integer(1), "2","2","3","3","4","4","4"};
          Set set = new HashSet();

          for (int i = 0; i < objArr.length; i++){
              set.add(objArr[i]);
          }
          // 1은 String 객체와 Integer 객체가 있어서 2개가 나온다.
          System.out.println("set  =" +set);
      }
  }
  

- ### TreeSet
```java
/**
 * TreeSet은 이진 검색 트리라는 자료구조의 형태로 데이터를 저장하는 컬렉션 클래스이다.
 * 이진 검색 트리는 정렬, 검색, 범위검색에 높은 성능을 보이는 자료구조이며 TreeSet은 이진 검색 트리의 성능을 향상시킨 레드-블랙 트리로 구현되어 있다.
 * 중복된 데이터의 저장을 허용하지 않으며 정렬된 위치에 저장하므로 저장순서를 유지하지도 않는다.
 */
public class Tree {
    public static void main(String args[]){
        TreeSet set = new TreeSet();
        
        
        // TresSet integer의 자동 정렬방식이 사용됐음.
        for ( int i = 0; set.size() <6; i++){
            int num = (int)(Math.random()* 100) +1;
            set.add(num);
        }

        System.out.println("set" + set);


        System.out.println("50보다 작은 값" + set.headSet(new Integer(50)));
        System.out.println("50보다 큰 값" + set.tailSet(new Integer(50)));
        System.out.println("첫 번째 값 " + set.first());
        System.out.println("첫 번째 값 " + set.ceiling(new Integer(50)));
        System.out.println("첫 번째 값 " + set.higher(new Integer(50)));


    }

}


/**
 *  트리 기본 구조
 *  이진 검색 트리
 *      - 모든 노드는 최대 두 개의 자식노드를 가질 수 있다.
 *      - 왼족 자식노드의 값은 부모녿의 값보다 작고 오른쪽 자식 노드의 값은 부모 노드의 값보다 커야한다.
 *      - 노드의 추가 삭제에 시간이 걸린다.
 *      - 검색(범위 검색)과 정렬에 유리하다
 *      - 중복된 값을 저장하지 못한다.
 */
class TreeNode{
    // TreeSet에 저장되는 객체가 Comparable을 구현하던가 아니면, TreeSet에게 Comparator를 제공해서 두 객체를 비교할 방법을 알려줘야 한다. 그렇지 않으면 TreeSet에 객체를 저장할 떄 예외가 발생한다.
    TreeNode left; // 왼쪽 자식 노드
    Object element; // 객체를 저장하기 위한 참조변수
    TreeNode right; // 오른쪽 자식노드
}

/**
 * 메서드  - 반환 - 설명
 *   add : 지정된 객체 또는 컬렉션들을 넣는다.
 *  first() : Object  :정렬된 순서에 첫번쨰 객체 반환
 *  last() : Object : 정렬된 순서에 마지막 객체 반환
 *  ceiling(Object o ): Object  :지정된 객체와 같은 객체 반환, 없으면 큰값중 가장 가까운 객체 반환, 없으면 null
 *  floor(Object o ) : Object : 지정된 객체와 같은 객체 반환 없으면 작은 값중 가장 가까운 객체 반환 없으면 null
 *  higher (Object o ) : Object : 지정된 객체중에서 큰값중 제일 가까운값 반환, 없으면 null
 *  lower(Object o ) : Object : 지정된 객체중에서 작은값중 제일 가까운 값 반환, 없으면 null
 *  subSet(Obejct formElement, Object toElement): SortedSet : fromelement부터 toElement까지의 객체들을 반환 toElement미포함
 *  headSet(Object toElement): SortedSet 지정된 객체보다 작은 값의 객체들을 반환
 *  tailSet(Object form Element): SortedSet : 지정된 객체보다 큰값의 객체들을 반환
 */

```

---

- ## Map

  - ### HashMap
```java

public class map1 {

    public static void main(String args[]){
        HashMap map = new HashMap();
        map.put("myID","1234");
        map.put("asdf","1234");
        map.put("asdf","1234");
        map.put("myID2","1234");

        Scanner s = new Scanner(System.in);
        while(true){
            System.out.println("id 와 password를 입력해주세요");
            System.out.print("id : ");
            String id = s.nextLine().trim();

            System.out.println("password ");
            String password = s.nextLine().trim();
            System.out.println("");

            if (!map.containsKey(id)) {
                System.out.println("입력하신 id는 존재하지 않습니다." + " 다시 입력해 주세요");
                continue;
            }
            else{
                if(!map.get(id).equals(password)){
                    System.out.println("비밀번호가 일치하지 않습니다.");
                }
                else{
                    System.out.println("일치 ");
                    break;
                }
            }
        }


    }
}



/**
 * HashMap 메서드
 *
 * HashMap() : 객체를 생성
 * contatinsKey(Object key) : HashMap에 지정된 키가 포함되어 있는지 알려준다(포함 되어있으면 True)
 * contatinsValue(Object value) : HashMap에 지정된 값이 포함되어 있는지 알려준다(포함되어 있으면 True)
 * Set entrySet() : HashMap에 저장된 키와 값을 엔트리(키와 값의 결합)의 형태로 Set에 저장해서 반환
 * Object get(Object key) 지정된 키의 값을 반환 못찾으면 null반환
 * Object getOrDefault(Obejct key, Object defaultValue) : 지정된 키와 값을 반환한다. 키를 못찾으면, 기본 값으로 지정된 객체를 반환
 * boolean isEmpty() : HashMapl이 비어있는지 알려준다.
 * Set keySet() : HashMAp에 저장된 모든 키가 저장된 Set을 반환
 * Object put(Obejct key, Object value) 지정도니 키와 값을 HashMap에 저장
 * Obeject replace(Object key, Object value) : 지정된 키의값을 지정된 객체로 대체
 * boolean replact(Object key, Object oldValue,Object newValue) : 지정된 키와 객체가 모두 일치하는 경우에만 새로운 객체로 대체
 * int size() : 저장된 요소의 개수를 반환환
 * Collection values(): HashMap에 저장된 모든 값을 컬렉션의 형태로 반환.
 */

```

- 해싱과 해시 함수
  - 해싱이란 해시함수를 이용해서 데이터를 해시테이블에 저장하고 검색하는 기법을 말한다. 해시함수는 데이터가 저장되어있는 곳을 알려 주기 때문에 다량의 데이터 중에서도 원하는 데이터를 빠르게 찾을 수 있다.

  - 해싱을 구현한 컬렉션 클래스로는 HashSet,HashMap,Hashtable등이 있다.

  - 저장할 데이터의 키를 해시함수에 넣으면 배열의 한 요소를 얻게 되고, 다시 그 곳에 연결되어 있는 링크드 리스트에 저장하괴 된다.

    - 검색 하고자 하는 값의 키로 해시함수를 호출한다.
    - 해시함수의 계산 결과로 해당 값이 저장되어 있는 링크드 리스트를 찾는다.
    - 링크드 리스트에서 검색한 키와 일치하는 데이터를 찾는다.
  
  - 해시함수가 서로 다른키에 대해서 중복도니 해시코드의 반환을 최소화해야 한다. 그래야 HashMap에서 빠른 검색시간을 얻을 수 있다.
    - 배열은 배열의 크기가 커져도 원하는 요소가 몇 번째에 있는 지만 알면 빠르게 원하는 값을 찾을 수 있기 떄문에 해시코드 중복을 피하고 다른 해시코드값을 넣어 중복을 최소화하여 각각 배열에 넣어놔야 속도가 빠르다.

  - 해싱을 구현하는 과정에서 제일 중요한 것은 해시함수의 알고리즘이다.
  - 새로운 클래스를 정의할 떄 equals()를 재정의오버라이딩해야 한다면, hashCode()도 같이 재정의해서 equals)의 결과가 true인 두 객체의 해시코드hashCode()의 결과값이 항상 같도록 해주어야 한다.

    - equals()로 비교한 결과가 false이고 해시코드가 같은 경우는 같은 링크드 리스트에 저장된 서로 다른 두 데이터가 된다.

  - ### TreeMap
    - 검색 성능은 HashMap이 더 뛰어나므로 검색 할 경우에 HashMap사용
    -  범위검색이나 정렬이 필요한 경우에 사용한다.

  - entrySet()
    ```java
    map.entrySet() -> [A=4,B=5,C=6]  // 이런식으로 저장된다

    ```

  - ### 컬렉션 특징
    - ### 컬렉션 프레임웍에 속한 대부분의 컬렉션 클래스들은 이처럼 서로 변환이 가능한 생성자를 제공하므로 이를 이용하면 간단히 다른 컬렉션 클래스로 데이터를 옮길 수 있다.
    ```java
    Arraylist al = new ArrayList(1000);
    LinkedList ll = new LinkedList(al);
    ``` 
    - ArrayList
      - 배열 기반, 데이터의 추가와 삭제에 불리, 순차적인 추가삭제는 제일빠름. 임의의 요소에 대한 접근성이 뛰어남
    - LinkedList
      - 연결기반, 데이터의 추가와 삭제에 유리, 임의의 요소에 대한 접근성이 좋지 않다.
    - HashMap
      - 배열과 연견이 결합된 형태, 추가 삭제,검색, 접근성이 모두 뛰어남
      검색에는 최고 성능으 보인다.
    - TreeMap
      - 연결 기반, 정렬과 검색(특히 범위 검색)에 적합, 검색성능은 HashMap보다 떨어짐
    - Stack 
      - Vector를 상속받아 구현
    - Queue 
      - LinkedList가 Queue인터페이스를 구현
    - Propertise
      - Hashtable을 상속받아 구현
    - HashSet
      - HashMap을 이용해서 구현
    - TreeSet 
      - TreeMap을 이용해서 구현
    - LinkedHashMap,LinkedHashSet
      - HashMap,HashSet에 저장 순서유지기능을 추가
--- 
## 제네릭(generic)

- 데이터 타입을 일반화한다는 것을 의미

- 클래스나 메소드에서 사용할 내부 데이터 타입을 컴파일 시에 미리 지정하는 방법

```
특징 : 
1. 클래스나 메소드 내부에서 사용되는 객체의 타입 안정성을 높일 수 있다.
2. 반환값에 대한 타입 변환 및 타입 검사에 들어가는 노력을 줄일 수 있다.
```
- 생성
  - 참조변수와 생성자에 대입된 타입(밴개변수화된 타입)이 일치해야 한다.
     - 생성자에 타입을 지정을 안해도된다. 
  - 타입 T가 Fruit인 경우 void add(fruit item)가 있다고 가정할 떄 Fruit의 자손들은 이 메서드의 매개변수가 될 수 있다.
  ```java
  Box<Fruit> fruitBox = new Box<Fruit>();
  fruitBox.add(new Apple());

  ```
  - 제한된 클래스
    - 제네릭 타입에 extends를 사용하면 특정 타입의 자손들만 대입할 수 있게 제한할 수 있다.
    - 인터페이스를 구현해야 한다는 제약이 필요할 떄도 implermnet가 아니아
    extends를 써야한다.
  ```java
  class FruitBox<T extends Fruit>{
    ArrayList<T> list = new ArrayList<T>();
  }


  //인터페이스 제한
  ineterface Eatable{}
  class FruitBox<T extends Eatable>
  ```

- 사용예시

```java

public class yushin<T>{
    T Food;
}

yushin<String> test = new yushin<>();
		String a = "좋아하는음식기록";
		test.Food = a;
		System.out.println("test.food ==> "+ test.Food);


```
- ### 제한
  - 모든 객체에 대해 동일하게 동작해야하는 static 멤버에 타입 변수 T를 사용할 수 없다.

- ### 와일드 카드
  - 제네릭 클래스의 객체를 메소드의 매개변수로 받을 때, 그 객체의 타입 변수를 제한하는 것을 말한다.
  - 메소드를 만드는 개발자의 관점에서 메소드에서 사용 될 매개변수가 제네릭 클래스를 구현한 객체일떄, 그 제네릭 클래스 '타입 변수'를 제한 하는것이다.

  -  방법
      ```
      - <? extneds T> : 와일드 카드의 상한 제한, T와 그 자손들만 가능
      - <? super T> : 와일드 카드의 하향 제한, T와 그 조상들만 가능
      - <?> 제한 없음 모든타입이 가능
      ```

      
  ```java

  static Juice makeJuice(FruitBox<? extends Fruit> box){
    
  }
  ```
  
- ### Enum(열거형)
  - 서로 관련된 상수를 편리하게 선언하기 위한 것으로 여러 상수를 정의할 떄 사용하면 유용하다. 

  - 정의하는 방법
    ```java
    enum 열거형이름 {상수명1, 상수명2}
    ```
  - 예시
   ```java
  public class enum_ex1 {
      public static void main(String args[]){
      Unit test1 = new Unit();
      test1.init();
      System.out.println("test.dir " + test1.dir); // EAST


      }
  }

  class Unit{
      int x,y;
      enum Direction{EAST,SOUTH,WEST,NORTH}

      Direction dir;
      void init(){
          dir = Direction.EAST;
      }
  }


   ```

  - 열거형 상수간 비교에는 ==를 사용할 수 있다. equals()가 아닌 '=='로 비교가 가능하다는 것은 그만큼 빠른 성능을 제공할 수 있다는 의미

  -Enum클래스에 정의된 메서드
  ```
    - Class<E> getDeclaringClass() : 열거형의 Class 객체를 반환
    - String name() : 열거형 상수의 이름을 문자열로 반환
    - int ordinal() 열거형 상수가 정의된 순서를 반환한다.(0부터시작)
    - T valueOf(Class<T> enumType,String name)
  ```

- ### 익명 클래스


```java
public class Anonymous_Class {
    public static void main(String[] args) {

        // 익명 클래스  - 무명의 어떠한 클래스가 부모클래스(Test)로 부터 상속을 받은 인스턴스 생성
        // 이름이 없으므로 생성자를 선언 할 수도 없다.
        // 자바에서는 함수를 일급객체로 취급하지 못하지만, 위에서와 같이 익명클래스를 사용하면 메서드 자체는 못넘기되 메서드를 감싼 익명클래스를 넘길 수는 있다.
        Test t1 = new Test(){
            public int num = 10;
            public int getNum(){
                return this.num;
            }
        };

        Test t2 = new Test();

        System.out.println("test1 " + t1.getNum()); //t1.num을 출력 10
        System.out.println("test2 " + t2.getNum());
    }
}



class Test {
    private int num = 1 ;
    public int getNum(){
        return this.num;

    }

    public void setNum(int num){
        this.num = num;
    }
}


```

- ### 익명 클래스를 활용하여 Enum클래스 만들기

```java


//익명 클래스를 활용하여 enum타입 만들어보기
public class enum_class2 {
    public static void main(String[] args) {   
      MyTransportation t1 = Mytransportation.BUS;
      System.out.println("t1.name"+  t1.name()+ t1.ordinal());

    }
}

// enum 클래스
abstract class MyEnum<T extends MyEnum<T>> implements Comparable<T> {
    static int id=0;
    int ordinal;
    String name= "";

    public int ordinal(){return ordinal;}


    MyEnum(String name){
        this.name = name;
        ordinal = id++;
    }
    public int compareTo(T t){
        return ordinal-t.ordinal;
    }
}

abstract class MyTransportation extends MyEnum{

    // 열거형 만들기 각각 열거형 상수에 추상메서드 구현
    static final MyTransportation BUS = new MyTransportation("BUS",100) {
        int fare(int distance){return distance * BASIC_FARE;}
    };

    static final MyTransportation TRAIN = new MyTransportation("TAXI",300) {
        int fare(int distance){return distance * BASIC_FARE;}
    };

    

    // 열거형에 추상메서드 생성
    abstract int fare(int distance);

    protected final int BASIC_FARE;

    private MyTransportation(String name,int basicFare){
        super(name);
        BASIC_FARE = basicFare;
    }

    public String name(){return name;}
    public String toString(){return name;}

}



```
---
 ## 어노테이션

- 프로그램의 소스코드 안에 다른 프로그램을 위한 정보를 미리 약속된 형식으로 포함 시킨것.

- 주석처럼 프로그래밍 언어에 영향을 미치지 않으면서도 다른 프로그램에게 유용한 정보를 제공할 수 있다

```java
@Test// 이 메서드가 테스트 대상임을 테스트 프로그램에게 알린다.
public void method(){
  
}
```

- 오버라이딩할 때 메서드 앞에 @Override를 붙이면  알아내기 어려운 실수를 미연에 방지할 수 있다.

- @Deprecated : 더 이상 사용되지 않는 필드나 매서드에 적용 (메서드나 필드가 다른것으로 대체 되었다는 의미를 준다.)

--- 
## 람다

- 람다식: 메서드를 하나의 식으로 표현한 것
- 메서드를 람다식으로 표현하면 메서드의 이름과 반환값이 없어지므로, 람다식을 익명 함수라고도 한다.
```java
int [] arr = new int[5];
Arrays.setAll(arr,(i)->(int)(Math.random()*5)+1);

//원래식
int method(){
  return (int)(Math.random()*5) +1;
}
```

- 사용법
  - 반환값이 있는 메서드의 경우 return문 대신 식으로 대신할 수 있따. 식의 연산결과가 자동적으로 반환값이 된다.
  ```java
  (int a, int b) ->{return a>b ? a:b 
  //람다
  (int a, int b) -> a>b? a:b}
  ```
  - 람다식에 선언된 매개변수의 타입은 추론이 가능한 경우는 생략할 수 있다. (대부분의 경우에 생략가능하다)
  ```java
  (int a, int b) -> a:b?a>b 
  //매개변수타입 생략
  (a,b) -> a>b ?a:b
  ```

  - 매개변수가 하나뿐인 경우에는 괄호()를 생략할 수 있다.(매개변수의 타입이 있으면 괄호를 생략할 수 없다)
  ```java
  (a) -> a* a 
  //() 생략
  a -> a*a
  ```

  - 괄호{} 안의 문장이 하나일 때는 {}를 생략할 수있다. 
  ```java
  (String name,int i) ->{System.out.println(name+"=" +i)}
  //{} 생략
  (name, i )->System.out.println(name+"="+i)
  ```


- 메서드의 매개변수로 전달되어지는 것이 가능하고, 메서드의 결과로 반환될 수도 있다.  (메서드를 다른 메서드의 파라미터로 전달할 수 있다.)
- 메서드와 함수의 차이
  - 메서드 : 특정 클래스에 반드시 속해야 한다는 제약이있음 
  - 함수는 클래스에 속하지 않아도됨



```java

public class Expreesion_class {
    public static void main(String[] args) {

        //1번
        MyFunction f = (int a, int b) -> a>b? a:b;
        int big = f.max(5,3);
        System.out.println("f.max == "  + big);


        /**
         * sort(list, comparator을 람다식으로 표현하는방법)
         *
         */
        List<String> list = Arrays.asList("a1","aaa","b2b2b2","c2c2c2");
        Collections.sort(list,(s1,s2)-> s2.compareTo(s1));
        System.out.println("람다식 적용list = " + list);

        
        Collections.sort(list, new Comparator<String>() {
            @Override
            public int compare(String s, String t1) {
                return s.compareTo(t1);
            }
        });
        System.out.println("sort 람다식 적용  x list " +list);


    }
}
```
- ## 함수형 인터페이스
- 람다식은 익명 클래스의 객체와 동등하다
```java
/**
 * 하나의 메서드가 선언된 인터페이스를 정의해서 람다식을 다루는것을 함수형 인터페이스라고 한다.
 * 오직 하나의 추상 메서드만 정의되어 있어야만 하는 제약이 있다.
 * static메서드와 default 메서드의 개수에는 제약이 없다.
 * @FunctionallInterface 붙이기
 */
interface MyFunction{
    public abstract int max(int a, int b);
}
```
-  인터페이스를 구현한 익명 클래스의 객체
```java
Myfunction f = new Myfunction(){
  public int max(int a, int b){
    return a>b ? a:b;
  }
};

int big = f.max(5,3); // 익명 객체의 메서드를 호출
```
- 익명 객체를 람다식으로 변경
```java
Myfunction f = (int a, int b) -> a>b?a:b
int big = f.max(5,3); 

// 람다식도 실제로는 익명 객체이고, Myfunction인터페이스를 구현한 익명 객체의 메서드max()와 람다식의 매개변수의 타입과 개수 그리고 반환값이 일치하기떄문에 람다식으로 max메서드를 대체할 수 있다.
```

---

## java.util.Optional <T> 클래스

- Integger나 Double 클래스 처럼 T타입의 객체를 포장해 주는 래퍼 클래스 -> __모든 타입의 참조 변수를 저장할 수 있다.__

- Optional.of 를 사용하면 객체가없을 경우  NPE가 터집니다.
```java
Optional<String> testString = Optinal.of("");
// NPE 발생 

```

- Optional 객체를 사용하면 예상치 못한 NullPointerException 예외를 제공되는 메소드로 간단히 회피  __ofNullable() 메소드__


```java
Optional<String> name = Optional.ofNullable("이름을 명시 안하면 비어있는 Optional객체를 반환 " )
```
- Optiona.else()


- 초기화
```java
Optional<String>optVal = Optional.<String>empty();
```

- 값 가져오기
```java
Optional<String> optVal= Optional.of("abc");
String str1 = optVal.get(); // optVal에 저장된 값을 반환. null이면 예외 발생
String str1 = optVal.orElese("");  // null일 떄는 ""를 반환.
```

- collect() 
  - collect()는 매개변수의 타입이 Collector인데 매개변수가 Collector를 구현한 클래스의 객체이어야 한다. 이 객체에 구현된 방법대로 스트림의 요소를 수집한다.

  ```
  collect() : 스트림의 최종연산, 매개변수로 컬렉터를 필요로 한다.
  collector : 인터페이스, 컬렉터는 이 인터페이스를 구현해야 한다.
  collectors: 클래스,static 메서드로 미리 작성된 컬렉터를 제공한다.
  ```
  - 스트림을 컬렉션과 배열로 반환 - toList(), toSet(),toMap(),toCollection(), toArray()
    ```java
    List<String> names = stuStream.ma(Student::getName).collect(Collectors.toList());

    ```
- Optional 중간처리
  - filter 
    - predicate값이 참이면 해당 필터를 통과시키면 거짓이면 통과시키지 않습니다.
    ```java
    Optional.of("True").filter( (val) -> "True".equals(val))orElse("NoData"); // True
    Optional.of("False").filate((val) ->"True".equals(val))orElse("NoDate"); //Nodate
    ```

  - Map
    - mapper 함수를 통해 입력값을 다른 값으로 변환하는 메서드
    ```java
    Integer test = Optional.of("1").map(Integer::valueOf).elseOf(NoSuchExcepiton::NEW);
    ```

- 최종연산
  -  orElse
      - 최종적으로 연산을 끝낸 후에도 옵셔널 객체가 비어있다면 기본값으로 제공할 객체를 지정합니다.
      ```java
      String result = Optional.ofNullable(null).orElse("default");
      // result = default;
      ```
  - orElseThrow
    - 최종적으로 연산을 끝낸 후에도 옵셔널 객체가 비어있다면 예외 공급자 함수를 통해 예뢰를 발생시킬 수 있습니다.
    ```java
    Optional.ofNullable(null).filter((val)-> "test".equals(val))orElseThrow(???Exception::NEW);
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


## 메서드체이닝

-  OOP(객체지향프로그래밍)에서 여러 메소드를 이어서 호출하는 방법
- 메소드가 객체를 반환하여 여러 메소드를 순차적으로 선언할 수 있도록 한다.
- 메소드 체이닝을 이용하면 코드가 간결해져 하나의 문장처럼 읽히게 할 수 있단느장점이 있고, 장기적으로 유지보수에 도움이 된다.



## 문자열데이터를 Date 객체로 바꾸어 시간차 계산하기


- LocalDate(날짜 정보만 필요할 떄 사용)
  - LocalDate currentDate = LocalDate.now(); // 2021-09-21

- LocalTime
  - 로컬 시간 클래스로 시간 정보만 필요할 때 사용
  - LocalTime currentTime = LocalTime.now(); // 19:41:21

- LocalDateTime(날짜와 시간 정보 모두가 필요할 때 사용)
  - LocalDateTime currentDateTiem= LocalDateTime.now();//2021-09-21T19:34:30.399

- 시간비교하기
```java
public 시간비교하기(){
  LocalTime startTime = LocalTime.now();
  LocalTime endTime = LocalTime.of(23.59.59);

  startTime.isBefore(endTime);//True

  startTime.isAfter(endtime); //false
}

// https://choi-dev.tistory.com/80  참고하기
```


- 시간차이 구하기

```java
//"yyyy-mm-dd" 로 나올경우
public test(String yymmdd){
  
    DateTimeFormatter formatter = DateTimeFormatter.ofPattern("yyyy-MM-dd");

    LocalDateTime date = LocalDateTime.parese(yymmdd,foramtter);
    LocalDateTime now = LocalDateTime.of(LocalDate.parse(endDate, DateTimeFormatter.ISO_DATE), LocalTime.of(23,59,59));
    ;
    LocalDateTime returnTime = .truncatedTo(ChronoUnit.MINUTES);

```