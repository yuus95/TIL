# 이펙티브 자바

## 1. 생성자 대신 정적 팩터리 메서드를 고려하라

클래스는 클라이언트에 public 생성자 대신 정적 팩터리 메소드를 제공할 수 있다.

- 장점
    - 이름을 가질 수 있다.
        - 생성자에 넘기는 매개변수와 생성자 자체만으로는 반환될 객체의 특성을 제대로 설명하지 못한다. 반면 정적 팩터리는 이름만 잘 지으면 반환될 객체의 특성을 쉽게 묘사할 수 있다.
        ```java
        BigInteger(int,int,Random)
        
        BigInteger.probablePrime

        // 둘 다 같은 기능이지만 probablePrime이 좀 더 확 와닿는다.
        // 값이 소수인 BigInteger를 반환하는 함수
        ```

    - 하나의 시그니처로는 생성자를 하나만 만들 수 있지만 정적 팩터리 메서드에는 이런 제약이 없다. 한 클래스에 시그니처가 같은 생성자가 여러 개 필요할 것 같으면, 생성자를 정적 팩터리 메서드로 바꾸고 각각의 차이를 잘 드러내는 이름을 지어낼 수 있다.

    - 호출될 때마다 인스턴스를 새로 생성하지 않아도 된다.
        - 이 덕분에 불변 클래스는 인스턴스를 미리 만들어 놓거나 새로 생성한 인스턴스를 캐싱하여 재활용하는 식으로 불필요한 객체 생성을 피할 수 있다. -> 플라이웨이트 패턴과 비슷

    - 반환 타입의 하위 타입 객체를 반환할 수 있는 능력이 있다.
        - 반환할 객체의 클래스를 자유롭게 선택할 수 있게 하는 엄청난 유연성을 선물한다.

        - API 만들 때 이 유연성을 응용하면 구현 클래스를 공개하지 않고도 그 객체를 반환할 수 있어 API를 작게 유지할 수 있다.

    - 입력 매개변수에 따라 매번 다른 클래스의 객체를 반환할 수 있다. 
        - 반환 타입의 하위 타입이기만 하면 어떤 클래스의 객체를 반환하든 상관없다.

    - 정적 팩터리 메서드를 작성하는 시점에는 반환할 객체의 클래스가 존재하지 않아도 된다.
        - 이런 유연함은 서비스 제공자 프레임워크를 만드는 근간이 된다.
        
        - ex) 대표적인 서비스 제공자 프레임워크 JDBC 
            - 서비스 제공자 프레임워크에서의 제공자는 서비스의 구현체다 그리고 이 구현체들을 클라이언트에 제공하는 역할을 프레임워크가 통제하여 클라이언트를 구현체로부터 분리해준다.

    
    - 단점
        - 상속을 하려면 public이나 protected생성자가 필요하니 정적 팩터리 메서드만 제공하면 하위 클래스를 만들 수 없다.
            - 이 제약은 상속보다 컴포지션을 사용하도록 유도하고 불변 타입으로 만들려면 이 제약을 지켜야 한다는 점에서 오히려 장점으로 받아 들일 수 있다.

            - 컴포지션?
                - 기존 구체 클래스를 확장하는 대신, 새로운 클래스에 private 필드로 구체 클래스의 인스턴스를 참조하자.
    
        - 정적 팩터리 메서드는 프로그래머가 찾기 어렵다.

    - 자주 사용하는 정적 팩터리 메서드
        - from : 매개변수를 하나 받아서 해당 타입의 인스턴스를 반환하는 형변환 메서드
        ```java
        Date d = Date.from(instant);
        ```
        
        - of: 여러 매개변수를 받아 적합한 타입의 인스턴스를 반환하는 메서드
        ```java
        Set<Rank> faceCards = EnumSet.of(JACK,QUEEN, KING);
        ```

        - valueOf: from과 of의 더 자세한 버전
        ```java
        BigInteger  prime = BigInter.valueOf(Interger.MAX_VALUE);
        ```

        - instnace, getInstance(매개변수를 받는다면) 매개변수로 명시한 인스턴스를 반환하지만 같은 인스턴스임을 보장하지 않는다.
        ```java
        StackWalker luke = StackWalker.getInstance(options);
        ```

        - create 혹은 newInstance : instance혹은 getInstance와 같지만 매번 새로운 인스턴스를 생성해 반환함을 보장한다.
        ```java
        Object newArray = Array.newInstance(classObject,arrayLen);
        ```

        - getType : getInstance와 같으나, 생성할 클래스가 아닌 다른 클래스에 팩터리 메서드를 정의할 때  쓴다."Type"은 팩터리 메서드가 반환할 객체의 타입이다. 
        ```java
        FileStore fs = Files.getFilestor(path)
        ```

        - newType: newInstance와 같으나 생성할 클래스가 아닌 다른 클래스에 팩터리 메서드를 정의할 때 쓴다. Type은 팩터리 메서드가 반환할 객체의 타입이다.
        ```java
        BufferedReader br = Files.newBufferedReader(path);
        ```

        - type: getYpe과 newType의 간결한 버전
        ```java
        List<Complaint> litany = Collections.list(legacyLitany);
        ```

        - 요약
        ```
        정적 팩터리 메서드와 public 생성자는 각자의 쓰임새가 있으니 상대적인 장단점을 이해하고 사용하는 것이 좋다. 그렇다고 하더라도 정적팩터리를 사용하는 게 유리한 경우가 더 많으므로 무작정 public 생성자를 제공하던 습관이 있다면 고치자.
        ```

        