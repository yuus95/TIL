# Lang 패키지


- lang패키지는 자바프로그래밍에 가장 기본이 되는 클래스들을 포함하고 있다.


## Object 클래스

- Object클래스는 모든 클래스의 최고 조상이기 떄문에 Object클래스의 멤버들은 모든 클래스에서 바로 사용 가능하다.

- 멤버변수는 없고 오직 11개의 메서드만 가지고 있다. 
```java
proteted Object clone //객체 자신의 복사본을 반환한다.

public boolean equals(Object obj) // 객체 자신과 개체 obj가 같은 객체인지 알려준다.

```

### equals(Object obj)
- 매개변수로 객체의 참조변수를 받아서 비교하여 그 결과를 boolean값으로 알려 주는 역할을 한다. 
    -  '==' 과 Object.eqauls는 참조변수를 이용하여 비교
```java
// object 클래스의 equals
public boolean equals(Object obj){
    return (this==obj);
}
```

- 주소가 아닌 객체에 저장된 내용을 비교하도록 변경하기 위한 객체별 오버라이딩

```java
// Person의 id값 비교를 위한 오버라이딩
class Person{
    long id;

    public boolean equals(Object obj){
        if(obj instanceof Person){
            return id == ((Person)obj).id;
        else{
            return false;
        }
        }
    }
}
```

### HashCode()

- 해싱(hashing)기법에 사용되는 해시함수를 구현한 것이다.
- 해싱은 데이터관리기법 중의 하나인데 다량의 데이터를 저장하고 검색하는 데 유용하다.

- Object클래스에 정의된 hashCode메서드는 객체의 주소값으로 해시코드를 만들어 반환한다.

- 해싱기법을 사용하는 HashMap이나 HashSet과 같은 클래스에 저장할 객체라면 반드시 hashCode메서드를 오버라이딩해야 한다. 
    - String클래스는 문자열의 내용이 같으먼 동일한 해시코드를 반환하도록 오버라이딩 되어있다.



### toString()

- 인스턴스에 대한 정보를 문자열로 제공할 목적으로 정의한 것이다.

```java
//Object에 정의된 문자열
public String toString(){
    return getClass.getName()+"@"+Integer.toHexString(hashCode());
}

// 클래스이름에 16진수의 해시코드를 얻게 된다.
```

- toString 오버라이딩

```java
public class test2 {
    public static void main(String[] args){

        Person person = new Person();
        System.out.println(person.toString());
    }
}

class Person{
    String name;
    String phone;

    public Person(){
        this("유신","01047102222");
    }
    public Person(String name, String phone){
        this.name = name;
        this.phone = phone;
    }

    public String toString(){
        return "name "+ name +"Phone " + phone;
    }
}

// 결과
// name 유신Phone 01047102222
```

### clone()

- 자신을 복제하여 새로운 인스턴스를 생성하는 일을 한다. 
- 어떤 인스턴스에 대해 작업을 할 때, 원래의 인스턴스는 보존하고 clone메서드를 이용해서 새로운 인스턴스를 생성하여 작업을 하면 <br> 작업 이전의 값이 보존되므로 작업에 실패해서 원래의 상태로 되돌리거나 변경 되기 전의 값을 참고하는데 도움이 된다.

- Object에 정의된 clone()은 단순히 인스턴스 변수의 값만 복사하기 때문에 <br> 참조타입의 인스턴스 변수가 있는 클래스는 완전히 인스턴스 복제가 이루어지지 않는다.
    - 배열의 경우, 복제된 인스턴스도 같은 배열의 주솔르 갖기 때문에 복제된 인스턴스의 작업이 원래의 인스턴스에 영향을 미치게 된다.  <br> 이런 경우 clone메서드를 오버라이딩해서 새로운 배열을 생성하고 배열의 내용을 복사하도록 해야 한다.


- clone() 오버라이딩

```java

public class clone {
    public static void main(String[] args) {
        Point origin = new Point();
        Point copy = (Point)origin.clone(); // 복제해서 새로운 객체 생성

        System.out.println("origin" +origin);
        System.out.println("copy" +copy);

    }
}

//Cloenable를 구현해야만 clone을 쓸 수 있다.  
// 데이터를 보호하기 위해 인터페이스를 구현한 클래스만 복사할 수 있게 설정해놨다.
class Point implements Cloneable {
    int x, y;

    Point(int x, int y) {
        this.x = x;
        this.y = y;
    }

    Point() {
        this(3, 4);
    }

    public String toString(){
        return "x = " + x +" y = " + y;
    }

    public Object clone() {
        Object obj = null;
        try {
            obj = super.clone();
        } catch (CloneNotSupportedException e) {
        }
        return obj;
    }
}

```